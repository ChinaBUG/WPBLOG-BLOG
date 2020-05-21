---
ID: 1301
post_title: PHP-preg_match
author: ChinaBUG
post_excerpt: |
  虫曰：
  一定要记住那个正则表达式外面还有两个/括住！！消掉，这正则表达式就没作用了。。悲惨呀~~~就为了这两个/号，我整整花了周末一天半时间来调试。基础很关键呀。
  这边提供测试可用的检查移动手机号码的正则表达式给需要的人哈：
  preg_match('/^(13\d{1}|15[03689])\d{8}$/',$mobile)
  
  /^(13|15|18)[0-9]\d{8}$/  ~~~ 目前使用的，全线匹配手机号码段，只要是13 15 18开头其他不要求哈
  /^((13|15|18)[0-9])\d{8}$/
  /^(13[0-9]|15[0-9]|18[0-9])\d{8}$/
  /^(13[0-9]|15[0|3|6|7|8|9]|18[7|8|9|2])\d{8}$/
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/php-preg_match.html
published: true
post_date: 2011-06-13 10:09:27
---
<h1><a name="function.preg-match"></a>preg_match</h1>
<div><a name="AEN96047"></a>(PHP 3&gt;= 3.0.9, PHP 4 )</div>
preg_match -- 进行正则表达式匹配
<div><a name="AEN96050"></a></div>
<h2>说明</h2>
int <strong>preg_match</strong> ( string pattern, string subject [, array matches [, int flags]])

在 <tt><em>subject</em></tt> 字符串中搜索与 <tt><em>pattern</em></tt> 给出的正则表达式相匹配的内容。

如果提供了 <tt><em>matches</em></tt>，则其会被搜索的结果所填充。<tt>$matches[0]</tt> 将包含与整个模式匹配的文本，<tt>$matches[1]</tt> 将包含与第一个捕获的括号中的子模式所匹配的文本，以此类推。

<tt><em>flags</em></tt> 可以是下列标记：
<div><dl><dt>PREG_OFFSET_CAPTURE </dt><dd>如果设定本标记，对每个出现的匹配结果也同时返回其附属的字符串偏移量。注意这改变了返回的数组的值，使其中的每个单元也是一个数组，其中第一项为匹配字符串，第二项为其偏移量。本标记自 <tt>PHP 4.3.0</tt> 起可用。</dd></dl></div>
 

<dl></dl><tt><em>flags</em></tt> 参数自 <tt>PHP</tt> 4.3.0 起可用。 

<strong>preg_match()</strong> 返回 <tt><em>pattern</em></tt> 所匹配的次数。要么是 0 次（没有匹配）或 1 次，因为 <strong>preg_match()</strong> 在第一次匹配之后将停止搜索。<a href="http://www.yesky.com/imagesnew/software/php/zh/function.preg-match-all.html"><strong>preg_match_all()</strong></a> 则相反，会一直搜索到 <tt><em>subject</em></tt> 的结尾处。<span style="text-decoration: underline;">如果出错 <strong>preg_match()</strong> 返回 <tt><strong>FALSE</strong></tt></span>。
<h3>语法参考：<a href="http://www.google.com.hk/url?q=http://www.yesky.com/imagesnew/software/php/zh/function.preg-match.html&amp;sa=U&amp;ei=D2_1TYjwD4iGvgPIy5neBg&amp;ved=0CBgQFjAD&amp;usg=AFQjCNHjI4oVyqb9j7BTu-uza1jNLiUw6A" target="_blank"><strong>preg_match</strong></a></h3>
<span style="text-decoration: underline;">若省略参数 matches，则只是单纯地比对，找到则返回值为 true</span>。

虫曰：

一定要记住那个正则表达式外面还<strong><span style="text-decoration: underline;">有两个/括住</span></strong>！！消掉，这正则表达式就没作用了。。悲惨呀~~~就为了这两个/号，我整整花了周末一天半时间来调试。基础很关键呀。

这边提供测试可用的检查移动手机号码的正则表达式给需要的人哈：

preg_match(<strong>'/</strong>^(13\d{1}|15[03689])\d{8}$<strong>/</strong>',$mobile)

下面还有几组都是能用来检测手机号码的~~
/^(13|15|18)[0-9]\d{8}$/  ~~~ 目前使用的，全线匹配手机号码段，只要是13 15 18开头其他不要求哈
/^((13|15|18)[0-9])\d{8}$/
/^(13[0-9]|15[0-9]|18[0-9])\d{8}$/
 /^(13[0-9]|15[0|3|6|7|8|9]|18[7|8|9|2])\d{8}$/