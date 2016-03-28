---
layout: post
title: "Github Blog - 搭建"
date: 2016-03-28 19:01:23 +0800
comments: true
categories: Octopress
tags: tech
keywords: octopress,github
---
关于搭建Github Blog请查看教程[http://wiki.jikexueyuan.com/project/github-page/](http://wiki.jikexueyuan.com/project/github-page/ "像geek一样写博客")

下面主要谈下自己在搭建过程中遇到的问题：

1、教程中访问ruby网站采用的淘宝镜像[http://ruby.taobao.org/](http://ruby.taobao.org/) 无法访问，需要采用[https://ruby.taobao.org/](https://ruby.taobao.org/) 替代。

2、教程中导入插件后提示*“Build Warning: Layout 'nil' requested in atom.xml does not exist.”*错误，需要将导入的xml文件中`layout = nil` 替换为 `layout = null`。

