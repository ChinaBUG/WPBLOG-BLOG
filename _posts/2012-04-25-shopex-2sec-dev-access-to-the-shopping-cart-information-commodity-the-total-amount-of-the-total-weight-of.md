---
ID: 2040
post_title: >
  ShopEX-二次开发获取购物车内的信息,商品数量商品重量,商品重量总金额等
author: ChinaBUG
post_excerpt: |
  今天在看ShopEX官方论坛看到有人提出这个问题 [485/易开店]请各位大虾赐教~~想在前台显示已购买商品的重量。求插件 好吧，正好最近没写文章了，联系一下，顺便分享一下开发的经验吧。
  在ShopEX二次开发之中要获取购物车内相关的信息的办法有两个，目前我知道的，但是我比较常用下面的这个办法，简单，只要调用一下即可。
  代码如下：
  ...
  好了，在变量 $trading中就有你需要的全部的信息，我们可以直接输出这个变量
  ...
  然后你就会看到里面的都是你需要的，比如购物车内的商品总重量、总金额、总的商品信息（个数、分别的编码等信息）、总获得的积分数，还有其他的杂项，如使用的优惠劵等信息。
  你需要什么就输出什么吧，在哪里输出？汗~~~学学基础还是不错的，对吧~
layout: post
permalink: >
  http://blog.ipodmp.com/2012/04/shopex-2sec-dev-access-to-the-shopping-cart-information-commodity-the-total-amount-of-the-total-weight-of.html
published: true
post_date: 2012-04-25 10:15:04
---
今天在看ShopEX官方论坛看到有人提出这个问题 <a href="http://bbs.shopex.cn/thread.php?fid-127-type-60.html">[485/易开店]请各位大虾赐教~~想在前台显示已购买商品的重量。求插件</a> 好吧，正好最近没写文章了，联系一下，顺便分享一下开发的经验吧。

在ShopEX二次开发之中要获取购物车内相关的信息的办法有两个，目前我知道的，但是我比较常用下面的这个办法，简单，只要调用一下即可。

代码如下：

$sale = &amp;$this-&gt;system-&gt;loadModel('trading/sale');
$trading = $sale-&gt;getCartObject($this-&gt;cart,$GLOBALS['runtime']['member_lv'],true);

好了，在变量 $trading中就有你需要的全部的信息，我们可以直接输出这个变量

print_r($trading);

然后你就会看到里面的都是你需要的，比如购物车内的商品总重量、总金额、总的商品信息（个数、分别的编码等信息）、总获得的积分数，还有其他的杂项，如使用的优惠劵等信息。

你需要什么就输出什么吧，在哪里输出？汗~~~学学基础还是不错的，对吧~