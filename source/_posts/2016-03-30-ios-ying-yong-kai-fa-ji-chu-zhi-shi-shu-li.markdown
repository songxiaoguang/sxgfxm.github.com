---
layout: post
title: "iOS - 应用开发基础知识梳理"
date: 2016-03-30 08:38:24 +0800
comments: true
categories: iOS
tags: 
keywords: iOS
---


这篇文章主要自己对iOS应用开发基础知识的梳理，用于基础知识的复习巩固，也可作为笔试、面试的资料。本人作为iOS开发初学者，能力有限，如果不足之处请多多包涵和在评论指出，一定及时更新回复。

##目录

- Objective-C语言相关
- Objective-C语法相关

##Objective-C语言相关

- 面向对象编程特点
	- 封装：把数据及操作这些数据的方法封装为类，只对外暴露接口使用；
	- 继承：子类可以继承父类的变量和方法，提高代码重用性，单继承特性；
	- 多态：子类对象可以赋值给父类对象；不同对象对相同的消息做出不同的响应。

---

- 动态语言特性
	- 动态类型定义：在代码中对象定义为id类型，直到运行时才确定类型；
	- 动态内存分配：在运行时，类创建新对象；
	- 动态消息绑定：在运行时，匹配消息中的selector与receiver中的方法实现。

---

- 动态类型定义
	- 使用id类型定义的对象，在运行时才知道该对象属于哪个类；
	- 运行时系统会查询该对象的isa指针，从而知道它属于哪个类；
	- 动态类型定义是动态绑定的基础。

---

- 动态消息绑定
	- 对message做出反应的method直到程序运行时才能确定；
	- function和parameters在编译时就被绑定到一起；
	- receiver和message直到程序运行且message被发出时才被绑定到一起；
	- 动态消息绑定使每个对象都可以有某一方法的个性化版本；
	- 通过改变receiver，且receiver运行时才能确定，从而同一message产生不同的结果；
	- receiver的确定受用户操作影响；
	- 消息发送者不必在意谁接收该消息，不同的接收方会采取个性化操作；
	- 消息本身也可以动态确定。

---

- 消息传递机制
	- 给某个对象发送一个消息，该对象根据消息采取特定的操作。
	- 发出消息——>运行时消息处理例程获取receiver和消息中的selector  
	——>查找接收器中的同名方法——>把接收器的实例变量传递给这个方法——>调用该方法。

---

- 静态语言特性
	- 静态类型定义：使用类名直接定义对象，增加给编译器和读者的信息量；
	- 动态内存分配：并不影响该对象在运行时如何创建；
	- 动态消息绑定：并不影响该对象接受消息的方式。

---

- 静态类型定义
	- 直接使用类名定义的对象，在编译时即知道该对象属于哪个类；
	- 静态类型定义的对象，编译器可以对其进行类型检查；
	- 可静态定义某对象为其父类对象。

---

- 动态行为与静态行为比较
	- 动态行为的优点：使面向对象编程更加灵活，强大。
	- 动态行为的缺点：无法进行类型检查，运行时容易出错；消息传递比函数调用慢。
	- 静态行为的优点
		- 允许编译时进行类型检查：
			- 当向一个静态定义的receiver发送消息时，编译器可检查其是否可以响应；
			- 当静态定义的对象赋值给一个静态定义的变量时，编译器可检查类型是否一致。
		- 打破同名方法需要相同的返回值和参数类型的限制：
			通常，不同类的同名方法需要有相同的返回值和参数类型，来使编译器允许动态绑定，因为编译器不知道接收器属于哪个类，所以只创建一个方法描述。当接收器为静态定义的对象时，编译器在编译时就知道其所属类的信息，可以为其单独创建方法描述，从而打破同名限制。
		- 允许使用->来访问实例变量。

---

