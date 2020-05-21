---
ID: 1900
post_title: >
  ShopEX二次开发之订单列表内的打印清单样式增加新的项目如折扣
author: ChinaBUG
post_excerpt: |
  ShopEX商城的管理后台的订单列表内有一项打印的功能，主要是打印购物清单与配货清单。
  那么里面的常规选项就那么多，需要扩展其他的怎么办？比如要增加一栏折扣？
  这边说一下常规项的功能修改吧。
  其实只要你查看一下打印样式里面的模板文件，有经验的你一定会了解，这个模板就是一套规则只要照里面的去做，调用相应的变量就可以实现栏目的显示与隐藏了。
  在打印样式里面有两个开头的变量前缀，就这样称呼吧，，具体怎么称呼还真不清楚，你会看到$orderInfo、$aGoods这样的打头的变量，如<{t}>收货人：<{/t}><{$orderInfo.ship_name}>、单价<{$aGoods.price|cur}>等。这个前缀不能变换，要不会出错滴。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/01/shopex-secondary-development-of-the-list-of-orders-list-of-the-print-style-of-adding-new-items-such-as-discount.html
published: true
post_date: 2012-01-16 09:14:04
---
ShopEX商城的管理后台的订单列表内有一项打印的功能，主要是打印购物清单与配货清单。

那么里面的常规选项就那么多，需要扩展其他的怎么办？比如要增加一栏折扣？

这边说一下常规项的功能修改吧。

其实只要你查看一下打印样式里面的模板文件，有经验的你一定会了解，这个模板就是一套规则只要照里面的去做，调用相应的变量就可以实现栏目的显示与隐藏了。

在打印样式里面有两个开头的变量前缀，就这样称呼吧，，具体怎么称呼还真不清楚，你会看到$orderInfo、$aGoods这样的打头的变量，如&lt;{t}&gt;收货人：&lt;{/t}&gt;&lt;{$orderInfo.ship_name}&gt;、单价&lt;{$aGoods.price|cur}&gt;等。这个前缀不能变换，要不会出错滴。

具体对应的变量如下：

$orderInfo.变量：

order_id,member_id,confirm,status,pay_status,ship_status,user_status,is_delivery,shipping_id,shipping,shipping_area,payment,weight,tostr,itemnum,acttime,createtime,refer_id,refer_url,refer_time,c_refer_id,c_refer_url,c_refer_time,ip,ship_name,ship_area,ship_addr,ship_zip,ship_tel,ship_email,ship_time,ship_mobile,cost_item,is_tax,cost_tax,tax_company,cost_freight,is_protect,cost_protect,cost_payment,currency,cur_rate,score_u,score_g,score_e,advance,discount,use_pmt,total_amount,final_amount,pmt_amount,payed,markstar,memo,print_status,mark_text,disabled,last_change_time,use_registerinfo,mark_type,extend,is_has_remote_pdts,order_refer,print_id,v_code,order_counp

$aGoods.变量：

item_id,order_id,product_id,dly_status,type_id,bn,name,cost,price,amount,score,nums,minfo,sendnum,addon,is_type,point,supplier_id