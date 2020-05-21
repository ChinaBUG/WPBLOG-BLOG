---
ID: 873
post_title: >
  JQuery插件-自动调整图片的宽度与高度
author: ChinaBUG
post_excerpt: |
  /*
  自动缩略一些大图的小JS
  吕 lvjiyong@Gmail.com
  http://www.lvjiyong.com/tag/jquery/
  更新:2007.9.22
  更新2007.9.28
  调用:
  先引用上面的脚本或将上页的脚本放入自己的JS库,然后只要再加 $(function(){ $("图片组所在的容器").ImageAutoSize(限制最大宽,限制最大高);});
  */
layout: post
permalink: >
  http://blog.ipodmp.com/2011/01/jquery-plugin-automatically-adjust-the-width-and-height.html
published: true
post_date: 2011-01-10 12:00:02
---
/*
自动缩略一些大图的小JS
吕 <a href="mailto:lvjiyong@Gmail.com">lvjiyong@Gmail.com</a>
<a href="http://www.lvjiyong.com/tag/jquery/">http://www.lvjiyong.com/tag/jquery/</a>
更新:2007.9.22
更新2007.9.28
调用:
先引用上面的脚本或将上页的脚本放入自己的JS库,然后只要再加 $(function(){ $("图片组所在的容器").ImageAutoSize(限制最大宽,限制最大高);});
*/
jQuery.fn.ImageAutoSize = function(width,height)
{
$("img",this).each(function()
{
var image = $(this);
if(image.width()&gt;width)
{
image.width(width);
image.height(width/image.width()*image.height());
}
if(image.height()&gt;height)
{
image.height(height);
image.width(height/image.height()*image.width());
}
});
}

<span style="text-decoration: underline;"><strong>虫曰：
</strong></span><span style="text-decoration: underline;"><strong>在使用的实践中，会出现载入时无法执行，需要重新载入方可执行命令。</strong></span>

附录：另一种写法
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

	$.fn.imgAutoSize = function (padding) {
		var maxWidth = this.innerWidth() - (padding || 0);
		return this.find('img').each(function (i, img) {
			loadImg(this.src, function () {
				if (this.width &gt; maxWidth) {
					var height = maxWidth / this.width * this.height,
						width = maxWidth;

					img.width = width;
					img.height = height;
				};
			});
		});
	};

})(jQuery);

使用时用下面代码：</pre>
<pre>jQuery(function ($) {
	// .imgWrap这个是图片外部容器，使用了本插件后超大的图片宽度将会限制在容器宽度
	// 如果要控制图片与容器的边距，如20像素： $('.imgWrap').imgAutoSize(20);
	$('.imgWrap').imgAutoSize();
});</pre>
2013.02.05 然后，因为使用的情况复杂哈，对这个做了扩展，增加了最小的宽度，原先只有一个参数就是指定padding，现在可以指定最小宽度，如果计算之后的宽度还是比设定的大，就以设定的为主。
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
	$.fn.imgAutoSize = function (padding,<span style="color: #ff0000;">minwidth</span>) {
		var maxWidth = this.innerWidth() - (padding || 0);
		if((minwidth||maxWidth)&lt;maxWidth) maxWidth=minwidth;
 		return this.find('img').each(function (i, img) {
 			loadImg(this.src, function () {
 				if (this.width &gt; maxWidth) {
					var height = maxWidth / this.width * this.height,width = maxWidth;
					img.width = width;
					img.height = height;
				};
			});
		});
	};
})(jQuery);</pre>