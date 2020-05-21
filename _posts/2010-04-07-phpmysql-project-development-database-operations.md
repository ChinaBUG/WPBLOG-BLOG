---
ID: 407
post_title: >
  PHP+MySQL懒人项目开发-项目开发常用操作
author: ChinaBUG
post_excerpt: |
  PHP+MySQL懒人项目开发-项目开发常用操作
  1、数据库连接(connect database)
  2、检查数据表是否存在(check table exist)
  3、纪录读取(read records)
  4、纪录更新(updata records)
  5、记录删除(delet records)
  6、记录插入(insert records)
  7、SESSION的使用
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/phpmysql-project-development-database-operations.html
published: true
post_date: 2010-04-07 09:49:13
---
　　懒人要有懒人的方式，下面就是在项目开发中常用的操作，你只需要复制粘贴即可噢。

1、数据库连接(connect database) 

<code lang="php">&lt;?php
 //根据实际的服务器参数来填写，一般的localhost是不需要改变的，
 $conn=mysql_connect('localhost', 'demo', 'demo') or die("Can't connect to mysql host");
 //选择要使用的数据库
</code><code lang="php"> mysql_select_db('ipodmp') or die("Can't connect to DB");
?&gt;</code> 

<code lang="php">注：</code><code lang="php">一般情况我们都是吧这段代码写入到conn.php文件内，然后在每一页内引用之，用法为在每一页开头加入：
   </code><code lang="php">&lt;?php include_once "condb.php";?&gt;</code> 

<code lang="php">2、检查数据表是否存在(check table exist)</code> 

function table_exists ($table, $db) {
<code lang="php"> </code>$tables = mysql_list_tables ($db);
<code lang="php"> </code>while (list ($temp) = mysql_fetch_array ($tables)) {
<code lang="php"> <code lang="php"> </code></code> if ($temp == $table) { return TRUE; }
<code lang="php"> </code>}
<code lang="php"> </code> return FALSE;
}  

如下调用 

if (table_exists(test_table, my_database)) {
<code lang="php"> </code>echo "Yes the table is there.";
} 

3、纪录读取(read records) 

&lt;?php 
<code lang="php"> </code>$query = "SELECT * FROM class order by ID desc";  
<code lang="php"> </code>$result = mysql_query($query) or die(mysql_error());
<code lang="php"> </code>while($row = mysql_fetch_array($result)){
<code lang="php"> <code lang="php"> </code></code>echo $row['ID'];
<code lang="php"> <code lang="php"> </code></code>echo $row['Name'];
<code lang="php"> </code>}
?&gt; 

4、纪录更新(updata records) 

&lt;?php 
 $ID = $_POST['IDs'];
 $Content = $_REQUEST['content1'];
 $query = "UPDATE config SET Content = '$Content' WHERE ID =$ID";
 $result = mysql_query($query) or die(mysql_error());
 ?&gt; 

5、记录删除(delet records) 

 &lt;?php 
 $ID = $_REQUEST['IDs'];
 $query = "DELETE FROM class WHERE ID = $ID";
 $result = mysql_query($query) or die(mysql_error()); 
?&gt; 

6、记录插入(insert records) 

&lt;?php 
 $Name = $_REQUEST['names'];
 $Model = $_REQUEST['Model'];
 $Class = $_REQUEST['Class'];
 $query="INSERT INTO products(Name,Model,Class) VALUES ('$Name','$Model','$Class')";
 $result = mysql_query($query) or die(mysql_error());
?&gt; 

<strong><span style="text-decoration: underline;">备注：</span></strong>
　　I.4、5 、6记录的读取，更新，删除，插入操作整体上比ASP操作来的简洁，只需要这样一种语法，一种方式，而ASP却有好几种语法，好几种方式，参数还一堆。
　　II.个人觉得PHP中的语法比较简洁，对于开发时能加速，如上面的变量替换吧，很简洁，ASP写出来的代码就多了。

7、SESSION的使用

　　PHP的SESSION使用特别麻烦，因为他有设置开关，要用时，需要打开，要不用不了，谁知道能写一次代码，就不用次次都写吗？

　　要使用SESSION的时候，需要使用 session_start(); 来启用，然后再使用变量，如下

if($_SESSION['Login'] != true){
<code lang="php"> </code>header("location:index.php");
}