---
ID: 3651
post_title: >
  SQL-怎么批量修改表的表名与表的前缀
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/09/sql-how-to-bulk-modify-table-table-name-and-table-prefix.html
published: true
post_date: 2014-09-29 15:19:48
---
今天，再看<a href="http://www.josiahcole.com/2012/03/04/wordpress-security-best-practices/">WordPress Security Best PracticesPosted</a> 的时候，第一条：改变$table_prefix变量。里面说，这个不要使用默认的，要不黑客们将会使用这个默认的前缀来猜测我们的表，并注入噢。

一看自己的，还好有改哈~那如果没有改，博客已经运行好久了怎么改噢？

所以就有下面的内容了。

第一种方法：

你要是使用pgpmyadmin这个超牛逼的管理工具的话，那么请直接使用，我使用的是：版本信息: 4.0.10.2，最新稳定版本： 4.2.9这一版的，然后你选择表之后再下面批量操作项那边有下拉框可以选择，可以选择：添加表前缀、修改表前缀、复制表为新前缀。如下图

<a href="http://blog.ipodmp.com/wp-content/uploads/2014/09/rename_db.jpg"><img class="alignnone size-full wp-image-3655" src="http://blog.ipodmp.com/wp-content/uploads/2014/09/rename_db.jpg" alt="修改表前缀" width="608" height="687" /></a>

第二种方法：

上面的简单的方法你没有的话，或者没有机会拥有，那么只能自己DIY了，请将下面的代码新建一个PHP文件，保存运行吧，也是可以的。记得修改里面的参数，如果连这个都不会的话，恩，你还是付费支持吧。

<strong><em>注：代码已经修改测试了，请放心使用~</em></strong>

&lt;?php

header('Content-Type: text/html; charset=utf-8');
//设置好相关信息
$dbserver='localhost';//连接的服务器一般为localhost
$dbname='wordpress';//数据库名
$dbuser='ChinaBUG';//数据库用户名
$dbpassword='597939146';//数据库密码
$old_prefix='wp_';//数据库的前缀
$new_prefix='wp2014_';//数据库的前缀修改为
//
if (!is_string($dbname) || !is_string($old_prefix)|| !is_string($new_prefix) ){
return false;
}
if (!mysql_connect($dbserver, $dbuser, $dbpassword)) {
print 'Could not connect to mysql';
exit;
}
//取得数据库内所有的表名
$result = mysql_list_tables($dbname);
if (!$result) {
print "DB Error, could not list tables\n";
print 'MySQL Error: ' . mysql_error();
exit;
}
//把表名存进$data
while ($row = mysql_fetch_row($result)) {
$data[] = $row[0];
}
//过滤要修改前缀的表名
foreach($data as $k =&gt; $v){
$preg = preg_match("/^($old_prefix{1})([a-zA-Z0-9_-]+)/i", $v, $v1);
if($preg){
$tab_name[$k] = $v1[2];
}
}

if($tab_name){
foreach($tab_name as $k =&gt; $v){
$sql = 'RENAME TABLE `'.$old_prefix.$v.'` TO `'.$new_prefix.$v.'`';
mysql_query($sql);
}
print  数据表前缀：.$old_prefix."&lt;br&gt;".已经修改为：.$new_prefix."&lt;br&gt;";
}else{
print 您的数据库表的前缀.$old_prefix.输入错误。请检查相关的数据库表的前缀;
if ( mysql_free_result($result) ) {return true;}
}
?&gt;