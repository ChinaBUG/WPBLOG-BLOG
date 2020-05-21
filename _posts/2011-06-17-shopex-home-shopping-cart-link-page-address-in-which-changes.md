---
ID: 1356
post_title: >
  ShopEX-首页购物车链接地址在哪个页面修改
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-home-shopping-cart-link-page-address-in-which-changes.html
published: true
post_date: 2011-06-17 00:33:19
---
昨天，在网上看到有人在问 首页购物车链接地址在哪个页面修改 这样的问题，却发觉没人回答，想回答吧，又要注册，算了，还是这边自己写一下，挣人气吧

找到 plugins/widgets/cart/ 目录，主页的购物车是一个插件，所以，到这个地方去找，就3个文件，直接修改default.html这个文件里面的内容吧。

凡是购物车里面的cookies的值都是可以在这边使用，比如计算购物车内的总价，统计数量，各类商品的子数量等等，均可以在里面设置。