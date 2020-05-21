---
ID: 2827
post_title: ShopNC-错误信息列表及解决方案
author: ChinaBUG
post_excerpt: |
  1、Db Error: Unknown database 'shopnc'
  2、Access denied for user 'root'@'localhost' (using password: YES)
  3、Db Error: Table 'shopnc_activity' doesn't exist
  4、Fatal error: Incompatible file format: The encoded file has format major ID 65540, whereas the Optimizer expects 2 in E:\PHPnow\127.0.0.4_ShopNC\index.php on line 0
  5、Zend Guard Run-time support missing!
  6、Db Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'Microsoft YaHei', 'Lucida Grande', 'Lucida Sans Unicode', Tahoma, Helvetica, Ari' at line 1
  7、Db Error: Duplicate entry '1' for key 1
  8、Db Error: Unknown column 'w' in 'field list'
  9、Fatal error: This file has expired. in D:\APMServer5.2.6\www.htcdocs\index.php on line 0
  10、Db Error: database connect failed
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopnc-list-of-error-messages-and-solutions.html
published: true
post_date: 2013-11-18 10:42:24
---
如果你觉得本文的内容还不够的话，推荐你阅读实操《<a title="ShopNC-商城使用过程中出现的一些问题及解答" href="http://blog.ipodmp.com/archives/shopnc-mall-in-use-some-of-the-questions-and-answers/">ShopNC-商城使用过程中出现的一些问题及解答</a>》一文，收集使用ShopNC过程的点点滴滴的问题。

<strong>1、Db Error: Unknown database 'shopnc'</strong>

提示原因：提示未知的数据库名。

错误分析：提示这个的话，证明系统没有安装ShopNC或者是ShopNC系统依赖的数据库没有新建成功，这种情况不常见，如果出现就是自己误操作，把数据库给删除了，或者换地方的时候这个数据库漏掉了，还有可能就是现在使用的数据库名字跟实际的不一致造成的，什么情况那就自己分析了，总的一句话，数据库不存在，管你是什么原因，新建就解决。

解决方案：进入install目录，将lock文件改名，然后再运行一次就会自动进入安装的界面了，重新安装新建数据库即可。。

<strong>2、Access denied for user 'root'@'localhost' (using password: YES)</strong>

提示原因：提示用户权限获取错误

错误分析：一般出现这个提示的话，那么十有八九就是自己的MySQL数据库的密码，账号不正确了。

解决方案：请修改正确的账号跟密码。

<strong>3、Db Error: Table 'shopnc_activity' doesn't exist</strong>

提示原因：提示shopnc_****这一张表不存在，这个表可能是ShopNC系统本身的表也可能是插件或者是二开的数据表。

错误分析：一旦提示这个的话，只要确认是安装好的系统，那么就肯定是自己安装的二开的内容调用的表没有导入，这种情况发生错误的居多。

解决方案：请根据提示的shopnc_****这个表明，假设是上面的'shopnc_activity'那么只需要查找数据库看看是否存在此表，不存在则新建。

4、<b>Fatal error</b>: Incompatible file format: The encoded file has format major ID 65540, whereas the Optimizer expects 2 in <b>E:\PHPnow\127.0.0.4_ShopNC\index.php</b> on line <b>0</b>

提示原因：这个是解码格式不正确造成的错误。

错误分析：一般的情况，大家开发的程序是向下兼容的，所以不存在环境版本的问题，但是ShopNC他比较特殊，区分不同的平台版本，他有对坏境作要求。下面内容摘自ShopNC官方说明
<blockquote>ShopNC商城系统需要服务器上装有如下软件
- 主流WEB服务器（如Apache、Nginx、IIS等）
- PHP 5.4（需要安装 Zend Guard Loader 6.0.0）<span style="color: red;">推荐</span>
- PHP 5.3（需要安装 Zend Guard Loader 5.5.0）
- MySQL 5.2 及以上的数据库环境</blockquote>
解决方案：如果你发现你下载的最新版本的ShopNC的话，安装会出现这个提示的话，那么请检查一下您的PHP环境，看看PHP的版本是不是5.3以上版本另外Zend Guard Loader 5.5.0以上版本噢，如果不是那么就赶紧更新吧。

其他说明请看本博客的 <a title="ShopNC-Fatal error: Incompatible file format: The encoded file has format major ID 65540, whereas the Optimizer expects 2" href="http://blog.ipodmp.com/archives/shopnc-fatal-error-incompatible-file-format-the-encoded-file-has-format-major-id-65540-whereas-the-optimizer-expects-2/">ShopNC-Fatal error: Incompatible file format: The encoded file has format major ID 65540, whereas the Optimizer expects 2</a>一文。

