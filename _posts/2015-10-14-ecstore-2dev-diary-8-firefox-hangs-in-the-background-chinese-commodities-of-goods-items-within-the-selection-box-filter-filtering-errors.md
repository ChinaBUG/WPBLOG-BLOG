---
ID: 4253
post_title: >
  ECStore二次开发日记之8.火狐浏览器后台挂件商品选择框内的商品筛选中文商品筛选错误
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/10/ecstore-2dev-diary-8-firefox-hangs-in-the-background-chinese-commodities-of-goods-items-within-the-selection-box-filter-filtering-errors.html
published: true
post_date: 2015-10-14 11:29:26
---
最近一段时间运营部的同仁在使用火狐编辑模板挂件的时候既然出现商品中文名搜索不到商品的情况。

具体情况如下：在编辑挂件拥有商品选择器的时候，输入中文查询不到数据，但是输入英文、数字是可以查询到数据的。具体请看下面的截图。

<a href="http://blog.ipodmp.com/wp-content/uploads/2015/10/esc201510132011.png"><img class="alignnone size-full wp-image-4254" src="http://blog.ipodmp.com/wp-content/uploads/2015/10/esc201510132011.png" alt="esc201510132011" width="767" height="640" /></a>

很奇怪，一开始没有注意，让她重新安装浏览器，然后问题照旧。所以这个时候需要跟踪调试了。

通过跟踪发现提交查询的链接的参数编码不对，假设“天玑”这个词语吧，在她的浏览器里面：

http://www.shanmai.cn/shopadmin/index.php?app=b2c&amp;ctl=admin_goods&amp;act=finder_goods_items&amp;isgroupbuy=false&amp;istimedbuy=false&amp;name=%CC%EC%E7%E1

编码是：%CC%EC%E7%E1

而我的电脑正常查询，编码是：

http://www.shanmai.cn/shopadmin/index.php?app=b2c&amp;ctl=admin_goods&amp;act=finder_goods_items&amp;isgroupbuy=false&amp;istimedbuy=false&amp;name=%E5%A4%A9%E7%8E%91

&nbsp;

编码是：%E5%A4%A9%E7%8E%91

问题找到就是编码问题呗，有时间的朋友可以分别对上面两个编码转码一下就会知道，第一个编码需要在gb2312的编码下才能正常转换，而第一个是utf8的。

找到问题所在我们第一个想法就是统一编码呗。

打开：/b2c/view/admin/goods/goods_select.html

经过查看，我们定位到

var ipt = $E('.search-ipt',body);
return this.sync(el.get('href')+'&amp;'+ ipt.name +'='+ipt.value);

这个位置，居然没有做转码操作，那么尝试转码一下呗。

var ipt = $E('.search-ipt',body);
// 2015.10.13 ChinaBUG 编码
return this.sync(el.get('href')+'&amp;'+encodeURIComponent( ipt.name )+'='+ipt.value);

然后，问题解决了~好神奇噢。简单吧。