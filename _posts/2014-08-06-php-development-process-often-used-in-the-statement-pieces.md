---
ID: 3547
post_title: >
  PHP-二次开发过程中经常用到的语句碎片
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/08/php-development-process-often-used-in-the-statement-pieces.html
published: true
post_date: 2014-08-06 11:44:26
---
<ol style="list-style-type: upper-roman;">
	<li>开发的时候是不是经常会发生一些错误，老是提示你什么索引没有定义啦，用这个吧，包你解决，一条见效
<blockquote>error_reporting(E_ALL &amp; ~(E_STRICT | E_NOTICE | E_WARNING));</blockquote>
</li>
	<li>开发的时候，每次输出的时候都是乱码，哎呀，真讨厌，没记住这个语句只能收藏以备复制黏贴了。
<blockquote>header("Content-Type: text/html; charset=utf-8");</blockquote>
</li>
	<li>要让网页页面不缓存数据怎么办噢？试试下面的代码呗
<blockquote>header("Cache-Control:no-store, no-cache, must-revalidate");
header("Expires: Mon, 26 Jul 1997 05:00:00 GMT");
header('Progma: no-cache');</blockquote>
</li>
	<li>字符串与数组经常要操作的方法如下
<blockquote>serialize(Array)与unserialize(Array)
explode("",String)与implode("",Array)</blockquote>
</li>
	<li></li>
</ol>