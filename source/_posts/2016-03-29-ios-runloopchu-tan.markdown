---
layout: post
title: "iOS - RunLoop初探"
date: 2016-03-29 19:39:29 +0800
comments: true
categories: iOS
tags: 
keywords: iOS,Runloop
---

###目录   

- Runloop的概念
- Runloop与线程的关系
- Runloop对外的接口
- Runloop的Mode
- Runloop的内部逻辑
- Runloop的底层实现
- 基于Runloop的功能
	- AutoreleasePool
	- 事件响应
	- 手势识别
	- 界面更新
	- 定时器
	- PerformSelecter
	- 关于GCD
	- 关于网络请求
	- AFNetworking

###RunLoop的概念

Runloop，简单来讲就是一个“运行着的循环”，而这个循环的目的是保持事件处理能力，不会执行完一个事件就退出。比如不规则的循环处理，一个保持运行状态的App，能够随时对用户的操作做出响应，并在无用户操作的情况下休眠以降低开销；再如规则的循环处理，开启的计时器NSTimer，周期性的响应某一事件，直到定时器销毁。   

对Runloop的理解需要把握两个关键点：   
- 保持对事件、消息的响应能力，直到退出；  
- 当无事件、消息需要处理时，休眠以降低开销；

贴近编程来讲，当我们开启一个线程执行某一任务，当该任务执行完毕后，线程会退出销毁。  
如果想让线程能随时执行任务并不退出，就需要上面谈到的“运行着的循环”Runloop来实现，  
通常的代码逻辑如下：  

	function loop() {
    	initialize();
    	do {
        	var message = get_next_message();
        	process_message(message);
    	} while (message != quit);
	}

这种模型通常被称作**Event Loop**。实现这种模型的关键点在于：  
- 如何管理事件、消息；  
- 如何让线程在没有处理消息时休眠以避免资源占用，在有消息到来时立刻被唤醒。

所以，Runloop实际就是一个对象，这个对象管理了其需要处理的事件和消息，并提供了一个入口函数来执行上面Event Loop的逻辑。  
线程执行了这个函数后，就会一直处于这个函数内部“接受消息->等待->处理”的循环中，直到循环结束（比如传入quit消息），函数返回。

OS X/iOS 系统中，提供了两个这样的对象：**NSRunLoop**和**CFRunLoopRef**。  

CFRunLoopRef是在CoreFundation框架内的，它提供了纯C函数的API，所有这些API都是线程安全的。  

NSRunLoop是基于CFRunLoop的封装，提供了面向对象的API，但这些API不是线程安全的。   

###RunLoop与线程的关系

通过Runloop的引入，我们很容易发现RunLoop与线程是密不可分的，Runloop是为线程服务的，使线程拥有持续响应事件的能力并在无事件时休眠。下面来看Runloop与线程到底什么关系。  

苹果不允许直接创建RunLoop，它只提供了两个自动获取的函数：CFRunLoopGetMain()和CFRunLoopGetCurrent()。这两个函数内部的逻辑大概如下：  

	/// 全局的Dictionary，key 是 pthread_t， value 是 CFRunLoopRef
	static CFMutableDictionaryRef loopsDic;
	/// 访问 loopsDic 时的锁
	static CFSpinLock_t loopsLock;
  
	/// 获取一个 pthread 对应的 RunLoop。
	CFRunLoopRef _CFRunLoopGet(pthread_t thread) {
    	OSSpinLockLock(&loopsLock);
     
    	if (!loopsDic) {
        	// 第一次进入时，初始化全局Dic，并先为主线程创建一个 RunLoop。
        	loopsDic = CFDictionaryCreateMutable();
        	CFRunLoopRef mainLoop = _CFRunLoopCreate();
        	CFDictionarySetValue(loopsDic, pthread_main_thread_np(), mainLoop);
    	}
     
    	/// 直接从 Dictionary 里获取。
    	CFRunLoopRef loop = CFDictionaryGetValue(loopsDic, thread));
     
    	if (!loop) {
        	/// 取不到时，创建一个
        	loop = _CFRunLoopCreate();
        	CFDictionarySetValue(loopsDic, thread, loop);
        	/// 注册一个回调，当线程销毁时，顺便也销毁其对应的 RunLoop。
        	_CFSetTSD(..., thread, loop, __CFFinalizeRunLoop);
    	}
     
    	OSSpinLockUnLock(&loopsLock);
    	return loop;
	}
  
	CFRunLoopRef CFRunLoopGetMain() {
    	return _CFRunLoopGet(pthread_main_thread_np());
	}
  
	CFRunLoopRef CFRunLoopGetCurrent() {
    	return _CFRunLoopGet(pthread_self());
	}

