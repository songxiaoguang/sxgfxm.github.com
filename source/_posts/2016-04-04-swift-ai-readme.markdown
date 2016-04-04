---
layout: post
title: "Swift AI - README"
date: 2016-04-04 15:36:11 +0800
comments: true
categories: 机器学习
tags: 
keywords: 机器学习,神经网络
---

Swift AI 是一个完全由Swift语言编写的高性能的人工智能和机器学习库。目前支持iOS和OS X平台。  

###Features

Swift AI包含一系列关于机器学习和人工智能研究的工具。这些工具灵活、强大，可以为许多应用程序。Swift AI 包括以下内容：  

- 前馈神经网络
	- 三层自定义神经网络
	- iOS和OS X的实例工程
- 循环神经网络
- 卷积神经网络
- GPU加速的神经网络
- 遗传算法
- 快速矩阵库
	- 支持常用计算的矩阵类
	- SIMD加速的操作
- 傅里叶变换函数

###What is't for

> “这是一个很酷的项目，但我可以用它来做什么呢？我对AI一窍不通”

我经常收到这样的问题，所以我想在这里说明一下：  

Swift AI专注于人工智能的其中一个分支——机器学习：使计算机通过训练学会某项技能而不必通过具体的编程实现。通过适当的应用，这些工具可以赋予你的应用特殊的技能甚至超出现实的。  

举例来说，编写一个可以识别字母的应用。根据你在学校学习的关于计算机科学的知识，你可以会尝试写下识别每个字母的规则。这些从图片中提取的像素数据，逐个图读取它们，并通过复杂的数学计算模型识别这些字母。然后你的程序可能如下所示：  

	if /* massive function for checking the letter A */ {
    	return "A"
	} else if /* massive, completely unique function for checking the letter B */ { 
    	return "B"
	} else if ...

然而上述做法是不可取的。最理性的情况，你需要写上行不可靠的代码来识别且只能识别你自己的笔迹。相反的，Swift AI 的样例程序示范了如何使用机器学习来写很少的代码实现高级的功能。并且开发者不需要写特定的规则。  

> 那么Swift AI 在现实世界是如何使用的呢？

以下是一些示例：

- 笔迹识别
- 手势识别
- 人脸识别
- 无人机稳定与导航系统
- 预测和识别医疗条件
- 歌曲识别
- 语音识别
- 游戏智能
- 天气预报
- 谎言识别
- 机器人

###Usage and Examples

请查看documentation来获取更详细的关于Swift AI 组件的说明。   

###Installation

把你需要的文件直接拖入工程里。

###Compatibility

Swift AI 目前依赖于苹果向量/矩阵加速和数字信号处理框架。  

###Using Swift AI？

如果你正在使用Swift AI 在你的工程里，请让我知道。我会添加链接到你的工程。

###Contact

邮箱地址：collinhundley@gmail.com