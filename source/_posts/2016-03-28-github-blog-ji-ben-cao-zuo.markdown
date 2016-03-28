---
layout: post
title: "Github Blog - 基本操作"
date: 2016-03-28 19:43:53 +0800
comments: true
categories: Octopress
tags: 
keywords: Octopress
---
###创建新博文
`rake new_post["article_name"]`<br>
会在octopress/source/_post/目录生成.markdown文件，可使用markdownpad2编辑。

###删除博文
只需删除对应的markdown文件即可。

###更新博客
`rake generate`<br>
自动生成/public目录下的展示页面。

`rake preview`<br>
本地预览，地址为loccalhost:4000,`ctrl+C`终止预览。

`rake deploy`<br>

###push到github
`git add .`<br>
`git commit -m "message"`<br>
`git push origin source`
