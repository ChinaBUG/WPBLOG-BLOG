---
ID: 671
post_title: >
  FLASH用loadMovie载入图片之后如何获取属性
author: ChinaBUG
post_excerpt: |
  FLASH中用loadMovie()函数载入图片，如何获取图片的真是尺寸呢(长、宽等~)？
  这个就是用来检测并获取属性的代码噢~
layout: post
permalink: >
  http://blog.ipodmp.com/2010/07/detect-image-load-complete-after-loadmovie.html
published: true
post_date: 2010-07-12 10:51:21
---
　　FLASH中用loadMovie()函数载入图片，如何获取图片的真是尺寸呢(长、宽等~)？

　　这个就是用来检测并获取属性的代码噢~

var mcLoader:MovieClipLoader = new MovieClipLoader();
var mclListener:Object = new Object();
mclListener.onLoadInit = function(target_mc:MovieClip, status:Number):Void {
    // if this event is fired (onLoadInit) it means the clip has 100% loaded
    trace("Clip loaded, image dimentions: " + target_mc._width + " x " + target_mc._height);
}
// start the loadClip process
mcLoader.loadClip("test.png", _root.ContainerMovieclip);