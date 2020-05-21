---
ID: 3402
post_title: >
  ShopEX二次开发DIY日记16之挂件goods_show里怎么二次开发增加商品销售总量或者其他
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/shopex-2dev-diy-journal-16-widgets-goods-show-how-to-increase-the-total-merchandise-sales-or-other.html
published: true
post_date: 2014-04-10 22:28:27
---
今天抽空去ShopEX的老东家商派论坛去看了看，发现，已经是变成广告的天地，没什么技术交流的氛围了，不知道是店大了，不在乎我们这些小店还是怎样噢，感觉一日不如一日呀~

话题扯远了，在论坛看到有人问怎么为挂件goods_show二次开发增加商品销售总量，这个是很好的话题哈，正好拿来DIY讲解一下我们应该怎么对挂件做二次开发噢。

在开讲之前我们先来看看sdb_goods表内的字段列表吧~
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<p align="center"><b>字段名称</b><b></b></p>
</td>
<td>
<p align="center"><b>数据类型</b><b></b></p>
</td>
<td>
<p align="center"><b>主键</b><b></b></p>
</td>
<td>
<p align="center"><b>必填</b><b></b></p>
</td>
<td>
<p align="center"><b>含义</b><b></b></p>
</td>
<td>
<p align="center"><b>说明</b><b></b></p>
</td>
</tr>
<tr>
<td>
<p align="center">goods_id</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">ID</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">cat_id</p>
</td>
<td>
<p align="center">object:goods/productCat</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">分类</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">type_id</p>
</td>
<td>
<p align="center">object:goods/gtype</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">类型</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">goods_type</p>
</td>
<td>
<p align="center">Array</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">销售类型</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">brand_id</p>
</td>
<td>
<p align="center">object:goods/brand</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">品牌</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">brand</p>
</td>
<td>
<p align="center">varchar(100)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">品牌</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">supplier_id</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">supplier_goods_id</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">wss_params</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">image_default</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">默认图片</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">udfimg</p>
</td>
<td>
<p align="center">bool</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">是否用户自定义图</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">thumbnail_pic</p>
</td>
<td>
<p align="center">varchar(255)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">缩略图</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">small_pic</p>
</td>
<td>
<p align="center">varchar(255)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">big_pic</p>
</td>
<td>
<p align="center">varchar(255)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">image_file</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">图片文件</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">brief</p>
</td>
<td>
<p align="center">varchar(255)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">商品简介</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">intro</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">详细介绍</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">mktprice</p>
</td>
<td>
<p align="center">money</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">市场价</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">cost</p>
</td>
<td>
<p align="center">money</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">成本价</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">price</p>
</td>
<td>
<p align="center">money</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">销售价</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">bn</p>
</td>
<td>
<p align="center">varchar(200)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">商品编号</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">name</p>
</td>
<td>
<p align="center">varchar(200)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">商品名称</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">marketable</p>
</td>
<td>
<p align="center">bool</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">上架</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">weight</p>
</td>
<td>
<p align="center">decimal(20,3)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">重量</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">unit</p>
</td>
<td>
<p align="center">varchar(20)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">单位</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">store</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">库存</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">store_place</p>
</td>
<td>
<p align="center">varchar(255)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">库位</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">score_setting</p>
</td>
<td>
<p align="center">Array</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">score</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">积分</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">spec</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">规格</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">pdt_desc</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">物品</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">spec_desc</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">物品</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">params</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">uptime</p>
</td>
<td>
<p align="center">time</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">上架时间</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">downtime</p>
</td>
<td>
<p align="center">time</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">下架时间</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">last_modify</p>
</td>
<td>
<p align="center">time</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">更新时间</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">disabled</p>
</td>
<td>
<p align="center">bool</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">notify_num</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">缺货登记</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">rank</p>
</td>
<td>
<p align="center">decimal(5,3)</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">rank_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">comments_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">view_w_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">周浏览数</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">view_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">浏览数</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">buy_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">购买数</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">buy_w_count</p>
</td>
<td>
<p align="center">int unsigned</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">周购买数</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">count_stat</p>
</td>
<td>
<p align="center">longtext</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">p_order</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">排序</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
<tr>
<td>
<p align="center">d_order</p>
</td>
<td>
<p align="center">number</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">√</p>
</td>
<td>
<p align="center">-</p>
</td>
<td>
<p align="center">-</p>
</td>
</tr>
</tbody>
</table>
大家可以看到上面常用的都已经注释好作用了，那么我们知道SQL只要有这个字段的名字，那么就能获取到值。

