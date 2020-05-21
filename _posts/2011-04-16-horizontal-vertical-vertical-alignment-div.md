---
ID: 921
post_title: DIV横向竖向垂直对齐
author: ChinaBUG
post_excerpt: DIV层横向竖向垂直对齐
layout: post
permalink: >
  http://blog.ipodmp.com/2011/04/horizontal-vertical-vertical-alignment-div.html
published: true
post_date: 2011-04-16 14:07:43
---
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "<a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd</a>"&gt;
&lt;html xmlns="<a href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</a>"&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8" /&gt;
&lt;meta content="all" name="minwt" /&gt;
&lt;meta name="author" content="男丁格爾&amp;梅干扣肉" /&gt;
&lt;meta name="Copyright" content="<a href="http://www.minwt.com/">www.minwt.com</a>版權所有" /&gt;
&lt;meta name="keywords" content="梅問題,教學網,photoshop教學,flex教學,javascript教學,css教學,網頁教學,架站教學,wordperss教學"&gt;

&lt;title&gt;梅問題‧教學網【CSS範例教學-讓區塊中的DIV垂直水平居中】&lt;/title&gt;
&lt;style type="text/css"&gt;
.div_table-cell{
 width:100%;
 height:500px; 
 background-color:#333;
 display:table-cell;
 text-align:center;
 vertical-align:middle;
 border:solid 2px #fff; 
 }

/* IE6 hack */
.div_table-cell span{
 height:100%;
 display:inline-block;
 }

/* 讓table-cell下的所有元素都居中 */
.div_table-cell *{ vertical-align:middle;}
&lt;/style&gt;
&lt;/head&gt;

&lt;body&gt;
&lt;div&gt;&lt;span&gt;&lt;/span&gt;&lt;img src="img01.jpg" width="350" height="233" /&gt;&lt;/div&gt;
&lt;div&gt;&lt;span&gt;&lt;/span&gt;&lt;img src="img02.jpg" width="350" height="233" /&gt;&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;