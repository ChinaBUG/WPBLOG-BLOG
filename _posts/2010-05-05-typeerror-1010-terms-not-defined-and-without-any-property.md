---
ID: 499
post_title: 'TypeError: Error #1010: 术语尚未定义，并且无任何属性。'
author: ChinaBUG
post_excerpt: 'TypeError: Error #1010: 术语尚未定义，并且无任何属性。 '
layout: post
permalink: >
  http://blog.ipodmp.com/2010/05/typeerror-1010-terms-not-defined-and-without-any-property.html
published: true
post_date: 2010-05-05 11:07:43
---
　　最近在用Actionscript3.0开发FLASH，今天调试时发生下面错误，网上找了一下，什么问题都有，还是自己记录一下吧。

　　错误信息如下：

<em>　　　　TypeError: Error #1010: 术语尚未定义，并且无任何属性。
　　　　at index30_fla::MainTimeline/clickHandler()</em>

　　经过逐步的调试，终于找到出错的原因，那就是我在clickHandler()中使用了this关键字来代替MC对象，结果FLASH找不到这个对象吧，就报错啦。