---
ID: 42
post_title: unexpected T_VARIABLE/T_STRING
author: ChinaBUG
post_excerpt: |
  Parse error: parse error, unexpected T_VARIABLE in XXXX.PHP on line XX
  Parse error: parse error, unexpected T_STRING in XXXX.PHP on line XX
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/unexpected-t_variablet-t_string.html
published: true
post_date: 2009-08-04 10:40:34
---
<blockquote>Parse error: parse error, unexpected T_VARIABLE in XXXX.PHP on line XX

Parse error: parse error, unexpected T_STRING in XXXX.PHP on line XX</blockquote>
 　　这种错误一般是解析错误。

　　这个错误信息是说，在不该出现变量的地方出现了变量。

　　错误是由上一个非空行引起的，多半是缺少行结束符。

　　意外的T_VARIABLE/T_STRING，T_VARIABLE/T_STRING是php内部标识。