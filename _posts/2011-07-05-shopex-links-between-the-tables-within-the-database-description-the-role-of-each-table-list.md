---
ID: 1480
post_title: >
  密码保护：ShopEX-数据库说明之表之间的联系说明(各个表的作用说明一览)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-links-between-the-tables-within-the-database-description-the-role-of-each-table-list.html
published: true
post_date: 2011-07-05 22:25:18
---
<h1>ShopEX表之间的联系说明</h1>
<h3>1、管理员相关</h3>
sdb_operators         后台管理员信息

sdb_admin_roles           后台管理员身份

sdb_lnk_roles         关联信息与身份[管理员列表]

sdb_lnk_acts          关联身份与能操作的功能页[角色编辑里面-权限选择]

&nbsp;
<h3>2、订单操作</h3>
sdb_sell_logs         订单处理：发货之后才会出现在这个表里面

&nbsp;
<h3>3、商品相关</h3>
sdb_goods             商品信息

sdb_products          产品信息-&gt;子商品表，与goods表结合
<h3>4、优惠卷</h3>
sdb_coupons          优惠券的种类及作用

sdb_coupons_u_items    优惠券使用情况记录表

sdb_member_coupon     会员拥有的优惠券
<h3>5、信息系统（短信、邮箱、站内信息）</h3>
sdb_systmpl           信息的模板库（编辑模板调用）

sdb_sendbox          信息发送记录（发件箱调用）
<h3>6、会员信息相关</h3>
sdb_members                       会员信息

sdb_member_addrs               会员地址信息

sdb_member_attr                  会员个人信息里面扩展的项目（比如加一个QQ选项）

sdb_member_coupon            会员拥有的优惠券

sdb_member_lv                    会员级别

sdb_member_mattrvalue              sdb_member_attr项目的具体信息，如QQ号具体是多少