- 协议Protocol
	- 协议声明了一组方法，这组方法可以被所有类实现；
	- 遵守某协议需要实现该协议中@required修饰的方法；
	- 使用协议的三种情况：
		- 声明想让其他类实现的方法（代理设计模式时采用）；
		- 声明interface并隐藏它的类；
		- 实现类之间共享某些方法。
	- 协议是独立存在的，协议的创建不需要与任何类建立联系；
	- 协议声明了一组方法，并不与类建立连接，把方法从类的结构中解放出来；
	- 协议不关心具体哪个类实现了自己，只关心这个类是否完整实现了自己声明的所有需要实现的方法；
	- 协议使实现该协议的类具有相似的方法集，不需要有继承关系；
	- 对象可以按类划分，也可以按协议划分；
	- 继承：向类中添加父类中的方法；
	- 协议：向类中添加协议中的方法；
	- 正式协议：
	- 非正式协议：

---

- 类别Category
	- 类别可以实现在不修改源代码的情况下，为类添加方法；
	- 类别一般只可以添加方法，可以通过Runtime实现添加属性；
	- 类别扩充了类的功能，而不必通过子类来实现，且添加的方法与类中的方法地位相同；
	- 类别可以把类的实现放在不同的文件中；
	- 如果扩展的方法与原始类中的方法相同，则会隐藏原始方法；
	- 不可在扩展方法中通过super调用原始方法；

---

- 扩展Extention
	- 扩展可以在源代码的基础上，添加方法和属性；
	- 扩展允许一个类拥有一个私有的interface，且可由编译器验证。

---

- 内存管理机制
	- Reference Count，引用计数。通过对象的引用计数来判断是否释放该对象，当对象的引用计数为0时，系统会调用dealloc方法来释放该对象；
	- retain，retainCount++；release，retainCount--；
	- 黄金法则：通过alloc、new、copy获取的对象，需要对其使用release或autorelease释放。
	- MRC，手动内存管理，
	- ARC，自动内存管理，iOS 5.0后采用，编译时自动插入retain、release和autorelease，  
	减轻开发者负担，但只针对Foundation框架下的对象；
	- GC：垃圾回收机制，运行时周期性的检查是否有不再使用的对象并进行释放，  
	iOS不支持垃圾回收机制。
	- 非ARC工程中采用ARC去编译某些类：-fobjc-arc；
	- 在ARC工程中采用MRC去编译某些类：-fno-fobjc-arc;

---

- 属性Property
	- 提供了一种清晰、准确的描述，accessor方法是如何工作的；
	- 编译器可以自动生成访问实例变量的方法，减少代码量；
	- @synthesize告诉编译器实现响应的accessor方法，@dynamic动态实现；
	- 属性修饰词：
		- 设置accessor方法的名字
			- getter = getterName
			- setter = setterName
			- 默认getter = propertyName，setter = setPropertyName：
		- 设置property的可写性
			- readwrite：可读可写，默认；
			- readonly：只可读，不会生成setter方法；
		- 设置proper的原子性
			- atomic：原子的，保证访问时线程安全，开销大；
			- nonatomic：非原子的，当确定该属性不会多线程访问时采用，节省开销；
		- 设置setter方法的语义
			- assign：常用于修饰基本数据类型，默认；
			- copy：当该对象被拷贝时，向该对象发送release消息；
			- retain：赋值时先release旧值，再retain新值，常用语对象；
			- strong：与目标对象有strong relationship
			- weak：与目标对象有weak relationship。当目标对象被释放时，该属性值自动置nil；
		
---

