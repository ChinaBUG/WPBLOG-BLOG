---
ID: 1871
post_title: >
  FLASH-影片的排序控制对象mx.behaviors.DepthControl.bringForward
  bringToFront sendBackward sendToBack
author: ChinaBUG
post_excerpt: |
  上移一层：mx.behaviors.DepthControl.bringForward
  移至顶层：mx.behaviors.DepthControl.bringToFront
  下移一层：mx.behaviors.DepthControl.sendBackward
  移至底层：mx.behaviors.DepthControl.sendToBack
layout: post
permalink: >
  http://blog.ipodmp.com/2011/12/flash-movie-sort-of-control-object-mx-behaviors-depthcontrol-bringforward-bringtofront-sendbackward-sendtoback.html
published: true
post_date: 2011-12-24 16:36:48
---
FLASH-影片的排序控制对象

上周，在开发庆圣诞元旦的双蛋活动的砸蛋游戏时，遇上一个问题：怎样把砸到的蛋蛋显示到最前面，不让边上的蛋蛋给遮住效果噢？就在想，要是也能用代码控制影片夹子的排列就好了，在FLASH开发环境中就很简单的噢，那么有办法用代码控制对象的排列顺序吗？

当然可以了噢，FLASH要是连这点简单的都不行，那还有什么搞头噢。下面就是排列的四个对象：

上移一层：mx.behaviors.DepthControl.bringForward

移至顶层：mx.behaviors.DepthControl.bringToFront

下移一层：mx.behaviors.DepthControl.sendBackward

移至底层：mx.behaviors.DepthControl.sendToBack

有了对象怎么使用这四行代码噢~下面是各自的语法了，其实好简单~~就接受一个参数，参数就是影片对象的ID噢。。。

如下例，将鸡蛋1移至到顶层~egg1就是鸡蛋1的ID名字噢~记住这个一定要唯一的，如果你这样写，但是没效果的话，请检查一下这个对象名到底是不是唯一的噢。

mx.behaviors.DepthControl.bringToFront( egg1 );

其他的语法应用如下：

将鸡蛋1上移一层：mx.behaviors.DepthControl.bringForward( egg1 );

将鸡蛋1移至顶层：mx.behaviors.DepthControl.bringToFront( egg1 );

将鸡蛋1下移一层：mx.behaviors.DepthControl.sendBackward( egg1 );

将鸡蛋1移至底层：mx.behaviors.DepthControl.sendToBack( egg1 );

这下好了噢~~回去试试看吧~

<a href="http://blog.ipodmp.com/wp-content/uploads/2011/12/201112-egg.jpg"><img class="alignnone  wp-image-1872" title="201112-egg" src="http://blog.ipodmp.com/wp-content/uploads/2011/12/201112-egg-300x183.jpg" alt="" width="522" height="318" /></a>