---
ID: 1926
post_title: >
  Javascript-将文件调用移到head头里
author: ChinaBUG
post_excerpt: |
  在html文档里面根据需要会不断的载入一个文件，搞的这边一个CSS文件，那边一个JS文件什么的，看起来烦躁噢。
  使用下面这段代码就可以将这些文件集中在一起噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/02/javascript-file-calls-over-the-head-in-advance.html
published: true
post_date: 2012-02-08 16:47:09
---
在html文档里面根据需要会不断的载入一个文件，搞的这边一个CSS文件，那边一个JS文件什么的，看起来烦躁噢。

使用下面这段代码就可以将这些文件集中在一起噢。

&lt;script type="text/javascript"&gt;
cssLink=document.createElement("link");
cssLink.type="text/css";
cssLink.rel="stylesheet";
cssLink.href="http://image.yihaodianimg.com/statics/index/css/home_index.css";
document.getElementsByTagName("head")[0].appendChild(cssLink);
&lt;/script&gt;