---
ID: 1594
post_title: >
  ShopEX-数据库多了如何批量删除相同前缀的数据表
author: ChinaBUG
post_excerpt: |
  实例描述：
  ShopEX安装几次后，数据库的表变得好多，不同的前缀，都不知道怎么一个个删了，累死都要，还好程序还是可以自动完成的。
  下面的代码另存为一个PHP文件，然后运行即可，注意修改ShopEX的配置文件所在的路径，文件默认是放在根目录里面。
  代码的功能是：寻找不是当前系统使用的数据库，然后把这些数据库删除。
  特别注意：
  请注意，如果使用插件的话，请要保证插件新建的表前缀是跟当前系统定义的前缀一致，否则将被一起删除！
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-database-how-much-mass-delete-the-same-table-prefix.html
published: true
post_date: 2011-07-13 10:31:05
---
实例描述：

ShopEX安装几次后，数据库的表变得好多，不同的前缀，都不知道怎么一个个删了，累死都要，还好程序还是可以自动完成的。

下面的代码另存为一个PHP文件，然后运行即可，注意修改ShopEX的配置文件所在的路径，文件默认是放在根目录里面。

代码的功能是：寻找不是当前系统使用的数据库，然后把这些数据库删除。

特别注意：

<span style="text-decoration: underline; color: #ff0000;">请注意，如果使用插件的话，请要保证插件新建的表前缀是跟当前系统定义的前缀一致，否则将被一起删除！</span>

代码如下：

&lt;?php
/*数据库操作 - 开始*/
//引入ShopEX的配置文件，获取ShopEX的数据库参数定义
require('config/config.php');
//常规用法
$con = mysql_connect(DB_HOST,DB_USER,DB_PASSWORD);
if (!$con){die('Could not connect: ' . mysql_error());}
mysql_select_db(DB_NAME, $con);
/*数据库操作 - 结束*/
$rs = mysql_query('show tables');
while($arr = mysql_fetch_array($rs)){
$TF = strpos($arr[0],DB_PREFIX);
if($TF !== 0){//===全等；!==非全等，比较数值还比较类型
$FT=mysql_query("drop table $arr[0]");
if($FT){
echo "$arr[0] 删除成功！&lt;br&gt;";
}
}
}
?&gt;