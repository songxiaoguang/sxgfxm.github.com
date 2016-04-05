---
layout: post
title: "YCML - README"
date: 2016-04-05 10:22:14 +0800
comments: true
categories: 机器学习
tags: 
keywords: 
---

YCML是用Objective-C语言开发的人工智能、机器学习和最优化框架。YCML既可以在Objective-C中使用也可以在Swift中使用。YCML支持Mac OS X和iOS平台。  

YCML目的是为Objective-C/Swift带来高效的算法，用最优的实现。实现这些算法的相关论文在文章的结尾列出。  

YCML包含超过30个高级的测试单元，适合广泛的需要。

###At a glance

- 七种有关监督学习的机器学习算法的实现
- 两种多对象遗传算法的变体

###Introduction

YCML可以用来解决机器学习和计算最优化问题。

###Machine Learning

机器学习领域是用来研究如何使计算机通过经验，而不是通过具体编程来学习某些技能。机器学习算法使计算机可以通过样例学会某项技能，并且把这项技能推广到未知的样例中。  

YCML主要关注回归问题，回归问题是一个经典的问题，目标从一个可以精确预测一个数字的模型中得到，建立在已知的一个或多个输入变量之上。以下是现实中的常见问题：  

- 股市预测
- 地产价格预测
- 机器人和控制
- 机械和材料性能预测
- 冗长和复杂实验的结果近似和模拟
- 在线广告预测
- 医疗诊断自动化

除了回归问题，YCML也可以处理分类问题，通过改变输入和输出数据。

###Muti-Objective Optimization

YCML提供了一些处理最优化问题的算法。具体来说，集中在多目标问题。  

多目标问题是一个经典的问题，超过一个目标存在，并且每个目标是相互冲突的。这意味着提高一个目标的性能，至少另一个目标的性能会降低。这种特性导致无法找到最佳的解决办法。然而，我们通常来找最佳的权衡，一个最佳的权衡是没有其他的性能比总体上这个更好。在多目标最优化问题中，还有叫做不受控的解决方案。  

YCML实现了多目标进化算法。多目标进化算法是一种随机最优化算法采用流行的解决方法。它们可以高效的搜索出最优化解决方案。

##Features

###Learning

目前支持一下算法：

- 梯度下降的反向传播
- 弹性反向传播
- 使用SMO支持向量机回归
- 极限学习机
- 采用正交最小二乘向前选择
- 采用正交最小二乘与媒体统计向前选择
- 二进制受限玻尔兹曼机


###Learning fetures

- 嵌入式模型的输入/输出标准化设施
- 通用监督学习基类，可以容纳多种算法
- 模块化backprop类，支持复杂图形，基于layer对象
- 强大的datarame类，拥有多种函数，可以与Matrix相互转化

###Optimization

- 为单个或者多个对象问题进行单独优化
- 用于优化的代理类

###Other

- 基于 YCMatrix，一个矩阵库，使用加速框架来提升性能
- 使用 NSArray 类进行基础统计

##Getting started

###Setting up

导入工程文件。YCML依赖YCMatrix。0.2.0版本后YCML包含YCMatrix，一般情况下只需要YCML即可。  

YCML定义了一个模块。在你的代码开头导入：  

	@import YCML;

另外，导入YCMatirx用于矩阵计算：

	@import YCMatrix;

###Your first predictive model

下面是一个简单的训练样例，返回一个训练好的模型，通过输入和输出数据集：  

	YCFFN *theModel = [[YCRpropTrainer trainer] train:nil input:trainingInput output:trainingOutput];

YCML模型和训练可能使用YCMatrix来替代数据集。在这种情况下，YCML模型接受矩阵且矩阵的每一列为一组训练样本。下面是通过矩阵训练：  

	YCFFN *theModel = [[YCRpropTrainer trainer] train:nil inputMatrix:trainingInput outputMatrix:trainingOutput];

训练好的模型可以用来做预测，通过数据数据集或矩阵：  

	YCDataframe *prediction = [theModel predict:testInput];

###Woring with YCML Dataframes

使用YCDatarame类，可以很容易的准备数据。增加YCDataframe的实例，调用`addSampleWithData:`方法，传递一个包含数据的字典为参数。如果字典中的参数不在dataframe中，会被自动创建。下面展示了如何创建一个新的datarame并且添加记录：  

	YCDataframe *frame = [YCDataframe dataframe];
	[frame addSampleWithData:@{@"x1":@1.0,@"x2":@2.0,@"x3":@-5.0}];
	[frame addSampleWithData:@{@"x1":@5.5,@"x2":@-3.0,@"x3":@-1.5}];

通过两个dataframe，一个input一个output，你可以很容易的训练一个预测模型。  

###Further Help

查看YCML documentation

##Examples

###Training and activation(oc,using matirces)

	YCMatrix *trainingData = [self matrixWithCSVName:@"housing" removeFirst:YES];
	YCMatrix *trainingOutput = [trainingData getRow:13];
	YCMatirx *trainingInput = [trainingData removeRow:13];
	YCELMtrainer *trainer = [YCELMTrainer trainer];

	YCFFN *model = (YCFFN *)[trainer train:nil inputMatrix:trainingInput outputMatrix:trainingOutput];

	YCMatrix *predictedOutput = [model activateWithMatrix:trainingInput];

##Framework Architecture

##References
[1] D. Rumelhart, G. Hinton and R. Williams. Learning Internal Representations by Error Propagation, Parallel Distrib. Process. Explor. Microstruct. Cogn. Vol. 1, Cambridge, MA, USA: MIT Press; pp. 318–362, 1985.

[2] M. Riedmiller, H. Braun. A direct adaptive method for faster backpropagation learning: the RPROP algorithm. IEEE Int. Conf. Neural Networks; pp. 586-591, 1993.

[3] JC. Platt. Fast Training of Support Vector Machines Using Sequential Minimal Optimization. Adv Kernel Methods pp. 185-208, 1998

[4] GW. Flake, S. Lawrence. Efficient SVM regression training with SMO. Mach Learn 46; pp.271–90, 2002.

[5] G.-B. Huang, H. Zhou, X. Ding, and R. Zhang. Extreme Learning Machine for Regression and Multiclass Classification, IEEE Transactions on Systems, Man, and Cybernetics - Part B:Cybernetics, vol. 42, no. 2, pp. 513-529, 2012.

[6] S. Chen, CN Cowan, PM Grant. Orthogonal least squares learning algorithm for radial basis function networks. IEEE Trans Neural Netw, vol. 2, no. 2, pp. 302–9, 1991.

[7] S. Chen, E. Chng, K. Alkadhimi. Regularized orthogonal least squares algorithm for constructing radial basis function networks. Int J Control 1996.

[8] X. Hong, P. Sharkey, K. Warwick. Automatic nonlinear predictive model-construction algorithm using forward regression and the PRESS statistic. IEEE Proc - Control Theory Appl. vol. 150, no. 3, pp. 245–54, 2003

[9] G. Hinton. Training Products of Experts by Minimizing Contrastive Divergence. Neural Comput. vol. 14, no. 8, pp.1771–800, 2002.

[10] K. Deb, A. Pratap, S. Agarwal, T. Meyarivan. A fast and elitist multiobjective genetic algorithm: NSGA-II. IEEE Trans Evol Comput. vol. 6, pp. 182–97, 2002.

[11] J. Bader, E. Zitzler. HypE: An algorithm for fast hypervolume-based many-objective optimization. Evol Comput 19, pp. 45–76, 2011.