---
ID: 1385
post_title: >
  SQL-如何在SQL语句中使用字符操作函数LEFT,RIGHT,SUBSTR,LENGTH,CHARINDEX,CONCAT,REPLACE及用法
author: ChinaBUG
post_excerpt: |
  UPDATE sdb_goods SET bn = right(bn,Length(bn)-1)
  
  同时，还能使用的函数如下：
  
  1、LEFT：从左截取
  
  LEFT(string,length)
  
  2、RIGHT：从右截取
  
  RIGHT(string,length)
  
  3、SUBSTR：
  
  MySQL: SUBSTR(), SUBSTRING() 、Oracle: SUBSTR() 、SQL Server: SUBSTRING()
  
  SUBSTR(str,pos): 由<str>中，选出所有从第<pos>位置开始的字元。请注意，这个语法不适用于SQL Server上。
  
  SUBSTR(str,pos,len): 由<str>中的第<pos>位置开始，选出接下去的<len>个字元。
  
  4、LENGTH(string):
  5、CHARINDEX ( expression1 , expression2 [ , start_location ] )：
  6、CONCAT(str1,str1,str1,...)
  7、Replace(str1, str2, str3)
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/sql-how-to-use-the-characters-in-the-sql-statement-manipulation-functions-left-right-substr-length-charindex.html
published: true
post_date: 2011-06-21 18:19:52
---
上次，经理要求把商城的商品的货号给批量删除开头的字母，一个个的删除，累都累死了，用SQL怎么操作呢？

经经验表明是可以滴。

经过研究发现，还真的可以，又学到了一些哈，SQL是有字符操作的函数滴。

下面为用法：

商品货号是A201106211234这样的形式，要把A删除，保留后面的数字，所以只要把第一个字符给截取掉就行了。

UPDATE sdb_goods SET bn = right(bn,Length(bn)-1)

同时，还能使用的函数如下：

1、LEFT：从左截取

LEFT(string,length)

2、RIGHT：从右截取

RIGHT(string,length)

3、SUBSTR：

MySQL: SUBSTR(), SUBSTRING() 、Oracle: SUBSTR() 、SQL Server: SUBSTRING()

SUBSTR(str,pos): 由&lt;str&gt;中，选出所有从第&lt;pos&gt;位置开始的字元。请注意，这个语法不适用于SQL Server上。

SUBSTR(str,pos,len): 由&lt;str&gt;中的第&lt;pos&gt;位置开始，选出接下去的&lt;len&gt;个字元。

4、LENGTH(string):

字符串的长度。MySQL: LENGTH() 、Oracle: LENGTH() 、SQL Server: LEN()

5、CHARINDEX ( expression1 , expression2 [ , start_location ] )：

函数返回字符或者字符串在另一个字符串中的起始位置

6、CONCAT(字串1, 字串2, 字串3, ...)

将字段联合起来，如CONCAT('http://shopex.ipodmp.com/product-',goods_id,'.html')

7、Replace(str1, str2, str3):

在字串 str1 中，當 str2 出現時，將其以 str3 替代。

case when 实现定值排序：

1：order by case [title] when '好友' then 1 else 2 end , id desc

2：order by case when [title] like '%好友%' then 1 else 2 end , id desc