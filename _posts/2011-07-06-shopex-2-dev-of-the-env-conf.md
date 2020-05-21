---
ID: 1500
post_title: ShopEX-二次开发之$env.conf
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-of-the-env-conf.html
published: true
post_date: 2011-07-06 15:16:04
---
在阅读源码时，我们在一些关键的公用地方经常会看到诸如$env.conf.site.thumbnail_pic_height、$env.conf.site.thumbnail_pic_width等以$env.conf开头的变量，那么这些变量是什么含义？代表什么？我们能够修改吗？

经过偶的不懈努力哈，发现这个其实就是后台的设置选项的值，由ShopEX系统自动获取并显示出来滴。

<span style="text-decoration: underline;">$env.conf.point.get_policy </span>   ==》$this-&gt;system-&gt;getConf('point.get_policy')

如上面说的那两个参数的设置的地方在 商品配置-&gt;商品图片设置 然后你会看到很多的设置的地方噢，这两个就是 列表页缩略图 的高度与宽度。

下面是一些的配置的名称：

[site] =&gt; Array
(
[certtext] =&gt;
[tax_ratio] =&gt; 0
[reading_glass_width] =&gt; 430
[reading_glass_height] =&gt; 600
[reading_glass] =&gt; 0
[thumbnail_pic_width] =&gt; 120
[thumbnail_pic_height] =&gt; 160
[big_pic_width] =&gt; 600
[big_pic_height] =&gt; 600
[small_pic_width] =&gt; 300
[small_pic_height] =&gt; 300
[watermark.wm_small_enable] =&gt; 0
[watermark.wm_big_enable] =&gt; 0
[trigger_tax] =&gt; 1
[delivery_time] =&gt; 2
[rsc_rpc] =&gt; false
[register_valide] =&gt; false
[login_valide] =&gt; false
[show_storage] =&gt; false
[show_mark_price] =&gt; true
[market_rate] =&gt; 1.2
[buy.target] =&gt; 3
[decimal_digit] =&gt; 2
[decimal_type] =&gt; 0
[level_switch] =&gt; 0
[login_type] =&gt; href
[market_price] =&gt; 1
[save_price] =&gt; 1
[retail_member_price_display] =&gt; 0
[wholesale_member_price_display] =&gt; 0
[api.maintenance.is_maintenance] =&gt;
[api.maintenance.notify_msg] =&gt;
[list_title] =&gt;
)

你要问我怎么获取的？

其实只需要随便loadModel个模块，然后输出即可。

如

$objGoods = &amp;$this-&gt;system-&gt;loadModel('goods/products');
print_r( $objGoods );
exit;

当然，你也可以直接输出$this对象哈。

print_r( $this );
exit;