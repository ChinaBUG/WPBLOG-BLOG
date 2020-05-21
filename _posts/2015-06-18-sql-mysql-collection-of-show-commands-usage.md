---
ID: 4079
post_title: SQL-MySQL的show操作命令集合用法
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/06/sql-mysql-collection-of-show-commands-usage.html
published: true
post_date: 2015-06-18 11:29:37
---
~.~ 序 ~.~

早上，在使用SQl插入记录的时候有个需求，怎么才知道一张表里面字段是否为空？

大家都知道如果不为空的话，没有指定默认值的话，那么会出错的，经验表明很多情况，特别是升级或者转移网站的时候出现错误基本上都有默认值的一份功劳，所以，我们要养成好习惯，在代码里面指定好不为空的字段值，不能随便放空。

话说这个需求其实如果字段很少，很容易就能看出来的，要是字段多的话咋办呢？从结构里面查看眼睛有点吃力了，网上查一下发现了show命令，下面就简单介绍一下这个命令的语法跟作用吧。

用途：列出 MySQL Server 数据库
用法：SHOW DATABASES

用途：列出数据库数据表
用法：SHOW TABLES FROM db_name

用途：导出数据表结构
用法：SHOW CREATE TABLE tbl_name

用途：列出数据表及表状态信息
用法：SHOW TABLE STATUS FROM db_name

用途：列出资料表字段
用法：SHOW COLUMNS FROM tbl_name

用途：列出字段属性
用法：SHOW FIELDS FROM tbl_name

用途：列出字段及详情
用法：SHOW FULL COLUMNS FROM tbl_name

用途：列出字段完整属性
用法：SHOW FULL FIELDS FROM tbl_name

用途：列出表索引
用法：SHOW INDEX FROM tbl_name

用途：列出 DB Server 状态
用法：SHOW STATUS

用途：列出 MySQL 系统环境变量
用法：SHOW VARIABLES

用途：列出执行命令
用法：SHOW PROCESSLIST

用途：列出某用户权限
用法：SHOW GRANTS FOR user

以上命令经本人确认，请放心使用噢^_^

~.~ 正文 ~.~

了解了这些命令后，我们该提出我们的需求了：我们希望能得到一个表的“不为空”字段，简单说是必填的字段列表。

那么我们该用哪个命令呢？

在上面有好几个命令是可以实现的，我们就拿

SHOW COLUMNS FROM tbl_name

这个命令来操作吧

在phpmyadmin内SQL标签内输入这个命令，我们会得到表的全部字段的信息噢，然后，请将命令增加一个条件吧，修改如下：

SHOW COLUMNS FROM user WHERE `Null` = "NO"

区别就是增加一个Null不为NO的条件。为啥是Null不为NO呢？显然就是第一遍执行命令的作用，请仔细看看就明白了，咱不多做解释。