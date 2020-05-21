---
ID: 1690
post_title: >
  密码保护：ShopEX-研究日记-ShopEX系统中怎么单独取消缓存机制(单个/项不缓存)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-diary-shopex-system-of-how-to-cancel-a-separate-caching-mechanism-single-entries-are-not-cached.html
published: true
post_date: 2011-07-18 16:33:59
---
请在类内区域设置

var $noCache = true;

或者在方法中设置

$this-&gt;noCache = true;

即可，那个文件不要缓存就写那个文件。