---
ID: 1741
post_title: >
  SQL-INSERT
  INTO插入语法的使用与多条记录的插入
author: ChinaBUG
post_excerpt: |
  INSERT INTO 语句
  INSERT INTO 语句用于向表格中插入新的行。
  语法
  INSERT INTO 表名称 VALUES (值1, 值2,....)
  我们也可以指定所要插入数据的列：
  INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)
  ~~~~~~~~~~~~下面是偶写的~~~~~~~~~~~~~~~~
  我们经常会使用这个语句来插入数据，正常情况下，是没什么特别可说的，不过有一种情况需要注意的，那就是从另外一个表里面查询数据，并把得到的记录集保存到另外一个表里面。下面的带啦就是这样的功能，特别要说的是那个语法噢，不能出现value关键字。
  下面的代码应用环境是：
  shopex不知道为什么会出现goods有这个商品，但是product表里面却没有这个商品的记录，造成前台能够下订单，后台不能手动添加商品到订单中或者新建订单，一旦添加就提醒错误噢
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/sql-insert-into.html
published: true
post_date: 2011-08-22 13:38:28
---
<h2>INSERT INTO 语句</h2>
INSERT INTO 语句用于向表格中插入新的行。
<h3>语法</h3>

[sql]INSERT INTO 表名称 VALUES (值1, 值2,....)[/sql]

我们也可以指定所要插入数据的列：

[sql]INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)[/sql]

上面两个语法的区别在于，第一个是按照默认的字段顺序赋值，下面是可以根据自己需要的字段顺序赋值。

那么如果要插入多条记录怎么操作呢？

其实很简单就是在上面的代码下面新增加一行，用逗号隔开即可，如下

[sql]INSERT INTO table_name (列1, 列2,...) VALUES (值11, 值12,....),(值21, 值22,....)[/sql]

~~~~~~~~~~~~下面是偶写的~~~~~~~~~~~~~~~~

我们经常会使用这个语句来插入数据，正常情况下，是没什么特别可说的，不过有一种情况需要注意的，那就是从另外一个表里面查询数据，并把得到的记录集保存到另外一个表里面。下面的代码就是这样的功能，特别要说的是那个语法噢，不能出现value关键字。

下面的代码应用环境是：

shopex不知道为什么会出现goods有这个商品，但是product表里面却没有这个商品的记录，造成前台能够下订单，后台不能手动添加商品到订单中或者新建订单，一旦添加就提醒错误噢

插入goods内的信息到product表内：


[sql]INSERT INTO cdb_products (goods_id, bn, price, price_ss, cost, mktprice, name, weight, unit, store, store_ss, store_place, pdt_desc, uptime, last_modify, disabled, marketable)(
SELECT g.goods_id, g.bn, g.price, g.price_ss, g.cost, g.mktprice, g.name, g.weight, g.unit, g.store, g.store_ss, g.store_place, g.pdt_desc, g.uptime, g.last_modify, g.disabled, g.marketable FROM cdb_goods AS g
WHERE g.goods_id NOT
IN (
SELECT DISTINCT p.goods_id
FROM cdb_products AS p
)
)[/sql]