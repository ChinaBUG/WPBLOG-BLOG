---
ID: 2117
post_title: >
  ShopEX-缺货通知为什么就不能自动填写会员的邮箱呢？
author: ChinaBUG
post_excerpt: |
  ShopEX如果商品缺货的话，就允许客户提交缺货通告，就是输入邮箱，如果到货了，就会通知这个邮箱的主人。
  但是会员登录之后，那个邮箱地址框还是没有出现会员的邮箱，就是说没有自动将会员内填写的邮箱地址。这个从易用性上来说，有点奇怪。
  所以我们的目标是增加这个功能。
  但是一打开这个方法所在的代码一看，额，这个功能模块有存在呢~~可是为什么不出现呢？
  经过偶的研究有如下问题：
  1、变量名写错了。
  2.即使你修改了变量名，还是不行
  2.1在gnotify方法的第一行写上
  2.2在$this->pagedata['goods'] = $aProduct[0];下面写如下代码
  3.你修改完上面的之后你会发现输出有值，但是视图显示还是没有值出现。这个问题很神奇，神奇的不行，是ShopEX固有的，特定的，莫名其妙问题之一。也是写这篇文章的重点问题。
  ......
  偶的测试环境是ShopEX 4.8.5.55324
layout: post
permalink: >
  http://blog.ipodmp.com/2012/07/shopex-2sec-dev-out-of-stock-notice-why-can-not-automatically-fill-the-mailboxes-of-the-members-of-it.html
published: true
post_date: 2012-07-05 17:30:48
---
ShopEX如果商品缺货的话，就允许客户提交缺货通告，就是输入邮箱，如果到货了，就会通知这个邮箱的主人。

但是会员登录之后，那个邮箱地址框还是没有出现会员的邮箱，就是说没有自动将会员内填写的邮箱地址。这个从易用性上来说，有点奇怪。

所以我们的目标是增加这个功能。

但是一打开这个方法所在的代码一看，额，这个功能模块有存在呢~~可是为什么不出现呢？

经过偶的研究有如下问题：

1、变量名写错了。

$this-&gt;member[member_id]

应该是

$this-&gt;member['member_id']

2.即使你修改了变量名，还是不行

你输出$this-&gt;member['member_id']看看，这个变量是没有值的，为什么会这样子呢？嗯，肯定是没有设置呗，这边有两种办法，一种用系统本身的方法，一种是自己动手写个。

2.1在gnotify方法的第一行写上

$this-&gt;_verifyMember(false);

然后就有值了

2.2在$this-&gt;pagedata['goods'] = $aProduct[0];下面写如下代码

list($ck_id) = explode('-',$_COOKIE['MEMBER']);

if(!$this-&gt;member['member_id']){$this-&gt;member['member_id'] = $ck_id;}

然后也有值了。

3.你修改完上面的之后你会发现输出有值，但是视图显示还是没有值出现。这个问题很神奇，神奇的不行，是ShopEX固有的，特定的，莫名其妙问题之一。也是写这篇文章的重点问题。

其实要解决这个问题，你只要把$this-&gt;pagedata['member'] = $aMemInfo;改为

$this-&gt;pagedata['member2'] = $aMemInfo;

或者是把member修改为其他的变量名，记得视图文件内也要修改噢，然后很神奇的，嗯，值输出了！！！

但是你改回去，只要是member的话，肯定是不行的，没值。

偶的测试环境是<span style="text-decoration: underline;">ShopEX 4.8.5.55324</span>