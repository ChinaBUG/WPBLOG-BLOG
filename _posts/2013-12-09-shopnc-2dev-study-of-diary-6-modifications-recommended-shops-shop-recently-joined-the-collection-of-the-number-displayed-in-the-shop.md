---
ID: 3013
post_title: >
  ShopNC二次开发研究日记6:修改推荐店铺收藏店铺最近加盟店铺的显示数量
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  要修改推荐店铺、收藏店铺、最近加盟店铺的显示数量这个其实是最好修改的，唯一麻烦的就是需要自己调整样式，因为样式是写死的，不调整难看。
  进入control\index.php文件然后找到下面的代码：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopnc-2dev-study-of-diary-6-modifications-recommended-shops-shop-recently-joined-the-collection-of-the-number-displayed-in-the-shop.html
published: true
post_date: 2013-12-09 17:07:40
---
要修改推荐店铺、收藏店铺、最近加盟店铺的显示数量这个其实是最好修改的，唯一麻烦的就是需要自己调整样式，因为样式是写死的，不调整难看。

进入control\index.php文件然后找到下面的代码：

//推荐店铺
$model_store = Model('store');
$r_store = $model_store-&gt;getRecommendStore(<span style="color: #ff0000;">3</span>);
Tpl::output('show_recommend_store',$r_store);

//收藏店铺
$f_store = $model_store-&gt;getFavoritesStore(<span style="color: #ff0000;">3</span>);
Tpl::output('show_favorites_store',$f_store);

//最近加盟店铺
$n_store = $model_store-&gt;getNewStore(<span style="color: #ff0000;">3</span>);
Tpl::output('show_new_store',$n_store);

看到红色的数字没？那个就是显示的数量，大胆的根据需要增加吧~前提是后台真的有那么多数据噢，要不可别看了觉得代码不起作用！

修改完，保存之后还需要删除缓存！

进入cache文件夹将除了session文件夹之外的文件及文件夹全部删除！

然后执行一下你会发现修改已经起作用了。