---
ID: 2004
post_title: 'ShopEX-Warning: array_merge() [function.array-merge]: Argument #1 is not an array in'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/03/shopex-warning-array_merge-function-array-merge-argument-1-is-not-an-array-in.html
published: true
post_date: 2012-03-30 16:49:48
---
<strong>Warning</strong>: array_merge() [<a href="http://localhost/shopadmin/function.array-merge">function.array-merge</a>]: Argument #1 is not an array in <strong>G:\PHPnow\htdocs\core\admin\smartyplugin\input.object.php</strong> on line <strong>14</strong>

在做ShopEX商城二次开发，遇到设计一个挂件的情况，结果出现这个错误~

为什么呢？

查看了代码才发觉是缺少了一个参数，很神奇的是这个参数为空也行，真不知道为什么缺少就不行呢？