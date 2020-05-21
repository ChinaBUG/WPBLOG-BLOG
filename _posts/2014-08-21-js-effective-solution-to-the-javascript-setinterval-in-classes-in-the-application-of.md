---
ID: 3586
post_title: >
  JS-有效解决Javascript中的setInterval在类中的应用
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/08/js-effective-solution-to-the-javascript-setinterval-in-classes-in-the-application-of.html
published: true
post_date: 2014-08-21 17:37:44
---
今天在写Mootools版的CountDown插件封装时，发现要在CountDown倒计时类里面调用setInterval来定时时发现，怎么都是没办法定时，只能是显示一次，奇了怪了，然后找了一圈发现还真的有解决方案哈。

文章摘自：《<a href="http://www.cnitblog.com/CoffeeCat/archive/2008/03/07/35095.html">有效解决Javascript中的setInterval在类中的应用</a>》

------

今天在写一个小程序，遇到一个小问题，就是我想用setInterval函数定时调用某一个函数，而这个函数，又恰好是我的一个类的成员函数。最初，我是这样写的：
<div style="border: 1px solid #cccccc; padding: 4px 5px 4px 4px; background-color: #eeeeee; font-size: 13px; width: 98%;"><span style="color: #0000ff;">function</span><span style="color: #000000;"> ClassA()
{
this.i = 0;
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.start</span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">function</span><span style="color: #000000;">()
{
setInterval(</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.callback,</span><span style="color: #000000;">5000</span><span style="color: #000000;">);
}
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.callback </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">function</span><span style="color: #000000;">()
{
this.i++;
//alert(</span><span style="color: #0000ff;">this</span><span style="color: #000000;">);
}
}
var a =</span><span style="color: #0000ff;"> new</span><span style="color: #000000;"> ClassA();
a.start</span><span style="color: #000000;">();</span></div>
这段代码的目的，是希望让ClassA类中的成员函数callback，每隔5秒被调用一次，从而能让ClassA中的属性i自增。

但是，运行这段代码以后，我们发现，浏览器提示this.i不存在。为什么会出现这样的问题呢？我们在this.i++;下面加上alert语句，看看 this到底是什么。结果显示，this是一个Object Window。而不是我们希望的对象a。为什么会这样呢？因为setInterval是一个Window类的成员函数，所以回调函数也从属于Window 类了。

那如何解决这个问题呢？请看下面的代码：
<div style="border: 1px solid #cccccc; padding: 4px 5px 4px 4px; background-color: #eeeeee; font-size: 13px; width: 98%;"><span style="color: #0000ff;">function</span><span style="color: #000000;"> ClassA()
{
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.i </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #000000;">0</span><span style="color: #000000;">;
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.start </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">function</span><span style="color: #000000;">()
{
</span><span style="color: #0000ff;">var</span><span style="color: #000000;"> _this </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">this</span><span style="color: #000000;">;
</span><span style="color: #0000ff;">var</span><span style="color: #000000;"> callback </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">function</span><span style="color: #000000;">(){ _this.callBack(); };
window.setInterval(callback,</span><span style="color: #000000;">5000</span><span style="color: #000000;">);
}
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.callback </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">function</span><span style="color: #000000;">()
{
</span><span style="color: #0000ff;">this</span><span style="color: #000000;">.i</span><span style="color: #000000;">++</span><span style="color: #000000;">;
}
}
var a </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000ff;">new</span><span style="color: #000000;"> ClassA();
a.start();</span></div>
在start成员函数中，我们用_this保存了ClassA的this，然后，定义了一个内部匿名函数，这个函数用于setInterval的回调，而 在这个匿名函数中，通过_this来调用callBack，这样，就可以保证在调用callback函数时，this指针不是Window，而是对象a， 大家可以细细的品味一下其中的奥秘~~