---
layout: post
title: "Github Blog - 界面自定义"
date: 2016-03-29 10:34:13 +0800
comments: true
categories: Octopress
tags: 
keywords: Octorpress,自定义，返回按钮
---
###添加侧边栏

首先，在`source\_includes\custom\asides\`下创建`name.html`文件，内容遵守如下格式：  
	
	<section>
    	<h1>Name</h1>
    	<ul id="name">
		此处填写要插入的内容
    	</ul>
	</section>

然后，在`_config.yml`文件中设置`default_asides`添加`custom\asides\name.html`。

<!-- more -->


###博客首页显示文章摘要

默认情况下，博客首页文章列表中都会全部展示，要想让文章在首页中只显示一部分配置如下：   
首先，在文章中先要展示的缩略部分添加标记：  
	
	<!-- more -->
然后，在`_config.yml`文件中设置`excerpt_link`为`"Read on &rarr;"`

	excerpt_link: "Read on &rarr;"

即可显示如下文章摘要效果：

###侧边栏增加新浪微博

首先，要从[新浪微博秀](http://app.weibo.com/tool/weiboshow)获取到自定义的微博秀代码，设定好样式后将代码复制，在`source/_includes/custom/asides`目录下创建`weibo.html`，并按添加侧边栏格式插入获取的代码。  
  
然后，在`default_asides`中加入`custom/asides/weibo.html`。

###添加返回顶部按钮

首先，创建`source/javascripts/top.js`，并添加如下代码设置返回按钮事件：  

	function goTop(acceleration, time)
	{
        acceleration = acceleration || 0.1;
        time = time || 16;

        var x1 = 0;
        var y1 = 0;
        var x2 = 0;
        var y2 = 0;
        var x3 = 0;
        var y3 = 0;

        if (document.documentElement)
        {
                x1 = document.documentElement.scrollLeft || 0;
                y1 = document.documentElement.scrollTop || 0;
        }
        if (document.body)
        {
                x2 = document.body.scrollLeft || 0;
                y2 = document.body.scrollTop || 0;
        }
        var x3 = window.scrollX || 0;
        var y3 = window.scrollY || 0;

        var x = Math.max(x1, Math.max(x2, x3));
        var y = Math.max(y1, Math.max(y2, y3));

        var speed = 1 + acceleration;
        window.scrollTo(Math.floor(x / speed), Math.floor(y / speed));

        if(x > 0 || y > 0)
        {
                var invokeFunction = "goTop(" + acceleration + ", " + time + ")";
                window.setTimeout(invokeFunction, time);
        }
	}

然后，创建`source/_includes/custom/totop.html`，  
并添加如下代码设置返回顶部按钮样式和位置：  

	<!--返回顶部开始-->
	<div id="full" style="width:0px; height:0px; position:fixed; right:180px; bottom:150px; z-index:100; text-align:center; background-color:transparent; cursor:pointer;">
		<a href="#" onclick="goTop();return false;"><img src="/images/top.png" border=0 alt="返回顶部"></a>
	</div>
	<script src="/javascripts/top.js" type="text/javascript"></script>
	<!--返回顶部结束-->

其中，`right`和`bottom`属性用于设置返回图片按钮的位置。

最后，选择自己喜爱的返回按钮图片，命名为`top.png`后添加到`source/images`目录中。

***注：我自己按此方法添加时，没有效果，查看网页源代码发现添加返回按钮的代码并没有加载。所以把`totop.html`文件中的内容添加到了`navigation.html`文件中，成功实现了添加返回顶部按钮。***


###设置在新标签中打开第三方链接

Octorpress博客中，默认是在当前界面中打开第三方链接，这导致网站浏览者跳到第三方链接后很难回来。  
将以下代码加入`source/_includes/custom/head.html`文件中：  

	<script type="text/javascript">
	function addBlankTargetForLinks () {
	$('a[href^="http"]').each(function(){
		$(this).attr('target', '_blank');
	});
	}
 
	$(document).bind('DOMNodeInserted', function(event) {
	addBlankTargetForLinks();
	});
 	</script>

*注：本站的链接还是会在当前界面中打开。*

###修改标签栏logo图片

选择喜欢的图片，替换`source`目录下的`favicon.png`即可。

###参考资料
[自定义你的Octopress博客](http://foggry.com/blog/2014/04/28/custom-your-octopress-blog/)




	