---
ID: 3426
post_title: >
  ShopNC二次开发研究日记10:多个ShopNC商城共享商品数据会员资料订单系统的思路分享
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/shopnc-2dev-study-of-diary-9-two-or-more-mall-share-product-data-of-shopnc-member-information-order-system.html
published: true
post_date: 2014-04-23 11:03:17
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

最近感冒了，造成头昏沉沉的，不想开发~看看群里面有人提出：<b style="color: #000000; font-family: ''; font-size: 12px; font-style: normal; font-variant: normal; letter-spacing: normal; line-height: normal; orphans: auto; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: auto; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: #ffedc4;"><span style="font-size: 12pt; font-family: 微软雅黑, 'MS Sans Serif', sans-serif;">谁知道两个shopnc  在搜索里面可以同步搜索到宝贝怎么搞</span></b>

恩，我就知道我又有机会继续啰嗦开课了。

对于这个问题其实很多人想要问，因为很多人有这个，那样的需求需要处理，比如：
<ol>
	<li>怎么共享商品数据？</li>
	<li>怎么同步会员资料？</li>
	<li>怎么共享订单？</li>
	<li>怎么开发多地区商城？</li>
	<li>怎么可以让不同的会员访问不同的商品数据？</li>
	<li>怎么能让会员注册一次却可以在旗下站都能登录呢？</li>
	<li>......</li>
</ol>
好多的想法需要实现，那么怎么做噢？

来吧，这边就分享一下思路吧，具体的代码......没有~~

1、多个ShopNC商城访问的数据，会员资料，订单系统等都要一样！

其实这个需求感觉没啥必要说但是架不住可能有人有这种奇葩的需求呀。

其实这个需求的解决方案很简单，就是指定同一个数据库即可！然后就可以在任意一个ShopNC商城里登录管理商品信息等操作，而且是实时更新。

这个的做法是打造一模一样的孪生兄弟网站，其实，感觉啥屁用，因为<span style="text-decoration: underline;">这样子多个站还不如一个站绑定多个域名来的简单、快捷，而效果是一样的！</span>

2、可以单独共享一部分的数据而不是全部？

这个需求可能需要的比较多，最起码有的人不希望订单系统是同一个吧？！
<blockquote><em>如果你希望商城的什么都一样，就是主题不一样的话，那么你只能是去查看我之前的一篇文章<a title="EcShop/ShopEX/ShopNC二次开发制作设计手机站与WAP站与微商城微网站的一点点思路与想法" href="http://blog.ipodmp.com/archives/ecshop-shopex-shopnc-the-idea-of-other-a-little-bit-of-thinking-and-the-development-of-design-and-wap-mobile-station-station/">EcShop/ShopEX/ShopNC二次开发制作设计手机站与WAP站与微商城微网站的一点点思路与想法</a></em></blockquote>
那么只要按照1说的指定同一个数据库那么肯定会出现同一个订单表的，怎么办？

这个时候才是真正的DIY呀~没有订单表，我们让他有订单表呗~根据实际情况复制一个订单表！不懂的操作的话我就没办法了~然后请修改相应的前缀，这个是非常重要的东西噢。

然后就去找订单模型，反正想要修改什么系统就去找什么模型，按照正常的开发流程进入做二次开发，用我的思路来说，在相关的模型里面需要增加一个参数，来辨别一下到底程序应该调用的是共有的数据库还是单独的数据库噢，当然也是可以在配置文件里面再增加前缀符，然后再配合着使用。

然后还要修改数据库操作类，数据库里面写死了数据库的前缀，所以我们这边需要根据需要修改为根据实际情况来调用正确的前缀，这样子就会获取正确的数据呗。

<span style="text-decoration: underline;">上面用订单系统来举例子感觉有点失败，因为订单系统涉及的表及内容比较多，在讲解的时候需要啰嗦一大篇，还是换为搜索商品数据来讲解吧。</span>

搜索商品数据，我们做如下设定：

两套ShopNC系统没有使用同一个数据库，但是我们希望搜索的商品却是同一个表。简单说，共享商品数据，其他的都是独立的。那么我们需要安装第一套为主数据A，第二套为辅数据B，在B中搜索调用A中的商品数据。这个定义是希望大家明白，到时候要去那里更新商品信息，要不会乱的噢。

A不需要修改，因为他本身就是一个完整的商城了。我们来看看B的搜索，请查找到ShopNC\control\search.php控制器文件打开。

然后我们打开ShopNC商城，点击最上方的搜索，你会看到出现全部的商品了，查看一下地址栏上的链接，你会发现链接地址是：

index.php?act=search&amp;keyword=

act是控制器的名字，那么操作是那个噢？ShopNC的默认操作是index方法，所以上面的链接有可以变成如下：

index.php?act=search&amp;op=index&amp;keyword=

试试看是不是一样的，从这个地方我们获取了控制器的名字是search，操作的方法是index，所以在打开上面的search.php控制器后我们找到indexOp这个方法。

仔细看这个方法都有注释噢，我们找到下面代码：


[php]//商品列表
$fieldstr = &quot; goods.goods_id,goods.goods_name,goods.gc_id,goods.gc_name,goods.store_id,goods.goods_image,goods.goods_store_price,
goods.goods_click,goods.goods_state,goods.goods_commend,goods.commentnum,goods.salenum,goods.goods_goldnum,goods.goods_isztc,
goods.goods_ztcstartdate,goods.goods_ztclastdate,goods.group_flag,goods.group_price,goods.xianshi_flag,goods.xianshi_discount,
goods.city_id,goods.province_id,goods.kd_price,goods.py_price,goods.es_price,
store.store_name,store.grade_id,store.store_domain,store.store_credit,store.praise_rate,store.store_desccredit&quot;;
$goods_list = $model_goods-&gt;getGoods(array(
'goods_id_in'=&gt;$data['goods_id_str'],
'price'=&gt;$price_interval,
'group_flag'=&gt;$group_flag,
'xianshi_flag'=&gt;$xianshi_flag,
'keyword'=&gt;$search_key,
'province_id'=&gt;(is_numeric($_GET['area_id']) &amp;&amp; $_GET['area_id'] &gt; 0) ? $_GET['area_id'] : '',
'goods_show'=&gt;'1',
'goods_form'=&gt;$goods_form,
'order'=&gt;$order,
),$page,$fieldstr,'store',$extend);
Tpl::output('goods_list',$goods_list);[/php]


然后你就知道了，原来商品是根据这些条件用商品模型的getGoods方法获取的，那么你是不是想到要去修改模型了呢？

没错哈~

刚才说了，在这边我们还需要指定一个参数，方便我们在数据库操作类那边做识别，怎么指定这个参数呢？其实就是在这个方法上增加一个参数或者给这个模型增加一个变量即可。如：


[php]$model_goods-&gt;getNO2 = true;[/php]


然后剩下的就是跟踪进去然后修改数据库操作类里面的方法了，这边就不详细说了，改完就达到效果了噢。~