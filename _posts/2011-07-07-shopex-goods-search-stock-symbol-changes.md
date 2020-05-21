---
ID: 1509
post_title: >
  密码保护：ShopEX-二次开发-商品搜索-缺货标志修改
author: ChinaBUG
post_excerpt: |
  我们在商品搜索的时候，如果商品缺货则会出现一个按钮让你 填写 缺货登记 ，当然你也可以点击图片进入商品详细页。
  有的人就会没看到的那个缺货登记的按钮，直接点击进入商品详细页。
  从客户感情的角度来说，有点被骗的感觉，而且费时哈~~~理由很多，所以我们现在来把它给取消掉，换一张明显的图片，提醒客户这个缺货！
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-goods-search-stock-symbol-changes.html
published: true
post_date: 2011-07-07 20:39:39
---
<ul>
	<li>文章内容：ShopEX-商品搜索-缺货标志修改</li>
	<li>文章难度：容易</li>
	<li>修改难度：简单</li>
	<li>二次开发：可以</li>
</ul>
我们在商品搜索的时候，如果商品缺货则会出现一个按钮让你 填写 缺货登记 ，当然你也可以点击图片进入商品详细页。

有的人就会没看到的那个缺货登记的按钮，直接点击进入商品详细页。

从客户感情的角度来说，有点被骗的感觉，而且费时哈~~~理由很多，所以我们现在来把它给取消掉，换一张明显的图片，提醒客户这个缺货！

~~

请找到 core/shop/view/product/menu.html文件，在大约22行的地方找到：

&lt;a type="g" href="&lt;{link ctl=product act=gnotify arg0=$product.goods_id arg1=$product_id}&gt;" rel="nofollow" title="缺货登记"&gt;&lt;{t}&gt;缺货&lt;{/t}&gt;&lt;/a&gt;

删掉，改为：

&lt;img src="statics/c_qh.jpg" /&gt;
然后在大约 45行的地方找到

&lt;a target="_blank" href="&lt;{link ctl="product" act="gnotify" arg0=$product.goods_id arg1=$product.product_id}&gt;" rel="nofollow" title="缺货登记"&gt;&lt;{t}&gt;缺货登记&lt;{/t}&gt;&lt;/a&gt;

改为

&lt;img src="statics/c_qh.jpg" /&gt;

测试一下，你会发现那个图标跑边上去了，现在吧上面的&lt;li的class属性换成class="addcart"，再看看就会发现，变成左边，显示正常了噢。

什么，你看不到图标？那是你没有上传图片文件，请检查图片是不是上传到根目录下的statics目录里面。

PS：如果看不到效果，请清空缓存。