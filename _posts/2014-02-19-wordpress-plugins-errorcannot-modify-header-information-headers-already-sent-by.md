---
ID: 3185
post_title: 'wordpress-plugins error:Cannot modify header information &#8211; headers already sent by'
author: ChinaBUG
post_excerpt: |
  手机模板插件wptouch更新之后在我的主页调用时会出现下面信息：
  Warning: Cannot modify header information - headers already sent by (output started at /home/ueiunezg/domains/ipodmp.com/public_html/index.php:1) in /home/ueiunezg/domains/ipodmp.com/public_html/blog/wp-content/plugins/wptouch/core/class-wptouch-pro.php on line 406
layout: post
permalink: >
  http://blog.ipodmp.com/2014/02/wordpress-plugins-errorcannot-modify-header-information-headers-already-sent-by.html
published: true
post_date: 2014-02-19 17:09:23
---
手机模板插件<b>wptouch</b>更新之后在我的主页调用时会出现下面信息：

<b>Warning</b>: Cannot modify header information - headers already sent by (output started at /home/ueiunezg/domains/ipodmp.com/public_html/index.php:1) in <b>/home/ueiunezg/domains/ipodmp.com/public_html/blog/wp-content/plugins/wptouch/core/class-wptouch-pro.php</b> on line <b>406</b>

我的主页调用的是wordpress的wp-load接口，以前用的好好的，不知道为什么哈~
<blockquote><cite>&lt;?php define('WP_USE_THEMES', false);require('blog/wp-load.php');?&gt;</cite></blockquote>
按照提示找到<b>class-wptouch-pro.php</b>文件的406行，然后会发现有一个

setcookie( WPTOUCH_CACHE_COOKIE, $cookie_value, time() + 3600, '/' );

调用，从PHP手册中我们很明显就知道setcookie这方法是需要在全部输出之前做输出的，出现这个提示证明在此之前有输出了，那要找那些输出的话很麻烦噢。。。咋办呢？

我发现在我的blog.ipodmp.com站内，没有加载wp-load接口的时候是不提示错误的。证明代码是没多大问题的。

我想，反正只有主页的时候错误，那就简单一下吧。

因为调用的时候有设置<cite>WP_USE_THEMES</cite>这个常量，那么我直接在406行增加如下代码：

if(WP_USE_THEMES!=false)

然后原先的代码就变成了
<blockquote>if(WP_USE_THEMES!=false)
setcookie( WPTOUCH_CACHE_COOKIE, $cookie_value, time() + 3600, '/' );</blockquote>
然后保存，然后问题就解决了。。。哈~~