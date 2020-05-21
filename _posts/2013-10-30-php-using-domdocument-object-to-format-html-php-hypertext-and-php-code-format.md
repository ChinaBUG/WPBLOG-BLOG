---
ID: 2678
post_title: >
  PHP-使用DOMDocument对象来自动格式化HTML/PHP超文本及PHP代码的格式
author: ChinaBUG
post_excerpt: |
  最近在设计采集时发现获取的文件的代码都是很紧凑的，就是压缩格式化过得，分析数据很吃力还要用dreamwever打开格式化一下，很麻烦！
  找了好久才找到下面的代码，作用就是格式化我们的代码噢，在线版的可以参考 JavaScript/HTML格式化 。下面是PHP代码版的，处理之后的代码跟使用在线工具是差不多的噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/php-using-domdocument-object-to-format-html-php-hypertext-and-php-code-format.html
published: true
post_date: 2013-10-30 13:21:54
---
最近在设计采集时发现获取的文件的代码都是很紧凑的，就是压缩格式化过得，分析数据很吃力还要用dreamwever打开格式化一下，很麻烦！

找了好久才找到下面的代码，作用就是格式化我们的代码噢，在线版的可以参考 <a href="http://tool.chinaz.com/Tools/JsFormat.aspx">JavaScript/HTML格式化</a> 。下面是PHP代码版的，处理之后的代码跟使用在线工具是差不多的噢。

//格式化HTML文件
function FormatHTML($html){
$dom = new DOMDocument();
$dom-&gt;preserveWhiteSpace = FALSE;
$dom-&gt;loadHTML($html);
$dom-&gt;formatOutput = TRUE;
return $dom-&gt;saveHTML();
}