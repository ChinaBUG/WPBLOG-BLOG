---
ID: 2355
post_title: >
  CheatSheet-FLASH(as2/as3)常用代码收藏
author: ChinaBUG
post_excerpt: |
  1、MovieClip对象怎么才能有鼠标指针呢？通常是手型标示
  2、怎么在flash中调用Javascript的方法函数？又怎么在Javascript中调用Actionscript内的方法函数？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/02/cheatsheet-flash-as2as3-commonly-used-code-favorites.html
published: true
post_date: 2013-02-20 17:34:21
---
自学的基础就是差呀，经常记得这个忘记那个，需要用到时才会发现，原来那个点自己没学到。记录一下吧。

++++

1、MovieClip对象怎么才能有鼠标指针呢？通常是手型标示

var start_btn:MovieClip;

this.start_btn.buttonMode = true;//指定指针

2、怎么在flash中调用Javascript的方法函数？又怎么在Javascript中调用Actionscript内的方法函数？

使用flash.external.ExternalInterface接口即可完成任务，具体参考<a href="http://livedocs.adobe.com/flash/9.0_cn/ActionScriptLangRefV3/flash/external/ExternalInterface.html">ExternalInterface</a>

3、