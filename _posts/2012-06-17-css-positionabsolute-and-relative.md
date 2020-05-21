---
ID: 2097
post_title: CSS-position:absolute and relative
author: ChinaBUG
post_excerpt: |
  今天，想要在一个按钮边上弄一张图片，漂浮在按钮的右边角，就像我们经常看到的new或者是hot那样子的效果。
  结果想半天不知道怎么去做。分析来肯定是用定位，而且是绝对定位，但是怎么尝试就是没办法实现我所要的效果。
  最后才发现，要实现这个效果的前提是父类容器需要的定位方式必须为相对，效果才会出现。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/06/css-positionabsolute-and-relative.html
published: true
post_date: 2012-06-17 00:10:53
---
今天，想要在一个按钮边上弄一张图片，漂浮在按钮的右边角，就像我们经常看到的new或者是hot那样子的效果。

结果想半天不知道怎么去做。分析来肯定是用定位，而且是绝对定位，但是怎么尝试就是没办法实现我所要的效果。

最后才发现，要实现这个效果的前提是父类容器需要的定位方式必须为相对，效果才会出现。

代码如下，作为备份吧：

&lt;div style="position:relative;"&gt;
&lt;input name="" type="submit" value="打包下载商品图片" /&gt;
&lt;div id="downtips" style="position:absolute;top:-65px; left:-85px; width:200px; line-height:15px;background-color: #FFFFF7;border: 1px solid #FFCC00;margin: 0 auto 20px; padding:5px;"&gt;
&lt;div style="background-image:url(statics/item_unsel.gif);height:12px;width:12px; float:right; cursor:pointer;" onclick="$('downtips').setStyle('display','none');"&gt;&lt;/div&gt;
&lt;b&gt;打包下载商品图片：&lt;/b&gt;&lt;br/&gt;勾选要下载图片的商品前的&lt;input name="" type="checkbox" value="" disabled="disabled" /&gt;复选框，最后点击下面的按钮。
&lt;/div&gt;
&lt;/div&gt;