- 强引用与弱引用  
	我们已经知道OC中的内存管理是通过“引用计数”来实现的。一个对象的生命周期取决于它是否被其他对象引用（是否retainCount == 0）。但当出现强引用循环时，将造成对象无法释放。

	- 强引用：当前对象被其他对象引用时，会执行retain操作，retainCount++。当retainCount == 0时，该对象才会被销毁。强引用持有该对象，需要负责该对象的内存管理，这是默认的引用方式。
	- 弱引用：不改变引用计数，为解决强引用循环而引入。弱引用不持有该对象，不需要负责对该对象的内存管理，当弱引用指向的对象被释放时，自动置为nil。         
	
	定义属性时，若声明为retain类型，则为强引用；若声明为assign类型的，则为弱引用。在ARC模式下，强引用声明为strong；弱引用声明为weak。
	retain和strong一致；assign和weak基本一致，同为弱引用，但weak为“归零弱引用”，即当对象被销毁后，会自动把它的指针置为nil，这样可以防止野指针错误。而assign不会自动置为nil，造成了野指针。   
	总结：由于要进行内存管理，OC里的引用默认都是强引用；为了解决“强引用循环”造成的内存泄露，引入了弱引用。   
	参考[关于iOS的强引用、弱引用](http://www.tuicool.com/articles/NVNrMv7)

--- 

- 深拷贝和浅拷贝
	- 深拷贝（mutableCopy）：内容拷贝，拷贝出相同大小的内存空间，内容一致；
	- 浅拷贝（copy）：指针拷贝，指针指向同一块内存空间。
	- 遵守NSCopying协议的类可以发送copy消息；
	- 遵守NSMutableCopying协议的类可以发送mutableCopy消息。

---

- retain和copy
	- copy属性会创建一个新对象，新对象的retainCount为1。copy减少对象对上下文的依赖；
	- retain属性会创建一个指针指向该对象，对象的retainCount加1.
	- retain表示两个对象的地址相同，属于指针拷贝；
	- copy会开辟新的内存空间，属于内容拷贝。

---

- KVC与KVO
	- KVC，键值编码，是一种通过KeyPath间接访问对象属性的途径，多数情况下用于简化代码；
	- KVC的执行时要解析KeyPath字符串，执行效率低于accessor方法；
	- KVO，简直观察机制，可以监控对象的属性变化，简化代码；
	- KVC是KVO的基础。
	- KVO的使用过程：
		- 注册观察者，`addObserver:forKeyPath:options:context:`
		- 接收变更通知，`observeValueForKeyPathOfObject:change:context:`
		- 移除观察者，`removeObserver:forKeyPath:`

---

- NSNotificationCenter
	- 通知中心，用于实现较大跨度的界面通信，可以为无引用关系的对象进行通信，使用了观察者模式；
	- 注册通知， `[[NSNotificationCenter defaultCenter]  addObserver:self selector:@selector(mytest:) name:@" mytest" object:nil];`
	- 发送通知，`[[NSNotificationCenter defaultCenter] postNotificationName:@"mytest"　object:searchFriendArray];`
	- 响应代理方法
	- 移除通知，`[[NSNotificationCenter defaultCenter] removeObserver:self name:@"mytest" object:nil];`
	- 键盘弹起收起通知  
	
			//键盘升起   
			[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillShow:) name:UIKeyboardWillShowNotification object:nil];  
			//键盘降下  
			[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(keyboardWillHide:) name:UIKeyboardWillHideNotification object:nil];

---

- Block
	- block是一个代码块，block名为指向这个代码块的指针，通过block名调用代码块。
	- 作用：
		- 对一个对象的集合的操作（`sortedArrayUsingComparator:`）
		- 在完成操作时回调，把操作的结果传回，可直接访问局部变量
	- 默认block里面引用一个OC对象时，该对象会被retain。
	- __block修饰的对象变量不会被retain；
	- __block修饰的block外的变量，变传值为传址，可以在block内修改；
	- 为防止内部循环引用，使用__weak修饰；
	- 根据isa指针，block一共有三种类型：
		- GlobalBlock，全局静态
		- StackBlock，保存在栈中，出函数作用域销毁，系统自动管理
		- MallocBlcok，保存在堆中，程序员手动管理

---

- 几种传值方式比较
	- 代理需要对象间有引用关系，通常一对一；通知中心耦合度更加松散，属于一对多的广播；
	- 代理逻辑更加清晰，容易理解；block代码简洁，使用方便；
	- 单例有时可用来共享信息；

---

- 数据持久化方式：
	- 属性列表：只有NSString、NSArray、NSDictionary和NSData可`writeToFile`，储存到plist文件；
	- 对象归档：将对象序列化为二进制流
		- 归档，`NSKeyedArchiver`
		- 解档，`NSKeyedUnArchiver`
		- 自定义对象需要遵守NSCoding协议，并实现归档和解档方法`encodeWithCoder:`和`initWithCoder:`
	- NSUserDefault
	- 沙盒：iOS引用程序独立的存储空间，保存较大的数据
		- Documents：保存数据，iTunes会备份此目录；
		- Library/Preference：存储程序的默认设置；
		- Library/Caches：存放缓存文件，iTunes不会备份此目录；
		- tmp：临时文件保存目录，程序退出时会清空
	- SQLite数据库：轻量级的数据库
	- CoreData：通过管理对象进行增删改查操作。它不是一个数据库。

