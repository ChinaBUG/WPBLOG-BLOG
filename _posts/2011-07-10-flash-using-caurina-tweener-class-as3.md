---
ID: 1550
post_title: >
  FLASH-在as3中使用Caurina Tweener
  Class FLASH特效库教程
author: ChinaBUG
post_excerpt: |
  FLASH-在AS3中使用Caurina Tweener Class特效库
  虫神曰：
  这篇文章写得很详细滴，非常不错，推荐阅读哈，即使英文不懂的人，单单看调用的代码就明白怎么使用这个效果库了。
  如果你要了解关于这个类的项目更多信息，请访问官方网站：http://code.google.com/p/tweener/
  重点提示： caurina目录必需与您的FLASH项目同一个目录。
  我们需要像下面代码一般引用这个类：
  import caurina.transitions.Tweener;
  现在让我们看看最基本的变化应用：
  Tweener.addTween(circle, {x:390, time:1, transition:"linear"});
  参数说明:
  * circle – 效果要应用的对象
  * x:390 - 这个参数是原对象的属性值
  * time –效果应用延时几秒
  * transition – 变化特效库的特效类型，具体其他的类型，请看参考手册
  * 其他参数请参考效果库手册
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/flash-using-caurina-tweener-class-as3.html
published: true
post_date: 2011-07-10 18:41:04
---
FLASH-在AS3中使用Caurina Tweener ClassFLASH特效库

虫神曰：

这篇文章写得很详细滴，非常不错，推荐阅读哈，即使英文不懂的人，单单看调用的代码就明白怎么使用这个效果库了。

原文：<a href="http://www.flashuser.net/flash-actionscript-as3/using-caurina-tweener-class-as3.html" target="_blank">Using Caurina Tweener Class AS3</a>

作者说他喜欢在他的Flash项目中使用caurina，因为它简单，优雅和灵活的。您可以使用多个属性创造一个过渡，而不会发生问题，几行代码就能创建复杂的动画。在本文中，他将使用几个例子来演示caurina的使用。

如果你要了解关于这个类的项目更多信息，请访问官方网站：

<a href="http://code.google.com/p/tweener/" target="_blank">http://code.google.com/p/tweener/</a>

首先，需要下载效果库的最新版本，请在官方主页左边部分找到“Featured"下的"downloades”一节，挑一个版本打开进入下载。

<span style="text-decoration: underline; color: #ff0000;">重点提示： caurina目录必须与您的FLASH项目同一个目录。（不懂得话，请在最后面，下载源代码去看一下就知道了）</span>

我们需要像下面代码一般引用这个类：

import caurina.transitions.Tweener;

现在让我们看看最基本的变化应用：

Tweener.addTween(circle, {x:390, time:1, transition:"linear"});

参数说明:
<ul id="circle">
	<li><strong>circle</strong> – 效果要应用的对象</li>
	<li><strong>x:390</strong> - 这个参数是原对象的属性值</li>
	<li><strong>time</strong> –效果应用时间，以秒为单位</li>
	<li><strong>transition</strong> – 变化特效库的特效类型，具体其他的类型，请看参考手册</li>
	<li>其他参数请参考效果库手册</li>
</ul>
<span style="text-decoration: underline; color: #ff0000;"><strong>特别关注：</strong></span><a href="http://hosted.zeh.com.br/tweener/docs/en-us/" target="_blank">在线参考手册</a>

&nbsp;

<strong>1. Simple Tween -- 简单直线运动:</strong>

将<em>circle</em>对象实例从x=10以线行运动方式移动到x=390的位置上。
<pre title="">import caurina.transitions.Tweener;
circle.x = 10;
Tweener.addTween(circle, {x:390, time:1, transition:"linear"});</pre>
<object id="fm_tweener1_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener1.swf" /><embed id="fm_tweener1_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener1.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>2. Multiple MovieClip attributes  -- 多属性运动: </strong>

