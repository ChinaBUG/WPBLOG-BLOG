---
ID: 2908
post_title: >
  ShopNC二次开发研究日记4:怎么修改各楼层里面的商品推荐板块显示的商品个数
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  首页的各楼层里面的商品推荐，系统设置只能是6个商品，感觉少？来改一下吧~~
  我们知道后台添加这个推荐板块的商品时，如果超过6个的话则不再添加，所以很自然的就想起这个应该是JS在控制的，所以查找一下就会发现在resource\web_config\web_index.js文件内明明白白的写着
  var recommend_max = 3;//推荐数
  var goods_max = 6;//商品数
  var goods_list_max = 7;//商品排行数
  var brand_max = 8;//品牌限制
  var recommend_show = 1;//当前选择的商品推荐
  哎，都不知道怎么说了，好简单对不~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopnc-2dev-study-of-diary-4-how-to-modify-the-number-of-displayed-merchandise-sector.html
published: true
post_date: 2013-11-26 17:55:03
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

首页的各楼层里面的商品推荐，系统设置只能是6个商品，感觉少？来改一下吧~~

我们知道后台添加这个推荐板块的商品时，如果超过6个的话则不再添加，所以很自然的就想起这个应该是JS在控制的，所以查找一下就会发现在resource\web_config\web_index.js文件内明明白白的写着

var recommend_max = 3;//推荐数
var goods_max = 6;//商品数
var goods_list_max = 7;//商品排行数
var brand_max = 8;//品牌限制
var recommend_show = 1;//当前选择的商品推荐

哎，都不知道怎么说了，好简单对不~

然后你修改一下，将

var goods_max = 12;//商品数

你喜欢多少就多少个，然后保存，然后你急匆匆的跑去添加然后会发现，限制还是6个，增加不了太多！

这个时候其实很简单，因为缓存还是6个的，你只要清空缓存就可以了~

但是，ShopNC的缓存真的很糟糕！

所以聪明的小朋友经过试验发现：将已经存在的商品删除掉，然后重新开始添加，就会发现，哈~~可以添加商品了！

等你添加完毕之后你到前台一看，额~~怎么还是6个商品呢？！

有火狐的小朋友一查，呵呵，商品真的出来了，可是被遮住了噢~把外框相关的样式的高度增加一下，就显示出来了噢。

祝好运~那个缓存要特别注意~ShopNC的难点就是缓存呀~~