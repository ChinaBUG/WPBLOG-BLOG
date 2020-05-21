---
ID: 2252
post_title: CSS-position:fixed固定你的层
author: ChinaBUG
post_excerpt: |
  最近在设计一个网站需要用到固定左边栏，不随着滚动条滚动而滚动。用JQ来实现吧，麻烦呀~有什么好办法没有噢？
  查找了一下还真有，下面就是代码噢，原来就这么简单
  应用场景：固定一个层；固定一个背景；两边的春联特效；固定一个导航等；右下脚的回到顶部效果等等
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/css-position-fixed.html
published: true
post_date: 2012-12-29 10:11:30
---
最近在设计一个网站需要用到固定左边栏，不随着滚动条滚动而滚动。用JQ来实现吧，麻烦呀~有什么好办法没有噢？

查找了一下还真有，下面就是代码噢，原来就这么简单

应用场景：固定一个层；固定一个背景；两边的春联特效；固定一个导航等；右下脚的回到顶部效果等等
<code>
* html,* html body{
background-image:url(about:blank);
background-attachment:fixed;
}
.main_left{
width:100px;
_height:50px;
position:fixed !important;
top:0px;
float:left;
position:absolute;
z-index:100;
top:expression(offsetParent.scrollTop+10);
left:5px;
}
</code>
然后在IE6+等全系列的浏览器都能用了噢。好神奇。淘宝站也有用噢。

不过，需要说的不要用expression的最好不用，比较耗资源。