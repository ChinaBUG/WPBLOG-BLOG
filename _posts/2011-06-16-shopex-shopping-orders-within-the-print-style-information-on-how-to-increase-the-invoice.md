---
ID: 1341
post_title: >
  密码保护：ShopEX-二次开发之购物订单内的打印样式如何增加发票信息
author: ChinaBUG
post_excerpt: |
  　　今天，在订单列表里面的购物清单跟配货清单上需要增加是否需要发票，需要发票的，则需要显示发票税额与发票抬头，看了一下 订单列表\打印样式 里面的模板，做了一下修改即完成。
  　　二次开发到现在，就这个最简单，方便呀。
  　　代码如下：
  　　请在需要的地方输入下面代码即可。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-shopping-orders-within-the-print-style-information-on-how-to-increase-the-invoice.html
published: true
post_date: 2011-06-16 18:47:25
---
　　今天，在订单列表里面的购物清单跟配货清单上需要增加是否需要发票，需要发票的，则需要显示发票税额与发票抬头，看了一下 订单列表\打印样式 里面的模板，做了一下修改即完成。

　　二次开发到现在，就这个最简单，方便呀。

　　代码如下：

　　请在需要的地方输入下面代码即可。

&lt;p&gt;&lt;{t}&gt;是否需要发票：&lt;{/t}&gt;&lt;{if $orderInfo.is_tax}&gt;需要&lt;br/&gt;发票税额：&lt;{$order.cost_tax|cur}&gt;&lt;br/&gt;发票抬头:&lt;{$order.tax_company}&gt;&lt;{else}&gt;不需要&lt;{/if}&gt;&lt;/p&gt;

20110624：如果出现没有信息的情况，那么请检查是否字段名错误，即上面的$order是否跟原文里面的一样噢。