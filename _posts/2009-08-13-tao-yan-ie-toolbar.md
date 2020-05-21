---
ID: 139
post_title: IE中讨人厌的图片工具栏
author: ChinaBUG
post_excerpt: >
  使用IE浏览一个图片时，把鼠标放在这个图片上，会在这个图片的左上角出现一个“图片工具栏”，里面含有保存、打印、email发送、我的图片文件夹这几个功能。
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/tao-yan-ie-toolbar.html
published: true
post_date: 2009-08-13 14:56:50
---
<p>　　使用IE浏览一个图片时，把鼠标放在这个图片上，会在这个图片的左上角出现一个&ldquo;图片工具栏&rdquo;，里面含有保存、打印、email发送、我的图片文件夹这几个功能。</p>
<p>　　功能是不错，可惜太容易让人把网站的图片给下载啦，怎么屏蔽掉呢？</p>
<p>　　网上找了一下，好简单的，能用，记录一下：</p>
<blockquote>
<p>1、在&lt;head&gt;里加入：</p>
<p>&lt;meta http-equiv=&quot;imagetoolbar&quot; content=&quot;no&quot;&gt;</p>
<p>2、或者在需要屏蔽的图片上加上：</p>
<p>galleryimg=&quot;no&quot;</p>
</blockquote>