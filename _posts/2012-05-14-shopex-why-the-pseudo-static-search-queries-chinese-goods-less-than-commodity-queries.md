---
ID: 2047
post_title: >
  ShopEX-为什么伪静态后搜索查询中文商品名会查询不到商品
author: ChinaBUG
post_excerpt: |
  RewriteRule ^(.*)$ index.php?$1 [L]
  改为
  RewriteRule ^(.*)$ index.php?$1 [QSA,NU,PT,L]
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/shopex-why-the-pseudo-static-search-queries-chinese-goods-less-than-commodity-queries.html
published: true
post_date: 2012-05-14 13:39:43
---
ShopEX 搜索乱码 - 启用伪静态后中文搜索乱码完美解决方案

现在shopex的伪静态规则都是适合rewrite3.0以上的，而国内普遍的都是1.3版本的自定义伪静态。

ISAPI_Rewrite 3_0070以后的版本对中文的处理会出现乱码，也就是shopex很多人出现的前台中文搜索出现乱码。

之前的一篇文章如果大家看过了就知道怎么解决了：

ISAPI_Rewrite 3.1 伪静态中文URL乱码的解决方案

默认的伪静态规则：

# Helicon ISAPI_Rewrite configuration file

# Version 3.1.0.73

RewriteBase /

RewriteCond %{REQUEST_FILENAME} \.(html|xml|json|htm|php|php2|php3|php4|php5|phtml|pwml|inc|asp|aspx|ascx|jsp|cfm|cfc|pl|cgi|shtml|shtm|phtm|xml)$

RewriteCond %{REQUEST_FILENAME} !-f

RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule ^(.*)$ index.php?$1 [L]

这个是正常的规则，在0070之前的版本都没问题的，之后就都出现了中文乱码的问题。

解决方法:把最后一句的规则添加个NU参数，RewriteRule ^(.*)$ index.php?$1 [QSA,NU,PT,L]

其实主要是NU这个，加上去就不会乱码，但直接显示的中文。QSA,PT这2个加上去和[NU,L]显示有点不一样，具体自己测试看看。

当然可以说是组件问题也可以说是规则问题，但是基本都是规则问题。包括以前的1.3版本，只是没人提供相关规则。