---
ID: 1744
post_title: >
  ShopEX二次开发-商品有库存，后台却不能添加到订单，前台可以下订单一例
author: ChinaBUG
post_excerpt: |
  ShopEX-商品有库存，后台却不能添加到订单，前台可以下订单一例
  实例描述：
  最近负责的ShopEX商城出现一个奇怪的现象，就是客人可以自己下订单，但是，后台客服人员要下订单，却不能添加该商品，新建订单时，输入商品号，查询结果为0。
  经过跟踪，发现这些商品都有一个特征：就是在goods表里面有数据，而在product表里面却没有数据，而正常情况下，非多规格商品在product表内也是有一条记录的，所以，补充吧，但是，怎么知道自己的商城有多少条这样的商品呢？怎么批量处理呢？
  请看本文噢，本文就是来告诉你怎么处理这个问题的。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/shopex-2-dev-goods-in-stock-but-can-not-be-added-back-to-the-order-you-can-order-one-case-of-front.html
published: true
post_date: 2011-08-22 14:00:04
---
ShopEX-商品有库存，后台却不能添加到订单，前台可以下订单一例

实例描述：

最近负责的ShopEX商城出现一个奇怪的现象，就是客人可以自己下订单，但是，后台客服人员要下订单，却不能添加该商品，新建订单时，输入商品号，查询结果为0。

经过跟踪，发现这些商品都有一个特征：就是在goods表里面有数据，而在product表里面却没有数据，而正常情况下，非多规格商品在product表内也是有一条记录的，所以，补充吧，但是，怎么知道自己的商城有多少条这样的商品呢？怎么批量处理呢？

请看本文噢，本文就是来告诉你怎么处理这个问题的。

首先，我们来查有多少个商品在product表里面没有记录的：

SELECT g.* FROM cdb_goods as g where g.goods_id not in (SELECT DISTINCT p.goods_id FROM cdb_products as p)

执行语句后你就会发现你的那些不能添加的商品都显示出来了。

显示数据不是我们的目的，我们的目的是要解决问题，怎么批量把这些没有记录的商品自动添加信息，下面的代码就是插入goods内的信息到product表内：

INSERT INTO cdb_products (goods_id, bn, price, price_ss, cost, mktprice, name, weight, unit, store, store_ss, store_place, pdt_desc, uptime, last_modify, disabled, marketable)(
SELECT g.goods_id, g.bn, g.price, g.price_ss, g.cost, g.mktprice, g.name, g.weight, g.unit, g.store, g.store_ss, g.store_place, g.pdt_desc, g.uptime, g.last_modify, g.disabled, g.marketable FROM cdb_goods AS g
WHERE g.goods_id NOT
IN (
SELECT DISTINCT p.goods_id
FROM cdb_products AS p
)
)

代码有点长，慢慢看你自然就会懂，特别要注意，里面的那个语法跟正常情况下的语法是不一样的，具体说明请看本博客内的<a title="SQL-插入语法的使用(INSERT INTO)" href="http://blog.ipodmp.com/archives/sql-insert-into/" target="_blank">SQL-插入语法的使用(INSERT INTO)</a>