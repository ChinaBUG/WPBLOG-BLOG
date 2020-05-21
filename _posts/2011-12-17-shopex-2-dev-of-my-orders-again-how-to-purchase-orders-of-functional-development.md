---
ID: 1861
post_title: >
  ShopEX二次开发我的订单如何再次购买再次下单功能开发
author: ChinaBUG
post_excerpt: |
  如果您每天上ShopEX商城购买的都是大同小异的商品，或者完全就是重复的商品，每次都要重新挑选累噢，如果商品里面还有特殊商品，如秒杀商品的话，添加都没有添加的地方呢~
  为了方便客户，人性化操作，特设计了这个功能。
  只要登录会员中心主页或者我的订单列表，您将看到出现一个新的选项“再次购买?”链接，点击之后就能将订单内的商品添加到购物车内，包含商品的数量噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/12/shopex-2-dev-of-my-orders-again-how-to-purchase-orders-of-functional-development.html
published: true
post_date: 2011-12-17 20:30:32
---
如果您每天上ShopEX商城购买的都是大同小异的商品，或者完全就是重复的商品，每次都要重新挑选累噢，如果商品里面还有特殊商品，如秒杀商品的话，添加都没有添加的地方呢~

为了方便客户，人性化操作，特设计了这个功能。

只要登录会员中心主页或者我的订单列表，您将看到出现一个新的选项“再次购买?”链接，点击之后就能将订单内的商品添加到购物车内，包含商品的数量噢。

&nbsp;

二次开发我的订单如何再次购买再次下单功能开发的心得：

要实现这个功能，有两个要点，一个是怎么添加到购物车，一个是如何获得订单的商品。

通过分析数据库我们很明显能获得订单内的商品的数据的结构，只要指定订单号自然就能获得商品列表了。那么怎么添加到购物车呢？一开始肯定要想用系统带有添加方法，对吧，但是发现那个不现实，因为不好加，反正我是没什么欲望使用加入购物车的方法来实现。

除了直接添加的操作外，还能怎样做呢？

想一下购物车的业务流程，发觉，每次系统登录之后上次的购物车内容不管到哪里登陆都会保留着，没有变化，每次都会自动将未登录之前的购物车内商品与登陆后的购物车内商品合并。那么是不是只要找到旧数据保存的地方更新之后是不是系统就会自动合并数据呢？

解决方法很明显了。。就是找到购物车数据保存的地方了。

通过分析代码，发现这个地方好容易找，查看一下他的规则，汗，好简单，简单的写了一下，搞定。

&nbsp;

<img src="http://img01.taobaocdn.com/imgextra/i1/108042908/T2gKWfXbpXXXXXXXXX_%21%21108042908.png" alt="" align="middle" />
<img src="http://img04.taobaocdn.com/imgextra/i4/108042908/T2reWfXbXXXXXXXXXX_%21%21108042908.png" alt="" align="middle" />
<img src="http://img04.taobaocdn.com/imgextra/i4/108042908/T2C1WfXaVXXXXXXXXX_%21%21108042908.png" alt="" align="middle" />
<img src="http://img04.taobaocdn.com/imgextra/i4/108042908/T2UeWfXatXXXXXXXXX_%21%21108042908.png" alt="" align="middle" />