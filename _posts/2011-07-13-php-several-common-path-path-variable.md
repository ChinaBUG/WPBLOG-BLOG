---
ID: 1605
post_title: PHP-几个常用的路径(Path)变量
author: ChinaBUG
post_excerpt: |
  print( $_SERVER['DOCUMENT_ROOT'] );//获得服务器文档根
  print( $_SERVER['PHP_SELF'] );//获得执行该代码的文件服务器绝对路径
  print( __FILE__ );//获得文件的文件系统绝对路径
  print( dirname(__FILE__) );//获得文件所在的文件夹路径
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/php-several-common-path-path-variable.html
published: true
post_date: 2011-07-13 13:57:08
---
ASP/ASP.NET用习惯了server.mappath()结果在PHP却不懂得怎么处理路径了噢~摘抄一下，留作记录~~你好我也好噢

代码：

print( $_SERVER['DOCUMENT_ROOT'] );//获得服务器文档根
print('&lt;br/&gt;');
print( $_SERVER['PHP_SELF'] );//获得<strong><span style="text-decoration: underline;">执行该代码</span></strong>的文件服务器绝对路径
print('&lt;br/&gt;');
print( __FILE__ );//获得文件的文件系统绝对路径
print('&lt;br/&gt;');
print( dirname(__FILE__) );//获得文件所在的文件夹路径
print('&lt;br/&gt;');

结果：

G:/PHPnow/htdocs
/index.php
G:\PHPnow\htdocs\core2_rcomp2p\model\iplocation\cmd.iplocation.php
G:\PHPnow\htdocs\core2_rcomp2p\model\iplocation

延伸：

//显示带有文件扩展名的文件名

echo basename($path);

//显示不带有文件扩展名的文件名

echo basename($path,".php");