---

- 多线程编程
	- 使用场景：当需要执行某些耗时操作如网络请求、图片加载、下载文件等操作时，开启异步线程执行，避免界面卡死；
	- NSThread，轻量级线程，需要控制线程的创建、开启、取消等操作。通常使用`[NSThread sleepForTimeInterval:]`使线程休眠；
	- NSOperation及NSOperationqueue，任务和队列，创建任务，加入队列，开始执行；
	- GCD，grand central dispatch，充分利用多核处理器的并行特性的C方法，无需关心线程创建、调度和销毁等，常用语创建单例，异步执行等。
	- 多线程编程需要注意**线程安全**；
	- 刷新UI需要回到主线程

---

- Runtime
	- Runtime，运行时系统，Objective-C的动态语言特性需要Runtime来支持，可以在程序运行时动态创建、检查、修改类、对象及方法；
	- 动态创建类：
		- 分配空间`objc_allocateClassPair`
		- 添加成员变量`class_addIvar`
		- 添加属性`class_addProperty`
		- 添加方法`class_addMethod`
		- 注册类`objc_registerClassPair`
	- 动态添加变量：无法向已经存在的类中添加变量
	- 动态添加方法：
		- 添加方法`class_addMethod(Class,SEL,IMP,String)`
		- 方法实现`void(id self, SEL _cmd)`
	- 动态替换方法的实现：
		- 获取方法`class_getInstanceMethod`
		- 交换方法`method_exchangeImplementation`
	- 动态创建对象：
		- 创建对象`class_createInstance`
	- 动态获取实例变量的值
		- 获取实例变量`object_getIvar`
	- 动态修改实例变量的值
		- 获取变量列表`class_copyIvarList`
		- 遍历列表获取变量`ivar_getName`
		- 修改对应变量`object_setIvar`
	- 为类别添加属性：
		- 声明名称`char cName`
		- 实现setter方法`objc_setAssociatedObject`
		- 实现getter方法`objc_getAssociatedObejct`

---

- RunLoop
	- RunLoop的关键作用：
		- 保持线程事件、消息处理能力，直到线程退出；
		- 当无事件、消息处理时，休眠以降低开销；当有消息时，及时响应。
	- 线程和RunLoop是**一一对应**的，其关系保存在一个全局的Dictionary里；
	- 线程创建时并没有RunLoop（符合一般情况下线程的状态），只有当调用`CFRunLoopGetMain()`或`CFRunLoopGetCurrent()`主动获取，且只在第一次获取时创建RunLoop，并加入全局Dictionary里；
	- 初始化全局Dictionary时，会为主线程创建一个RunLoop。即主线程默认开启RunLoop（符合App启动后可以保持事件处理能力）；
	- 当线程结束时，会销毁该线程的RunLoop；
	- RunLoop的结构：
		- 一个**RunLoop**包含若干个**Mode**；
		- 每个Mode又包含若干个**Source/Timer/Observer**；
		- 每次调用RunLoop的主函数时，只能指定其中一个Mode，这个Mode被称作**CurrentMode**；
		- 如果需要切换Mode，只能退出Loop，再重新指定一个Mode进入；
		- **CFRunLoopSourceRef**是事件产生的地方：
			- **Source0**，只包含一个回调指针；
			- **Source1**，包含一个**mach_port**和一个回调指针；
		- **CFRunLoopTimerRef**是基于时间的触发器；
		- **CFRunLoopObserveRef**是观察者；
	- Mode的结构：
		- **CommonModes**
		- **CommonModeItems**
		- **CurrentMode**
		- **Modes**

---

- 浅谈设计模式
	- 代理模式：当一个类的某些功能需要其他类来实现时采用，如生产者和经销商，界面传值等；
	- 单例模式：一个类只有一个实例变量，用于资源共享控制；
	- 策略模式：定义算法族，封装起来，使其可相互替换，如数组排序；
	- 观察者模式：负责监听并发布消息，如通知中心；
	- 工厂模式：工厂方式创建类的实例，多与代理模式配合。

---









