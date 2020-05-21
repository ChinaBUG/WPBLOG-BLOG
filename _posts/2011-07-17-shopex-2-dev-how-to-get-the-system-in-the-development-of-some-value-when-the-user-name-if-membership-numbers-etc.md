---
ID: 1661
post_title: >
  ShopEX-二次开发-如何在开发时获取系统的一些值如用户名会员编号经验积分等(cookies)
author: ChinaBUG
post_excerpt: |
  我们在开发时需要用到会员的一些信息，如何获取噢？
  很简单，只需要调用COOKIE对象，取得里面的值即可！
  具体如下：
  我们先浏览一下COOKIE里面有什么东西：
  print_r($_COOKIE);
  exit;
  然后你就会看到（记得先登录噢）：
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-how-to-get-the-system-in-the-development-of-some-value-when-the-user-name-if-membership-numbers-etc.html
published: true
post_date: 2011-07-17 11:59:30
---
我们在开发时需要用到会员的一些信息，如何获取噢？

很简单，只需要调用COOKIE对象，取得里面的值即可！

具体如下：

我们先浏览一下COOKIE里面有什么东西：

print_r($_COOKIE);
exit;

然后你就会看到（记得先登录噢）：

Array
(
[N] =&gt; 079934B4-E604-71E4-A98C-1E146B4617C4
[L_RANDOM_CODE] =&gt; f1ea154c843f7cf3677db7ce922a2d17
[loginName] =&gt; demo
[S_RANDOM_CODE] =&gt; 3bd7ef30b1a12dc749b97afc9517a4f4
[ST_ShopEx-Order-Buy] =&gt; bf7d97a0538ed79ed2817d021e5c9a28
[CART_NUMBER] =&gt; 2
[MEMBER] =&gt; 1-fe01ce2a7fbac8fafaed7c982a04e229-3bd5d9a2041f34a4a41ec56f635f44d6-1310906383
[UNAME] =&gt; demo
[MLV] =&gt; 1
[CART_COUNT] =&gt; 2
)

这边是以demo为例。

其他没什么好说的，只有下面几个段需要说明一下：

[S_RANDOM_CODE]

这个是验证码的MD5值。验证验证码是否正确就是用这个值跟你输入的值比较。

[MEMBER] =&gt; 1-fe01ce2a7fbac8fafaed7c982a04e229-3bd5d9a2041f34a4a41ec56f635f44d6-1310906383

这行的格式是：$memberID."-".utf8_encode( $row['uname'] )."-".md5( $row['password'].STORE_KEY )."-".time( );

[CART_COUNT]

这个是购物车商品数量，前台的那个购物篮挂件就是直接调用这个值显示出来的。

&nbsp;