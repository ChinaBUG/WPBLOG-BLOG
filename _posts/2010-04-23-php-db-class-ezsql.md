---
ID: 490
post_title: PHP DB 类库 ezSQL
author: ChinaBUG
post_excerpt: 'WordPress 使用的数据库操作类就是它 -- ezSQL'
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/php-db-class-ezsql.html
published: true
post_date: 2010-04-23 15:21:55
---
WordPress 使用的数据库操作类就是它 -- ezSQL
我用了好多年了，我特别喜欢它的几个类方法，可以有效提高代码简洁度。

<strong>ezSQL 下载地址：</strong>
下载 : <a href="http://www.21andy.com/blog/download/ez_sql_2.05.zip">最好的 PHP DB 类 ezSQL</a> 2010-04-13 (60.53 KB, 已下载 36 次)

新版本是2.05添加了很多支持，包括 CodeIgniter,MSSQL, PDO 等等
我之前也为 CodeIgniter 写过一次，不过只支持 MySQL

看看使用示例，详细的文档请看<a rel="nofollow" href="http://justinvincent.com/docs/ezsql/ez_sql_help.htm" target="_blank">这里</a>

其实也没什么难度，直接看源代码即可，主要是程序设计的思想很好。

Example 1
----------------------------------------------------

// Select multiple records from the database and print them out..
$users = $db-&gt;get_results("SELECT name, email FROM users");
foreach ( $users as $user ) {
            // Access data using object syntax
            echo $user-&gt;name;
            echo $user-&gt;email;
}

Example 2
----------------------------------------------------

// Get one row from the database and print it out..
$user = $db-&gt;get_row("SELECT name,email FROM users WHERE id = 2");
echo $user-&gt;name;
echo $user-&gt;email;

Example 3
----------------------------------------------------

// Get one variable from the database and print it out..
$var = $db-&gt;get_var("SELECT count(*) FROM users");
echo $var;

Example 4
----------------------------------------------------

// Insert into the database
$db-&gt;query("INSERT INTO users (id, name, email) VALUES (NULL,'justin','jv@foo.com')");

Example 5
----------------------------------------------------

// Update the database
$db-&gt;query("UPDATE users SET name = 'Justin' WHERE id = 2)");

Example 6
----------------------------------------------------

// Display last query and all associated results
$db-&gt;debug();

Example 7
----------------------------------------------------

// Display the structure and contents of any result(s) .. or any variable
$results = $db-&gt;get_results("SELECT name, email FROM users");
$db-&gt;vardump($results);

Example 8
----------------------------------------------------

// Get 'one column' (based on column index) and print it out..
$names = $db-&gt;get_col("SELECT name,email FROM users",0)
foreach ( $names as $name ) {
    echo $name;
}

Example 9
----------------------------------------------------

// Same as above ‘but quicker’
foreach ( $db-&gt;get_col("SELECT name,email FROM users",0) as $name ) {
    echo $name;
}

Example 10
----------------------------------------------------

// Map out the full schema of any given database and print it out..
$db-&gt;select("my_database");
foreach ( $db-&gt;get_col("SHOW TABLES",0) as $table_name ) {
    $db-&gt;debug();
    $db-&gt;get_results("DESC $table_name");
}
$db-&gt;debug();

摘自 <a href="http://www.21andy.com/blog/20100413/1863.html" target="_blank">Andy's Blog</a>