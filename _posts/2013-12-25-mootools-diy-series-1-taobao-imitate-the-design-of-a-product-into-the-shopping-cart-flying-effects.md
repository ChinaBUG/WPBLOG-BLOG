---
ID: 3113
post_title: >
  mootools-DIY系列1:模仿淘宝设计一个商品飞入购物车的特效果
author: ChinaBUG
post_excerpt: |
  有一个客户需要我协助代写脚本开发mootools版的飞入购物车的特效，报价700结果嫌贵没做跑了~真是好一阵伤心呐~难道这年头做技术的就是这么廉价吗？！
  郁闷的话就不说了~这边就公开一下设计思路，给想免费的童靴一个圣诞礼物吧~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/mootools-diy-series-1-taobao-imitate-the-design-of-a-product-into-the-shopping-cart-flying-effects.html
published: true
post_date: 2013-12-25 11:12:15
---
有一个客户需要我协助代写脚本开发mootools版的飞入购物车的特效，报价700结果嫌贵没做跑了~真是好一阵伤心呐~难道这年头做技术的就是这么廉价吗？！

郁闷的话就不说了~这边就公开一下设计思路，给想免费的童靴一个圣诞礼物吧~

<span style="text-decoration: underline;"><strong>思路分享</strong></span>

商品飞入购物车是什么样子的呢？具体请看淘宝，点击商品就会看到商品的图片慢慢的飞进我们右下方的购物车内。

在这个添加到购物车的操作中，我们会发现，这个操作可以分解为两个部分：一个是点击触发添加商品，一个是触发之后的动画。

<span style="text-decoration: underline;"><em>1.点击触发添加商品</em></span>

这部分其实比较简单，想怎么触发就怎么触发，触发之后添加商品需要什么样子的行为都在这边控制，这个不难。

<span style="text-decoration: underline;"><em>2.触发之后的动画</em></span>

这部分比较难，对于对mootools不熟悉的童靴来说，可能会没有思路，不懂得这个要怎么设计噢。

这边就重点说一下2.的动画要怎么设计，其他的都是很简单的问题了。

我们看到商品飞进购物车里面，我们很自然就会这么思考着：

Q.要怎么才能让商品图片一边缩小，一边变透明，一边移动？

A.很自然我们就会想到mootools是提供动画模块的<a href="http://mootools.net/docs/core/Fx/Fx.Morph#Fx-Morph">Fx.Morph</a>类，这个类专门用于多属性同事变化的动画。具体就请看相关的类说明。

Q.要怎样才能做到原图不变，影子图不断变化呢？

A.这个其实很简单，只要复制一份同样的内容叠在原来的图片上方即可。操作一份变化，另外原图不动。

所以，思路出来了，我们在做这个功能的时候整个的设计思路是：

<em>当我们点击添加购物车按钮的时候，我们先复制当前商品的图片，然后对这个复制的图片应用动画，移动到购物车所在的位置，然后隐藏即可。</em>

思路简单吧~

PS：mootools其实个人觉得很简单，主要是资料不多才是弊病，其他的都觉的比较适合初学者使用。

在mootools中我们怎么获取购物车的位置呢？

在网页设计中要确定一个元素的位置就是确定他的top,left的值，那么在mootools中是怎样获取到这个值的呢？
<blockquote>参考：<a href="http://mootools.net/docs/core/Element/Element.Dimensions">Element.Dimensions</a></blockquote>
我们从参考网站内知道我们可以使用<a href="http://mootools.net/docs/core/Element/Element.Dimensions#Element:getCoordinates">getCoordinates</a>来获取元素的宽度，高度，top，left等等的值，所以我们就可以使用它获取购物车的位置了：
<pre>var cart_pos = $('shopping-cart').getCoordinates();</pre>
获取完购物车的位置，我们还需要做什么呢？噢~复制我们需要的商品的图片，在mootools之中提供一个方法，就是<a href="http://mootools.net/docs/core/Element/Element#Element:clone">clone()</a>方法，他的作用就是复制目标元素：
<pre>var copy = myElement.clone([contents, keepid]);</pre>
然后我们需要设置这个复制的元素的样式跟原先的一样，唯一区别是需要指定定位方式，要不可没办法叠加在一起噢。
<pre>el.setStyles(obj_good_img.getCoordinates()).setStyles({position: 'absolute'})</pre>
接着就是要指定动画了~

下面的代码就是全部的代码，跟之前的思路一样简洁吧~~

代码列表（<a href="http://jsfiddle.net/ChinaBUG/zWZ5W/">可以点击查看实际效果</a> 进入jsfiddle.net后点击顶部的run按钮，在右下角区域查看效果）：
<iframe src="http://jsfiddle.net/ChinaBUG/zWZ5W/embedded/" height="300" width="100%" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

很简单吧~效果出来了有没有呀~~

PS：例子之中的代码除了JS代码之外其他的结构代码均取自JQuery版的同类项目<a href="http://codepen.io/ElmahdiMahmoud/pen/tEeDn">jQuery. Fly to cart effect. - CodePen</a>。感谢作者哈~