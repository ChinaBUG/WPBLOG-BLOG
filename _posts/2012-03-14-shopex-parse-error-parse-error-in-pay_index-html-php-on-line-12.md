---
ID: 1982
post_title: 'ShopEX-Parse error: parse error in &#8230;pay_index.html.php on line 12'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/03/shopex-parse-error-parse-error-in-pay_index-html-php-on-line-12.html
published: true
post_date: 2012-03-14 17:47:50
---
现一例shopex系统由4.8.4升级到4.8.5的，再点击 支付方式 时会出现如下错误信息：

Parse error: parse error in D:\wwwroot\home\cache\front_tmpl\388719dc6281d3a88b622a85cd70ba4fpay_index.html.php on line 12

从提示信息上看这个是告知我们这个文件里面有语法错误。那么到底是什么语法错误呢？不知道，在shopex系统中，如果出现这种错误的话，一般都是只需要清空缓存即可解决问题，但也有例外，比如这一例。

那么到底是什么问题呢？

经过一步步的查找才发现，是代码跟数据库的错误。

经过将近1小时的排查，终于解决问题了。