从上面代码可以看出：

1. 线程和RunLoop之间是**一一对应**的，其关系保存在一个全局的Dictionary里。  

2. 线程创建时并没有RunLoop（符合一般情况下线程的状态），只有当调用上述两个接口主动获取，且只在第一次获取时创建RunLoop，并加入到全局的Dictionary里。

3. 初始化全局Dictionary时，会为主线程创建一个RunLoop。  
即主线程默认开启Runloop（符合App启动后可以保持事件处理能力）。

4. 当线程结束时，会销毁该线程的RunLoop。

###RunLoop对外的接口

在CoreFoundation里关于RunLoop有5个类：  
 
+ CFRunLoopRef  
+ CFRunLoopModeRef  
+ CFRunLoopSourceRef  
+ CFRunLoopTimerRef  
+ CFRunLoopObserverRef  

其中CFRunLoopModeRef类并没有对外暴露，只是通过CFRunLoopRef的接口进行了封装，它们的关系如下：   

![](http://cc.cocimg.com/api/uploads/20150528/1432798883604537.png)

- 一个RunLoop包含若干个**Mode**；  

- 每个Mode又包含若干个**Source**/**Observer**/**Timer**；

- 每次调用RunLoop的主函数时，只能指定其中一个Mode，这个Mode被称作**CurrentMode**；

- 如果需要切换Mode，只能退出Loop，再重新指定一个Mode进入。这样做主要是为了分割开不同组的Source/Observer/Timer，让其互不影响。


**CFRunLoopSourceRef**是事件产生的地方。Source有两个版本：  

+ **Source0**只包含了一个回调指针，它并不能主动出发事件。使用时需要先调用CFRunLoopSourceSignal(source)，将这个 Source 标记为待处理，然后手动调用 CFRunLoopWakeUp(runloop) 来唤醒 RunLoop，让其处理这个事件。

+ **Source1**包含了一个**mach_port**和一个回调指针，被用于通过内核和其他线程相互发送消息。这种Source能主动唤醒RunLoop的线程。  

**CFRunLoopTimerRef**是基于时间的触发器，它和 NSTimer 是toll-free bridged 的，可以混用。其包含一个时间长度和一个回调（函数指针）。当其加入到 RunLoop 时，RunLoop会注册对应的时间点，当时间点到时，RunLoop会被唤醒以执行那个回调。  

**CFRunLoopObserveRef**是观察者，每个 Observer 都包含了一个回调（函数指针），当 RunLoop 的状态发生变化时，观察者就能通过回调接受到这个变化。可以观测的时间点有以下几个：  

	typedef CF_OPTIONS(CFOptionFlags, CFRunLoopActivity) {
		kCFRunLoopEntry         = (1UL << 0), // 即将进入Loop
		kCFRunLoopBeforeTimers  = (1UL << 1), // 即将处理 Timer
		kCFRunLoopBeforeSources = (1UL << 2), // 即将处理 Source
		kCFRunLoopBeforeWaiting = (1UL << 5), // 即将进入休眠
		kCFRunLoopAfterWaiting  = (1UL << 6), // 刚从休眠中唤醒
		kCFRunLoopExit          = (1UL << 7), // 即将退出Loop
	};

上面的Source/Timer/Observer被统称为**mode item**。一个item可以被同时加入多个mode，但一个item被重复加入同一个mode时是不会有效果的。如果一个mode中一个item都没有，则RunLoop会直接退出，不进入循环。

###RunLoop的Mode

