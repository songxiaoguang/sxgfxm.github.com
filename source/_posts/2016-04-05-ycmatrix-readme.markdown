---
layout: post
title: "YCMatrix - README"
date: 2016-04-05 12:40:17 +0800
comments: true
categories: 机器学习
tags: 
keywords: 
---

一个适用于Objective-C和Swift的灵活的矩阵库。 

支持OS X(10.7+)he iOS(8.0+)

##Getting started

###Adding the framework to your project

把库导入你的工程

###Importing

使用`@include YCMatirx`或`#import YCMatirx/YCMatirx.h`

###Dependencies

无需关联其他系统框架

##Usage

###Naming Conventions

产生矩阵的方法以“matrixFrom”为前缀。

###Example

矩阵加法为例：  

	@include YCMatrix;

	Matrix *I = [Matrix identifyOfRows:3 columns:3];
	Matrix *C = [Matrix matrixOfRows:3 columes:3 value:2];
	Matirx *S = [I matrixByAddition:C];
	NSLog:(@"result :%@",S);

	// result:
	// 3.0 2.0 2.0
	// 2.0 3.0 2.0
	// 2.0 2.0 3.0

###What's in thers?

这个库中的文件可以分为4类：  

- Matrix.h：YCMatrix类定义和基础操作
- Matrix+Advanced.h：LAPACK函数
- Matrix+Manipulate.h：操作行列的函数
- Matrix+Map.h：线性矩阵映射
