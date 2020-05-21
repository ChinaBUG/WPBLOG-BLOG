---
ID: 2484
post_title: >
  jQuery-相册插件Galleriffic Scale
  Images – A Quick Hack (jQuery)
author: ChinaBUG
post_excerpt: |
  jQuery相册插件Galleriffic如何缩放图片-一个快速的Hack方法
  //现在增加下面这些代码到到方法preloadRecursive和refresh方法之中相应的地方
  
  var origWidth = image.width;
  var origHeight = image.height;
  var newWidth = this.scaleWidth;
  var newHeight = parseInt((parseInt(origHeight) * parseInt(newWidth) / parseInt(origWidth)));
  image.height = newHeight;
  image.width = newWidth;
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/jquery-galleriffic-scale-images-a-quick-hack-jquery.html
published: true
post_date: 2013-06-15 17:33:49
---
jQuery相册插件Galleriffic如何缩放图片-一个快速的Hack方法

原谅我编辑了 jQuery gallerific相册插件，让他有了缩放的功能。下面是我所做的代码修改，我一共修改了两个地方，我们开始吧。

根据源代码显示，我使用的 jQuery galleriffic相册插件的版本是2.0的，我需要的功能是图片能够自动的根据宽度及高度做缩放的调整。

下面从一个基本的公式说起：

orig_width x orig_height 与new_width x new_height是一个等比的关系（哈，不知道是不是这么叫的）

new_width我们知道具体值，那么new_height最终的结果是：

new_height = (orig_height * new_width) / orig_width

对此，我们可以根据这个算法来做修改。我们需要将代码增加到插件中去（注：这个是快捷方式，不需要注重代码效率，当然你可以优化一下。）

//在代码中查找这个实例

var image = new Image();

//在下面您将看待下面的代码

image.alt = imageData.title;
image.src = imageData.slideUrl;

//现在增加下面这些代码到到方法preloadRecursive和refresh方法之中相应的地方

var origWidth = image.width;
var origHeight = image.height;
var newWidth = this.scaleWidth;
var newHeight = parseInt((parseInt(origHeight) * parseInt(newWidth) / parseInt(origWidth)));
image.height = newHeight;
image.width = newWidth;

就这样修改结束。

备注：

需要将修改到方法preloadRecursive和refresh方法之中，然后在使用的时候，增加一个选项scaleWidth就可以了。

摘自：<a href="http://blog.lysender.com/2010/04/galleriffic-scale-images-a-quick-hack-jquery/">Galleriffic Scale Images – A Quick Hack (jQuery)</a>