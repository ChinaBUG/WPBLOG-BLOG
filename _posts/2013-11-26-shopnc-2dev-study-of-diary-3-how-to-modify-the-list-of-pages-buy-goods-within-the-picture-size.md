---
ID: 2904
post_title: >
  ShopNC二次开发研究日记3:怎么修改团购列表页内的商品的图片大小
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  我们访问团购页面的时候会看到那个图片，有的人会觉得很小，需要修改，可是修改了样式修改了代码就是不见得有效果，为什么呢？
  我们查看一下那边的代码会发现一些情况噢：
  &lt;img width="216" height="216" onload="javascript:DrawImage(this,296,216);" alt="" src="http://127.0.0.4/upload/groupbuy/f183d2d2b87757da7abc5ca0326160fb.jpg_mid.jpg"&gt;
  上面红色的代码表示一载入就执行，正好说明了为什么我们怎么改都不行的原因！
  因为我们修改的是先天的，但是载入修改是后天的，后天的行为会覆盖先天的呗。
  所以我们需要查看一下DrawImage()方法的作用，我们根据感觉都能猜到这个是重置图片的方法，那么我们只要把他的参数修改为我们需要的不就可以了吗？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopnc-2dev-study-of-diary-3-how-to-modify-the-list-of-pages-buy-goods-within-the-picture-size.html
published: true
post_date: 2013-11-26 17:39:38
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

我们访问团购页面的时候会看到那个图片，有的人会觉得很小，需要修改，可是修改了样式修改了代码就是不见得有效果，为什么呢？

我们查看一下那边的代码会发现一些情况噢：

&lt;img width="216" height="216" <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">onload="javascript:DrawImage(this,296,216);"</span></span> alt="" src="http://127.0.0.4/upload/groupbuy/f183d2d2b87757da7abc5ca0326160fb.jpg_mid.jpg"&gt;

上面红色的代码表示一载入就执行，正好说明了为什么我们怎么改都不行的原因！

因为我们修改的是先天的，但是载入修改是后天的，后天的行为会覆盖先天的呗。

所以我们需要查看一下DrawImage()方法的作用，我们根据感觉都能猜到这个是重置图片的方法，那么我们只要把他的参数修改为我们需要的不就可以了吗？

来查看一下这个方法的原型吧：

//图片比例缩放控制
function DrawImage(ImgD,FitWidth,FitHeight){}

我们看到注释这个方法是按比例做缩放的，那就简单了，给他随便指定一下吧。注意缩放的条件即可。反正是以最小的来显示。