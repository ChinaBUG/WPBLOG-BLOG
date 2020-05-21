---
ID: 2171
post_title: 'PHP-failed to open stream: No such file or directory'
author: ChinaBUG
post_excerpt: |
  Warning: require(./ThinkPHP//ThinkPHP.php) [function.require]: failed to open stream: No such file or directory in D:\WebSites\bobing\admin.php on line 8
  
  Fatal error: require() [function.require]: Failed opening required './ThinkPHP//ThinkPHP.php' (include_path='.;C:\php5\pear') in D:\WebSites\bobing\admin.php on line 8
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/php-failed-to-open-stream-no-such-file-or-directory.html
published: true
post_date: 2012-12-14 21:29:43
---
最近在做ThinkPHP新浪微博之博饼应用开发的时候，不知道是什么神奇的，老是出现下面的问题，真是令人烦恼呀。

<strong>Warning</strong>: require(./ThinkPHP//ThinkPHP.php) [<a href="function.require">function.require</a>]: failed to open stream: No such file or directory in <strong>D:\WebSites\bobing\admin.php</strong> on line <strong>8</strong>

<strong>Fatal error</strong>: require() [<a href="function.require">function.require</a>]: Failed opening required './ThinkPHP//ThinkPHP.php' (include_path='.;C:\php5\pear') in <strong>D:\WebSites\bobing\admin.php</strong> on line <strong>8</strong>

好吧，为了清净一点，直接用下面的代码解决吧

define('ROOT_PATH',dirname(__file__));
define('THINK_PATH',ROOT_PATH.'/ThinkPHP/');

有同样情况的朋友请尝试一下吧。