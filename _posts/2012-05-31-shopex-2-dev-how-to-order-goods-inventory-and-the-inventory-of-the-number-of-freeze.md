---
ID: 2078
post_title: >
  ShopEX-二次开发下订单时怎么知道商品的库存与冻结库存的数量呢
author: ChinaBUG
post_excerpt: |
  ShopEX作为一个成熟的商城，整体还是不错的，虽然技术支持等方面差了点，可不影响他的使用。
  当然，如果在易用性方面再下点功夫我觉得还是可以的，对吧，反正也不多花你们多少时间？
  话说，我们在后台添加新订单时，经常会出现添加的商品缺少库存而不能成功下单的问题，为什么不显示一个库存选项呢？
  你想到了，但是ShopEX可能觉得这个功能不重要吧，没关系我们自己设计吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/shopex-2-dev-how-to-order-goods-inventory-and-the-inventory-of-the-number-of-freeze.html
published: true
post_date: 2012-05-31 13:42:46
---
ShopEX作为一个成熟的商城，整体还是不错的，虽然技术支持等方面差了点，可不影响他的使用。

当然，如果在易用性方面再下点功夫我觉得还是可以的，对吧，反正也不多花你们多少时间？

话说，我们在后台添加新订单时，经常会出现添加的商品缺少库存而不能成功下单的问题，为什么不显示一个库存选项呢？

你想到了，但是ShopEX可能觉得这个功能不重要吧，没关系我们自己设计吧。

主要需要修改如下文件：

core/admin/view/order/order_new.html;

core/admin/view/order/new_items.html;

cct.order.php

我们需要在视图上增加一个调用入口，要不ShopEX是不懂得怎么去运行我们的程序的。然后我们再二次开发一个方法，用作查询库存及冻结库存的数量，输出即可。

下面看看偶设计的效果吧。

<a href="http://blog.ipodmp.com/wp-content/uploads/2012/05/后台-新建订单-检测库存.jpg"><img class="alignnone size-full wp-image-2085" title="后台-新建订单-检测库存" src="http://blog.ipodmp.com/wp-content/uploads/2012/05/后台-新建订单-检测库存.jpg" alt="" width="585" height="1896" /></a>