<strong>5、Zend Guard Run-time support missing!</strong>

提示原因：Zend Guard运行时支持失败，调用Zend Guard的解码功能失败。

错误分析：没有安装Zend Guard的程序，目前PHP加密的程序基本上需要使用Zend Optimizer或者<em>Zend Guard</em> Loader的。

解决方案：根据提示区安装呗，使用一键安装环境的，那就找找看有没有这个组件，没有的话那就只有更换了。

附加资源：懒得去注册下载的朋友可以直接点击下载
<a href="http://pan.baidu.com/s/1quZZL">ZendGuardLoader-php-5.3-Windows.zip 文件大小:66.6K</a>
<a href="http://pan.baidu.com/s/1CFCOi">ZendGuardLoader-70429-PHP-5.4-Windows-x86.zip 文件大小:65.32K</a>

<strong>6、Db Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'Microsoft YaHei', 'Lucida Grande', 'Lucida Sans Unicode', Tahoma, Helvetica, Ari' at line 1</strong>

提示来源：ShopNC兴趣群内提问。作者说：我现在存在的问题是。店铺商品编辑的时候如果使用html的话。会出现跟上面类似的错误。

提示原因：这是SQL语法错误造成的提示，但是以You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near开头的基本上都是SQL语法错误。

错误分析：根据提示说明，告诉我们在'Microsoft YaHei', 'Lucida Grande'....等信息附近有SQL语句错误，既然告诉你了你觉得能怎么办？

解决方案：遇上这种SQL语法错误的是最好解决的，因为已经告诉你错误的所在了。那么，你只要在调用这句语句的时候直接输出这个语句，然后找到提示的位置慢慢找过去就可以解决了。一般出现上面这种情况，都是引号关系没处理好的，重点排查即可。

<strong>7、Db Error: Duplicate entry '1' for key 1</strong>

提示来源：数据库存储的时候

提示原因：重复的关键键值

错误分析：开发的不规范造成的，已经存的数据重复存储

解决方案：程序中更换该键值即可。当然，删除此键值的记录也是可以的。

错误附加：

2014.04.07 "Db Error: Duplicate entry '940' for key 'adv_id'" 请查找adv_id这个值为940的记录，删除或者换成其他的数字，或者查找调用的地方将写入的值改掉即可。

<strong>8、Db Error: Unknown column 'w' in 'field list'</strong>

提示来源：数据库查询的时候

提示原因：调用的程序调用不正确的方法或者指定不正确的参数

错误分析：开发的不规范造成的

解决方案：程序中更换该键值即可。

<strong>9、Fatal error: This file has expired. in D:\APMServer5.2.6\www.htcdocs\index.php on line 0</strong>

提示来源：安装程序时或者新装插件时

提示原因：这个提示说明调用的文件是用zend加密的，而且加密的时候还设置了过期时间的限制，所以这个提示文件过期了。

错误分析：没设好分析的，这个是程序加密的手段。

解决方案：更新到最新的版本文件或者破解了，那么就没有这个限制了。

<strong>10、Db Error: database connect failed</strong>

提示来源：数据库操作时均可能会出现

提示原因：字面上的意思就是数据库连接失败。

错误分析：既然是数据库连接失败那么有两个方面的原因：一数据库信息指定不正确，多发生在更换服务器等情况下，二是数据库服务器不稳定造成的错误

解决方案：数据库信息指定不正确这个是低级问题，请自行检查。服务器不稳定的话，解决一下稳定的问题即可。

错误延伸：曾经遇到一个案例是，本地使用的LAMP程序多套，而MySQL服务器自然也就多套了，结果互相冲突！造成的服务器不稳定，所以如果有此种现象的则请关闭一个MySQL服务器即可，至于线上的话则需要找管理员解决，一般情况自己是没办法处理的。

11、<strong>Db Error: Unknown column 'addtime' in 'where clause'</strong>

提示来源：后台订单管理内的订单按日期导出

提示原因：条件语句内有未知的<strong>addtime</strong>字段

错误分析：从这个提示中我们可以看出其实是系统的SQL写错了，查看数据库结构会发现没有addtime字段，但是有add_time字段。

解决方案：直接找到代码所在地方查询addtime修改为add_time就可以了。

12、......