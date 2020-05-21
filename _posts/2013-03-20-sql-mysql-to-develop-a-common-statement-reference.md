---
ID: 2397
post_title: SQL-MySQL开发常用的语句参考
author: ChinaBUG
post_excerpt: |
  1、在MySQL中怎么获取随机的记录？
  2、在MySQL之中怎么按照in语句的参数顺序来排序呢？
  3、在MySQL里面用代码获取表的全部字段
  4、SQL UNION 和 UNION ALL 操作符
  5、字符串序列要怎么查询呢，使用find_in_set查询字符串？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/03/sql-mysql-to-develop-a-common-statement-reference.html
published: true
post_date: 2013-03-20 12:06:24
---
1、在MySQL中怎么获取随机的记录？

SELECT* FROM <span style="color: #ff0000;">cdb_members_2</span> AS t1 JOIN (
<div>

SELECT ROUND( RAND()* ( (
<div>SELECT MAX(<span style="color: #ff00ff;"> member_id</span> ) FROM <span style="color: #ff0000;">cdb_members_2</span> )- ( SELECT MIN( <span style="color: #ff00ff;">member_id</span> ) FROM <span style="color: #ff0000;">cdb_members_2</span> ))+ ( SELECT MIN( <span style="color: #ff00ff;">member_id</span> ) FROM <span style="color: #ff0000;">cdb_members_2</span> )</div>
) AS id

</div>
) AS t2 WHERE t1.<span style="color: #ff00ff;">member_id</span> &gt;= t2.id ORDERBY t1.member_id LIMIT <span style="text-decoration: underline;"><span style="color: #003366; text-decoration: underline;">10</span></span>

说明：上面语句总的cdb_members_2为需要查询的数据表名， member_id为表的自增长字段，10代表随机获取的记录条数。

2、在MySQL之中怎么按照in语句的参数顺序来排序呢？

SELECT *
FROM cdb_goods
WHERE goods_id
IN (<span style="color: #ff0000;"> 90, 75, 60, 95</span>)
ORDER BY instr(',<span style="color: #ff0000;">90,75,60,95</span>,', CONCAT(',', goods_id,','))
LIMIT 0 , 10

说明：IN语句里面的请直接复制到instr函数内，<span style="text-decoration: underline;">记得前后需要增加一个分号</span>，为什么这样子我是不懂了，不过只有这样才能通过噢。

附注：
2013.09.16
instr(“被查找的字符串”,“要查找的字符串”)：返回“要查找的字符串”在“被查找的字符串”中第一次出现的位置，如果不存在则显示0，索引从1开始算。
concat(“字符串1”，“字符串2”，“字符串3”，....“字符串N”)：将字符串全部合并在一起。
<span style="text-decoration: underline;"><span style="color: #ff6600; text-decoration: underline;">如果字段编号是字符串怎么办呢？</span></span>其实就是在IN ( 90, 75, 60, 95)每一项增加一个引号，在CONCAT(',', goods_id,',')里面修改为CONCAT(',<strong><span style="color: #ff6600;">"</span></strong>', goods_id,'<strong><span style="color: #ff6600;">"</span></strong>,')即可。

备注：其他的数据库~未测试，请自行测试

Access<strong>:</strong>select * From 表 Where id in(1,5,3) order by instr(',1,5,3,',','&amp;id&amp;',')
MSSQL<strong>:</strong>select * From 表 Where id in(1,5,3) order by charindex(','+rtrim(cast(id as varchar(10)))+',',',1,5,3,')

3、在MySQL里面用代码获取表的全部字段

<code>SELECT COLUMN_NAME
FROM information_schema.columns
WHERE table_name = "<span style="color: #ff0000;">sdb_app4ordercrm_crm</span>"</code>

4、SQL UNION 和 UNION ALL 操作符

如果你有两张一摸一样的表，你想要获取两张表的内容怎么办？使用SQL UNION 和 UNION ALL 操作符就可以实现了！

参考：<a href="http://www.w3school.com.cn/sql/sql_union.asp">SQL UNION 和 UNION ALL 操作符</a>

5、字符串序列要怎么查询呢，使用find_in_set查询字符串？

我们字段存储格式如果是：1,2,3,...类型的，要怎么很快的查询出来呢？MySQL其实有一个方法专门针对此种情况查询的。

如果你不明白这个格式是什么格式的话，你可以查看ecstore的规则的会员级别字段，或者是shopex系统的优惠劵字段或者是分类的path字段等等都是这个格式的。

我们需要用到的方法是：find_in_set，他的语法如下：

FIND_IN_SET(str,strlist)  <a href="http://dev.mysql.com/doc/refman/5.0/en/string-functions.html#function_find-in-set">详细</a>

参数str就是我们需要查找的内容，strlist就是我们需要查询得目标字符或者字段。

SELECT FIND_IN_SET('b','a,b,c,d');
-&gt; 2

6、