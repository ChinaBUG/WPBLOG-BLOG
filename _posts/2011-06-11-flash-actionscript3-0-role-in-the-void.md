---
ID: 1280
post_title: FLASH-ActionScript3.0中void的作用
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/flash-actionscript3-0-role-in-the-void.html
published: true
post_date: 2011-06-11 10:20:51
---
 今天在进行FLASH转盘抽奖程序的开发，在一个需要返回值的函数上，老是出现一个莫名其妙的提示，提示如下：

<strong><span style="text-decoration: underline;">1051: 返回值必须未定义。</span></strong>

让我一阵怀疑到底是不是AS 3.0不允许函数返回这个操作，想想怎么可能呢~怎么办呢~~仔细找了找，还是没发现什么语法上的问题呀，忽然，发现偶的函数后面带了一个void关键字，因为省事，都是直接把原先的函数直接复制过来，之前的函数是可以运行的，这个是啥意思？

不管是啥意思，先去掉再说把，一去掉，运行，哈~~正常运行了~~

原因就是多了void这个关键字，为什么会出错呢？上网找了找......

<span style="text-decoration: underline;"><em>用void是为了方便编译器帮你检查语法以外的错误。</em></span>

<strong><span style="text-decoration: underline;"><em>比如有个函数，不应该有返回值...但是你不注意写了返回值，如果有些void的话，编译器会告诉你你的错误。</em></span></strong>

<em>如果你不需要这种检查，写不写没任何关系。</em>

哈，还是基础不过关造成的呀，真是要命噢~~~分享一下哈~