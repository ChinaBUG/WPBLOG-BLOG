---
ID: 1440
post_title: >
  密码保护：ShopEX-二次开发之商品信息-相关商品位置改变及排序
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-product-information-related-to-the-location-change-and-the-sort-of-goods.html
published: true
post_date: 2011-07-03 17:00:02
---
二次开发之商品信息-相关商品的设置

　　我们这次的目标是把商品详细页内的相关商品给转移到我们需要的地方，并且控制相关商品的排序问题。下面先来转移相关商品的位置吧：

先找到 core/shop/view/product/index.html 文件，查找

&lt;{if $goods.intro}&gt;
&lt;div tab="商品详情"&gt;

然后将将整段代码复制到

&lt;div&gt;
    &lt;{if $goodspropposition&lt;&gt;1}&gt;

的上方，并且将复制的代码的相关位置删除或者弄成注释，修改地方如下：

&lt;!--div tab="相关商品"--&gt;
&lt;!--h2&gt;&lt;{t}&gt;相关商品&lt;{/t}&gt;&lt;/h2--&gt;

还有最后的

&lt;/table&gt;
  &lt;/div&gt;
&lt;/div&gt;
&lt;!--/div--&gt;
&lt;{/if}&gt;

明白了吗？不明白，那就照做吧。这样做的结果是，相关商品跑到价格下方，详细参数的上方了。

怎么控制商品显示的排序呢？

请找到 core/shop/controller/ctl.product.php 文件，找到下面的代码

$objProduct = &amp;$this-&gt;system-&gt;loadModel('goods/products');

在下面加入我们自己的代码：

//增加排序功能
$orderby = ' brand DESC,brand_id DESC,p_order DESC';

然后保存噢，现在查看之后就会发现商品相关的排序是按照品牌来排序的~

衍生学习：<a title="ShopEX-二次开发之品牌专区品牌页内的商品如何排序" href="http://blog.ipodmp.com/archives/shopex-brand-merchandise-page-to-order/">ShopEX-二次开发之品牌专区品牌页内的商品如何排序</a>