下面就是对代码作分析噢，其实很简单，只要有PHP基础的朋友都懂得直接修改的，就是很多朋友在问，说到底就是PHP没有基础！又不愿意自己去学，哎，最鄙视了~我们是真的遇上问题要求救，这些人是从没打算自己学习。

先打开plugins\widgets\goods_show目录，然后你会看到5个文件，分别是_config.html、defalut.html、widget_cfg_goods_show.php、widget_goods_show.php、widgets.php，其实有的挂件还有一个文件名字叫_preview.html，这些文件时必备的文件，功能如下：
<table>
<tbody>
<tr>
<td>文件名称</td>
<td>文件作用</td>
</tr>
<tr>
<td>_config.html</td>
<td>后台配置模板文件</td>
</tr>
<tr>
<td>widget_cfg_goods_show.php</td>
<td>后台配置项的控制器</td>
</tr>
<tr>
<td>defalut.html</td>
<td>前台默认的模板，在widgets.php内定义</td>
</tr>
<tr>
<td>widget_goods_show.php</td>
<td>前台挂件功能的控制器，将后台的参数转换为数据用以前台的输出</td>
</tr>
<tr>
<td>widgets.php</td>
<td>挂件配置文件，有关挂件的信息配置</td>
</tr>
<tr>
<td>_preview.html</td>
<td>挂件在编辑的状态下的预览，用以没有界面的挂件的后台编辑占位</td>
</tr>
</tbody>
</table>
今天不是教怎么开发挂件，所以我们不需要每个文件都去查看噢，只需要打开widget_goods_show.php文件，然后找到：

[php]$result=$o-&gt;getList(null,$filter,0,$limit,$orderby['sql']);[/php]

这行是调用products模型获取商品信息，有开发经验的朋友肯定就知道我们要获取的数据就是这个在控制，只需要在这个控制的方法里面增加销售总量的字段或者其他需要的内容，然后前台调用一下标签即可。

下面我们先来查看一下getList方法的原型哈~

[php]getList($cols,$filter='',$start=0,$limit=20,$orderType=null)[/php]

由上面我们知道getList的第一个参数是字段列表，类型为字符串，我们只需要将需要的字段名合并成一个字符串传入即可，比如我们需要商品名称，价格，那么在底下用getlist方法之前我们只需要定义：

[php]$cols=&quot;name,price&quot;;[/php]

&nbsp;

&nbsp;

然后就可以调用getList方法，然后你就会获取每个商品的名称及价格字段。当然，在挂件中定义为null值，代表这个使用默认的字段，我们打开mdl.products.php文件，你会看到默认字段的定义了：

[php]var $defaultCols = 'bn,name,cat_id,price,store,marketable,brand_id,weight,d_order,uptime,type_id,supplier_id';
var $appendCols = 'goods_id,image_default,thumbnail_pic,brief,pdt_desc,mktprice,big_pic';[/php]

也就是没有显示销售量字段，那么我们怎么调用噢？有两个处理办法：

1、直接指定参数

[php]$result=$o-&gt;getList(null,$filter,0,$limit,$orderby['sql']);[/php]

挂件的设置是指定字段为空，那么我们就按照上面的说明指定一下字段参数即可。如下：

[php]$cols=&quot;name,price&quot;;
$result=$o-&gt;getList($cols,$filter,0,$limit,$orderby['sql']);[/php]

然后就可以了，噢，请将$cols的值设置为*号吧，获取全部的字段好了~你懂得修改不？

2、设定默认变量哈

[php]$o-&gt;defaultCols = 'bn,name,cat_id,price,store,marketable,brand_id,weight,d_order,uptime,type_id,supplier_id';
$o-&gt;appendCols = 'goods_id,image_default,thumbnail_pic,brief,pdt_desc,mktprice,big_pic';
$result=$o-&gt;getList(null,$filter,0,$limit,$orderby['sql']);[/php]

也可以获取正确的值噢~~哈，具体的字段请参考上面的表结构，然后自己修改吧~