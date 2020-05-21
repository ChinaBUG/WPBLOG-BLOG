---
ID: 3850
post_title: >
  mypic-图片相册系统的伪静态修改
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/01/mypic-photo-album-system-pseudo-static-modify.html
published: true
post_date: 2015-01-05 10:07:42
---
<em>虫神曰：最近，台湾朋友定制mypic这个图片相册系统，做了一点二次开发的内容，需要用到伪静态支持，修改好，分享一下吧，省的自己时间长了忘记了。后面会抽空将这个系统的一些修改发上来分享的。</em>

关于mypic这套系统，请自行问度娘，咱就不多说了，下面就是修改后的.htaccess文件的内容，其实就是增加2行规则哈~


[php]# Turn on URL rewriting
RewriteEngine On

# Installation directory
RewriteBase /

# Protect application and system files from being viewed
RewriteRule ^(template|data|model) - [F,L]

# 2014.12.30 ChinaBUG
RewriteRule ^pageu/(\w*).html$ index.php?page-read-url-$1.html
RewriteRule ^page/(\d*).html$ index.php?page-read-id-$1.html

# Allow any files or directories that exist to be displayed directly
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

# Rewrite all other URLs to index.php?URL
RewriteRule .* index.php?$0 [PT,L]

[/php]