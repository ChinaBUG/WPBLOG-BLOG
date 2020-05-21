---
ID: 2464
post_title: >
  ShopEX-root.htaccess的伪静态配置实例
author: ChinaBUG
post_excerpt: |
  哈~今天分享一下公司目前用的.htaccess配置吧，不懂这玩意儿，所以都是同事自个研究写的，膜拜一下吧！对于高手，还望不吝赐教呀。
  说明：
  除了转向之外，做的最多的就是为了优化而设置的一些属性，将ShopEX的一些控制器全部都伪静态了，做成二级目录的形式，据说这样子比较有利SEO哈。
  其中要特别注意的是最后那个[QSA,NU,PT,L]不要修改噢，这个形式是解决ShopEX搜索乱码问题的，经测试是有效的，目前网上很多人问ShopEX伪静态之后搜索不出结果，或者在访问的时候会出现乱码的情况，请加上[QSA,NU,PT,L]这个参数，则有可能解决这个问题，目前我是这么解决的，你行不行也不妨碍你试试，对吧？！
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/shopex-root-htaccess-configuration-examples-for-xu-com.html
published: true
post_date: 2013-06-04 09:29:07
---
哈~今天分享一下公司目前用的.htaccess配置吧，不懂这玩意儿，所以都是同事自个研究写的，膜拜一下吧！对于高手，还望不吝赐教呀。
<blockquote><em>备注：</em>
<em>请自己查询一下服务器是否支持伪静态，具体查询如下：执行&lt;?php phpinfo()?&gt;，然后Ctrl+F查找“Loaded Modules”，其中列出了所有apache2handler已经开启的模块，如果里面包括“mod_rewrite”，则已经支持如果没有则不支持需要安装或者开启。</em></blockquote>
说明：

除了转向之外，做的最多的就是为了优化而设置的一些属性，将ShopEX的一些控制器全部都伪静态了，做成二级目录的形式，据说这样子比较有利SEO哈。

其中要特别注意的是最后那个[QSA,NU,PT,L]不要修改噢，这个形式是解决ShopEX搜索乱码问题的，经测试是有效的，目前网上很多人问ShopEX伪静态之后搜索不出结果，或者在访问的时候会出现乱码的情况，请加上[QSA,NU,PT,L]这个参数，则有可能解决这个问题，目前我是这么解决的，你行不行也不妨碍你试试，对吧？！
<pre># Helicon ISAPI_Rewrite configuration file
# Version 3.1.0.98

#$Id: root.htaccess 17348 2008-12-23 05:53:22Z flaboy $

#DirectoryIndex index.htm index.php index.html

RewriteEngine  on
# 设置不带www跳转到带www
RewriteCond %{HTTP_HOST} ^xu.com [NC]
RewriteRule ^(.*)$ http://www.xu.com/$1 [L,R=301]

# 设置RewriteBase的值为你的商店目录地址
RewriteBase /
RewriteRule ^tuan/$ groupon.php
RewriteRule ^tuan/tuangou-([0-9]+)\.html$ groupon-$1.php
RewriteRule ^tuan/(.*)([0-9]+)\.html$ groupon-$1-$2-item.php

RewriteRule ^buynow/$ miao.html
RewriteRule ^buynow/miaosha-([0-9]+)\.html$ miao-$1-detail.html

RewriteRule ^brand/$ brand-showList.html
RewriteRule ^brand/pinpai-([0-9]+)\.html$ brand-$1.html

RewriteRule ^credit/$ gift-showList.html
RewriteRule ^credit/list-([0-9]+)\.html$ gift-$1-showlist.html
RewriteRule ^credit/list-([0-9]+)-page-([0-9]+)\.html$ gift-$1-$2-showlist.html
RewriteRule ^credit/jifen-([0-9]+)\.html$ gift-$1.html

RewriteRule ^help/$ page-help.html
RewriteRule ^help/(.*)\.html$ page-$1.html

RewriteRule ^regimen/$ page-yangsheng.html
RewriteRule ^regimen/nvxingshiliao.html$ artlist-159.html
RewriteRule ^regimen/nvxingshiliao-([0-9]+)\.html$ artlist-159-$1.html

RewriteRule ^regimen/nanxingshiliao.html$ artlist-160.html
RewriteRule ^regimen/nanxingshiliao-([0-9]+)\.html$ artlist-160-$1.html

RewriteRule ^regimen/yuezizhongxin.html$ artlist-194.html
RewriteRule ^regimen/yuezizhongxin-([0-9]+)\.html$ artlist-194-$1.html

RewriteRule ^regimen/caiputuijian.html$ artlist-161.html
RewriteRule ^regimen/caiputuijian-([0-9]+)\.html$ artlist-161-$1.html

RewriteRule ^regimen/yangsheng-([0-9]+)\.html$ article-$1.html

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php?$1 [QSA,NU,PT,L]</pre>