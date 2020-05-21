---
ID: 3220
post_title: >
  jQuery-jQuery.imgAutoSize.js自动调整图片大小(auto
  scale)
author: ChinaBUG
post_excerpt: |
  最近项目要用到自动根据容器的大小调整图片尺寸的大小的功能，记得曾经有收藏过这个插件的，哎，找了半天却发现好像没有~好不容易从过去的项目中找到这个插件，简单，方便，还好用，收藏一下省的以后遇上了还要乱找一通。
  // jQuery.imgAutoSize.js
  // Tang Bin - http://planeArt.cn/ - MIT Licensed
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/jquery-jquery-imgautosize-js-automatically-adjust-the-picture-size-auto-scale.html
published: true
post_date: 2014-03-12 23:54:53
---
最近项目要用到自动根据容器的大小调整图片尺寸的大小的功能，记得曾经有收藏过这个插件的，哎，找了半天却发现好像没有~好不容易从过去的项目中找到这个插件，简单，方便，还好用，收藏一下省的以后遇上了还要乱找一通。
<pre>// jQuery.imgAutoSize.js
// Tang Bin - http://planeArt.cn/ - MIT Licensed
(function ($) {

    var loadImg = function (url, fn) {
        var img = new Image();
        img.src = url;
        if (img.complete) {
            fn.call(img);
        } else {
            img.onload = function () {
                fn.call(img);
                img.onload = null;
            };
        };
    };

    $.fn.imgAutoSize = function (padding,minwidth) {
        var maxWidth = this.innerWidth() - (padding || 0);
        if((minwidth||maxWidth)&lt;maxWidth) maxWidth=minwidth;
        return this.find('img').each(function (i, img) {
            loadImg(this.src, function () {
                if (this.width &gt; maxWidth) {
                    var height = maxWidth / this.width * this.height,
                        width = maxWidth;

                    img.width = width;
                    img.height = height;
                    //2014.03.12 CSS全局设置为100%,防止初始的图片太大撑破容器载入后设置正确大小
                    $(img).css({'width':width,'height':height});
                };
            });
        });
    };

})(jQuery);</pre>
这个插件我已经根据我的需要修改过的噢，至于原始请自行查找。

使用时，只需要指定容器，即可。
<pre>&lt;script&gt;
    $(function(){
        $('.box-p-content').imgAutoSize(10,604);
    });
&lt;/script&gt;</pre>
类名.box-p-content就是容器对象了~