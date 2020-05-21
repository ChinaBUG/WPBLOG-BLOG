---
ID: 213
post_title: 12条htaccess基础技巧
author: ChinaBUG
post_excerpt: |
  1.创建自定义错误页(Create a custom error page)
  2.禁止目录浏览(Prevent directory browsing)
  3.设置每个目录的启动文档(Set the default page of each directory)
  4.设置301重定向(Set up a 301 redirect)
  5.使用GZIP压缩输出文件(Compress file output with GZIP)
  6.重定向到一个加密的HTTPS的链接(Redirect to a secure https connection)
  7.禁止脚本引擎运行(Block script execution)
  8.强制下载的文件出现另存为对话框(Force a file to download with a “Save As” prompt)
  9.上传功能的限制设置(Restrict file upload limits for PHP)
  10.允许文件缓存功能(Enable File Caching)
  11.保护你的网站，防止盗链(Protect your site from hotlinking)
  12.掩饰你的文件类型(Disguise your file types)
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/12-basic-htaccess-tips.html
published: true
post_date: 2009-08-24 15:21:55
---
1.创建自定义错误页(Create a custom error page)

　　Linux Apache服务器的.htaccess文件让你很容易创建自定义错误页。现在创建你的自定义错误页并将下面代码加入到.htaccess文件中：

ErrorDocument 401 /401.php
ErrorDocument 403 /403.php
ErrorDocument 404 /404.php
ErrorDocument 500 /500.php

　　你可以替换代码中/500.php文件的名字及路径。

2.禁止目录浏览(Prevent directory browsing)

　　如果你的网站目录没有包含一个启动文档(如：index.html,index.htm,index.asp,index.php等等)，那么网站就会自动启动浏览目录的功能，这很危险，所以需要禁止，禁止方法也很简单，只需要在.htaccess中添加如下代码：

Options All -Indexes

3.设置每个目录的启动文档(Set the default page of each directory)

　　如果你不想使用默认的启动文档(如：index.html,index.htm,index.asp,index.php等等)，那么你可以指定你希望启动的任何文档，如一个关于文档，在.htaccess中添加如下代码：

DirectoryIndex news.html

　　当然，你可以将news.html文件改为任何你想要打开的文件。

4.设置301重定向(Set up a 301 redirect)

　　假如你修改了网站的结构想将旧的网址重定向到新的位置，下面的代码回帮到你：

Redirect 301 /original/filename.html http://domain.com/updated/filename.html

5.使用GZIP压缩输出文件(Compress file output with GZIP)

　　添加下面的代码，你将能够使用GZIP压缩全部的JavaScript，CSS和HTML文件。

&lt;IfModule mod_gzip.c&gt;
 mod_gzip_on   Yes
 mod_gzip_dechunk Yes
 mod_gzip_item_include file  \.(html?|txt|css|js|php|pl)$
 mod_gzip_item_include handler  ^cgi-script$
 mod_gzip_item_include mime  ^text\.*
 mod_gzip_item_include mime  ^application/x-javascript.*
 mod_gzip_item_exclude mime  ^image\.*
 mod_gzip_item_exclude rspheader ^Content-Encoding:.*gzip.*
&lt;/IfModule&gt;

6.重定向到一个加密的HTTPS的链接(Redirect to a secure https connection)

　　如果你想重定向使用一个加密的HTTPS链接，那么使用下面的代码：

RewriteEngine On
RewriteCond %{HTTPS} !on
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI}

7.禁止脚本引擎运行(Block script execution)

　　你可以排除一些文件不运行脚本引擎，比如html文件：

Options -ExecCGI
AddHandler cgi-script .pl .py .php .jsp. htm .shtml .sh .asp .cgi

　　你需要将禁止的文件类型添加到上面代码中。

8.强制下载的文件出现另存为对话框(Force a file to download with a “Save As” prompt)

　　如果你希望你网页内的需要下载的文件，不在浏览器中打开而是出现另存在对话框的话，请使用下面的代码：

AddType application/octet-stream .doc .mov .avi .pdf .xls .mp4

9.上传功能的限制设置(Restrict file upload limits for PHP)

　　你可以设置最大上传文件的大小，当然还有最大的运行时间，只需要你添加如下代码：

php_value upload_max_filesize 10M
php_value post_max_size 10M
php_value max_execution_time 200
php_value max_input_time 200

　　第一行是设置最大上传文件大小；第二行是设置POST数据的最大容量；第三行是最大的运行时间；第四行是设置最大的输入数据容量。

10.允许文件缓存功能(Enable File Caching)

　　允许文件缓存可以大大提高网站的性能和速度。使用下面的代码来设置缓存（根据你的网站来更改文件类型和时间值）：

#cache html and htm files for one day 
&lt;FilesMatch ".(html|htm)$"&gt; 
Header set Cache-Control "max-age=43200"
&lt;/FilesMatch&gt; 
 #cache css, javascript and text files for one week 
&lt;FilesMatch ".(js|css|txt)$"&gt; 
Header set Cache-Control "max-age=604800"
&lt;/FilesMatch&gt; 
 #cache flash and images for one month 
&lt;FilesMatch ".(flv|swf|ico|gif|jpg|jpeg|png)$"&gt; 
Header set Cache-Control "max-age=2592000"
&lt;/FilesMatch&gt; 
 #disable cache for script files 
&lt;FilesMatch "\.(pl|php|cgi|spl|scgi|fcgi)$"&gt; 
Header unset Cache-Control 
&lt;/FilesMatch&gt;

　　注意：时间单位为秒。

11.保护你的网站，防止盗链(Protect your site from hotlinking)

　　过去，你的网站内容含有的图片等信息会被其他人非法盗用，这很占用你的带宽，下面是一个阻止的办法：

RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://([ -a-z0-9] \.)?domain\.com [NC]
RewriteRule \.(gif|jpe?g|png)$ - [F,NC,L]

　　请将domain\.com替换成您的域名。

12.掩饰你的文件类型(Disguise your file types)

　　你可以掩饰你的文件类型，这样有利于网站的安全性，请插入下面代码：

ForceType application/x-httpd-php
<p style="text-indent:2em">英文原版：<a href="http://www.noupe.com/php/htaccess-techniques.html" target="_blank">The Definitive Guide to htaccess Techniques</a></p>