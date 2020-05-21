---
ID: 2197
post_title: >
  定时器函数setTimeout及clearTimeout和setInterval及clearInterval
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/10/timer-function-settimeout-and-cleartimeout-and-setinterval-and-clearinterval.html
published: true
post_date: 2012-10-09 12:07:49
---
定时器函数setTimeout和setInterval

Javascript及Flash actionscript都是差不多的语法与作用。

setTimeout()方法用于在指定的毫秒数后调用函数或计算表达式， setTimeout() 只执行 code 一次。使用clearTimeout可以随时停止计时。

setInterval(funhander,time)的作用是，每隔time毫秒后，就执行一次句柄funhander指向的方法，setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。

实例：

function page_list(){
alert("123");
}
obj = setTimeout(page_list, 5000);
clearTimeout (obj);

obj = setInterval(page_list, 5000);
clearInterval (obj);