将 <em>circle</em>对象实例以线性运动的方式从x=10,y=75,alpha = 0移动到x=350,y=150,alpha = 1。
<pre title="">import caurina.transitions.Tweener;
circle.x = 10;
circle.y = 75;
circle.alpha = 0;
Tweener.addTween(circle, {x:350, y:150, alpha:1, time:1, transition:"linear"});</pre>
<object id="fm_tweener2_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener2.swf" /><embed id="fm_tweener2_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener2.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>3. Two transitions -- 应用两种变化: </strong>

首先移动 circle对象实例的x轴，接着变化是移动 circle对象实例的y轴。
<pre title="">import caurina.transitions.Tweener;
circle.x = 10;
circle.y = 75;
Tweener.addTween(circle, {x:350, time:0.5, transition:"easeInQuart"});
Tweener.addTween(circle, {y:150, time:1, transition:"easeOutBounce"});</pre>
<object id="fm_tweener3_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener3.swf" /><embed id="fm_tweener3_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener3.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>4. Delay parameter -- 延时参数: </strong>

在第一次变化之后将有1秒的延时方进行第二次的变化（怎么计算第二次的延时开始时间？它是以第一次开始时间为基点计算的，time参数的值）。
<pre title="">import caurina.transitions.Tweener;
circle.x = 10;
circle.y = 75;
Tweener.addTween(circle, {x:350, time:0.5, transition:"easeInQuart"});
Tweener.addTween(circle, {y:150, time:1,transition:"easeOutBounce", delay:1.5});</pre>
<object id="fm_tweener4_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener4.swf" /><embed id="fm_tweener4_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener4.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>5. onComplete 方法的参数: </strong>

在变化完成之后你可以做一些事情，比如改变一下对象的透明值？
<pre title="">import caurina.transitions.Tweener;
tF.alpha = 0;
circle.x = 10;
circle.y = 75;
Tweener.addTween(circle, {x:350, time:0.5, transition:"easeInQuart", onComplete:func});

function func() {
	tF.alpha = 1;
}</pre>
<object id="fm_tweener5_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener5.swf" /><embed id="fm_tweener5_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener5.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>6. onCompleteParams 方法的参数: </strong>

加入你希望你的onComplete方法有参数，那么你需要使用onCompleteParams方法。
<pre title="">import caurina.transitions.Tweener;
tF.alpha = 0;
	circle.x = 10;
	circle.y = 75;
	Tweener.addTween(circle, {x:350, time:0.5, transition:"easeInQuart", onComplete:func, onCompleteParams:["Using onCompleteParams"]});

function func(t:String) {
	tF.txt.text = t;
	tF.alpha = 1;
}</pre>
<object id="fm_tweener6_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener6.swf" /><embed id="fm_tweener6_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener6.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

<strong>7. Special Properties – Color  -- 特殊的属性~颜色: </strong>

特殊的颜色属性帮你应用简单的色彩到你的对象上。
<pre title="">import caurina.transitions.Tweener;
import caurina.transitions.properties.ColorShortcuts;
ColorShortcuts.init();

circle.x = 10;
circle.y = 75;
Tweener.addTween(circle, {x:200, _color:0xFF0000, time:1, transition:"easeOutElastic"});</pre>
<object id="fm_tweener7_1966566756" width="450" height="200" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="base" value="http://www.flashuser.net/flash-files/tutorials/tweener/" /><param name="src" value="http://www.flashuser.net/flash-files/tutorials/tweener/tweener7.swf" /><embed id="fm_tweener7_1966566756" width="450" height="200" type="application/x-shockwave-flash" src="http://www.flashuser.net/flash-files/tutorials/tweener/tweener7.swf" base="http://www.flashuser.net/flash-files/tutorials/tweener/" /></object>

&nbsp;

例子中的参数还不足以设计出更强，更有冲击力的效果，你可以参考在线手册，去设计更好，更强悍的效果噢。

下面提供源代码下载，让你方便进行必要的修改与学习：<a href="http://www.flashuser.net/flash-files/tutorials/tweener/tweener.zip" target="_blank">点我下载源文件</a>