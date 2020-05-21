---
ID: 1286
post_title: FLASH-AS3.0 随机数random的应用
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/flash-as3-0-random-application-of-random-numbers.html
published: true
post_date: 2011-06-11 11:19:48
---
　　在做FLASH转盘抽奖程序之中，需要用到随机数，来判断当前的中奖名次，之前AS2.0的random函数在AS3.0上又不能使用真是令人着急哈，所幸网络无极限，好容易被我找了好几种方式实现需要的功能。这边提供两种哈，个人觉得还不错，一种比较单调，一种比较更加的随机哈。

　　就不废话了，直接上代码，你要是看不懂就别问了，因为我基础差都懂滴~~~哈

public function showCode() : Number
{
     //定义验证码的范围
     var array = new Array(0,1,2,3,4,5,6,7,8,9);
     var code = "";
     //随机取得验证码字符
     //方法一：0&lt;Math.random()&lt;1扩大百倍再截取个位数上的值即可
     //var j = int(Math.random()*100).toString().substr(-1,1);
     //方法二：0&lt;Math.random()&lt;1扩大N倍等同于AS2.0中的random(n)
     //Math.floor(Math.random()*10)) == random(n)
     var j = Math.floor(Math.random()*10);
     code = array[j];
     return code;
  }