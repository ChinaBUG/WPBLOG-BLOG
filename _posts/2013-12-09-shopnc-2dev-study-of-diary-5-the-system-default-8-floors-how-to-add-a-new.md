---
ID: 3009
post_title: >
  ShopNC二次开发研究日记5:系统默认8个楼层怎么新增加一个并定义新的样式？
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  看着默认的8个楼层，你是不是很纠结噢，看看淘宝，看看京东，看看壹号店，为什么大家都要盖那么高的楼呢~
  来吧，怨叹是解决不了问题的。动手DIY吧~~
  找了一下后台发现没有针对这个功能的操作界面，也就是说新建这个功能需要我们自己开发出来，要不就只能直接操作数据库了~~
  我们今天是直接操作数据库来新建楼层，有基础的朋友可以二次开发一下~
  我们先进入后台，使用phpmyadmin这个工具，当然你也可以使用其他的，只要你懂得操作就行。
  找到shopnc_web表，可能你的前缀跟我的不一样，请将shopnc_改为你的前缀即可，你会发现这个表内已经存在8个记录，而且都好眼熟噢，是不是就是板块楼层哈？执行下面的SQL语句，请自行按照实际修改一番：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopnc-2dev-study-of-diary-5-the-system-default-8-floors-how-to-add-a-new.html
published: true
post_date: 2013-12-09 16:22:43
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

看着默认的8个楼层，你是不是很纠结噢，看看淘宝，看看京东，看看壹号店，为什么大家都要盖那么高的楼呢~

来吧，怨叹是解决不了问题的。动手DIY吧~~

找了一下后台发现没有针对这个功能的操作界面，也就是说新建这个功能需要我们自己开发出来，要不就只能直接操作数据库了~~

我们今天是直接操作数据库来新建楼层，有基础的朋友可以二次开发一下~

我们先进入后台，使用phpmyadmin这个工具，当然你也可以使用其他的，只要你懂得操作就行。

找到shopnc_web表，可能你的前缀跟我的不一样，请将shopnc_改为你的前缀即可，你会发现这个表内已经存在8个记录，而且都好眼熟噢，是不是就是板块楼层哈？执行下面的SQL语句，请自行按照实际修改一番：
<blockquote><em>INSERT INTO `shopnc_web` (`web_id`, `web_name`, `style_name`, `web_page`, `update_time`, `web_sort`, `web_show`, `web_html`) VALUES</em>
<em>(9, '自定义', 'abc', 'index', 1346654976, 9, 0, NULL)</em></blockquote>
然后执行，你会发现shopnc_web增加了一行~然后你直接进入“网站&gt;首页配置&gt;管理”就会看到新增加一个叫“自定义”的楼层板块了噢。

你迫不及待的点击编辑，然后......系统提醒你没有这个板块信息~

不要着急，我们再找到shopnc_web_code表，你会看到这个表好多的记录，感觉好高深噢，其实这些就是那些模块的记录，如果你要增加新的组件的话，恩就是这里面做修改，不过这个不是我们现在的主题，请按照实际修改一下下面的代码并执行：
<pre>INSERT INTO `shopnc_web_code` (`web_id`, `code_type`, `var_name`, `code_info`, `show_name`) VALUES
(9, 'array', 'tit', 'a:2:{s:3:"pic";s:56:"upload/adv/web-1-11_f3acb5954fbdeef69591fd5a84ac3adb.png";s:3:"url";s:0:"";}', '标题图片'),
(9, 'array', 'category_list', 'a:2:{i:200;a:2:{s:9:"gc_parent";a:2:{s:5:"gc_id";s:3:"200";s:7:"gc_name";s:12:"休闲零食";}s:11:"goods_class";a:9:{i:203;a:2:{s:5:"gc_id";s:3:"203";s:7:"gc_name";s:6:"鸭脖";}i:204;a:2:{s:5:"gc_id";s:3:"204";s:7:"gc_name";s:6:"鸭舌";}i:205;a:2:{s:5:"gc_id";s:3:"205";s:7:"gc_name";s:6:"鸡翅";}i:206;a:2:{s:5:"gc_id";s:3:"206";s:7:"gc_name";s:9:"牛肉干";}i:207;a:2:{s:5:"gc_id";s:3:"207";s:7:"gc_name";s:9:"猪肉脯";}i:208;a:2:{s:5:"gc_id";s:3:"208";s:7:"gc_name";s:9:"猪肉松";}i:209;a:2:{s:5:"gc_id";s:3:"209";s:7:"gc_name";s:6:"糖果";}i:210;a:2:{s:5:"gc_id";s:3:"210";s:7:"gc_name";s:9:"鱿鱼丝";}i:211;a:2:{s:5:"gc_id";s:3:"211";s:7:"gc_name";s:12:"饼干糕点";}}}i:202;a:2:{s:9:"gc_parent";a:2:{s:5:"gc_id";s:3:"202";s:7:"gc_name";s:12:"蜜饯果脯";}s:11:"goods_class";a:8:{i:220;a:2:{s:5:"gc_id";s:3:"220";s:7:"gc_name";s:9:"芒果干";}i:221;a:2:{s:5:"gc_id";s:3:"221";s:7:"gc_name";s:9:"葡萄干";}i:222;a:2:{s:5:"gc_id";s:3:"222";s:7:"gc_name";s:6:"梅类";}i:223;a:2:{s:5:"gc_id";s:3:"223";s:7:"gc_name";s:9:"菠萝干";}i:224;a:2:{s:5:"gc_id";s:3:"224";s:7:"gc_name";s:9:"草莓干";}i:225;a:2:{s:5:"gc_id";s:3:"225";s:7:"gc_name";s:9:"香蕉干";}i:226;a:2:{s:5:"gc_id";s:3:"226";s:7:"gc_name";s:9:"玫瑰脯";}i:227;a:2:{s:5:"gc_id";s:3:"227";s:7:"gc_name";s:6:"蓝莓";}}}}', '推荐分类'),
(9, 'array', 'act', 'a:2:{s:3:"pic";s:56:"upload/adv/web-1-13_53bfbfc958cb55a435545033bd075bf3.png";s:3:"url";s:0:"";}', '活动图片'),
(9, 'array', 'recommend_list', 'a:1:{i:1;a:2:{s:9:"recommend";a:1:{s:4:"name";s:16:"食品保健2222";}s:10:"goods_list";a:6:{i:33;a:5:{s:8:"goods_id";s:2:"33";s:8:"store_id";s:1:"2";s:10:"goods_name";s:75:"新疆特级哈密五堡大枣(500克)养颜滋补红枣 【演示数据】";s:11:"goods_price";s:5:"45.00";s:9:"goods_pic";s:69:"upload/store/goods/2/70cd400bc29f748ebde074e04f1ad081.jpeg_small.jpeg";}i:29;a:5:{s:8:"goods_id";s:2:"29";s:8:"store_id";s:1:"2";s:10:"goods_name";s:63:"Truffles德菲丝松露巧克力果仁味400g【演示数据】";s:11:"goods_price";s:5:"99.00";s:9:"goods_pic";s:67:"upload/store/goods/2/b3fa422271ee0e974af458a049ca7e77.jpg_small.jpg";}i:25;a:5:{s:8:"goods_id";s:2:"25";s:8:"store_id";s:1:"2";s:10:"goods_name";s:48:"正品连帽硬朗帅气夹克【演示数据】";s:11:"goods_price";s:6:"348.00";s:9:"goods_pic";s:67:"upload/store/goods/2/ed3755ac3f250e8a7be3c14343c67832.jpg_small.jpg";}i:24;a:5:{s:8:"goods_id";s:2:"24";s:8:"store_id";s:1:"2";s:10:"goods_name";s:62:"时尚都市舒适潮流长袖T恤edc-JE0722【演示数据】";s:11:"goods_price";s:6:"133.00";s:9:"goods_pic";s:67:"upload/store/goods/2/ea36f7ea0aff6af0a50674b1619f7702.jpg_small.jpg";}i:21;a:5:{s:8:"goods_id";s:2:"21";s:8:"store_id";s:1:"2";s:10:"goods_name";s:51:"正品edc系列连帽休闲夹克【演示数据】";s:11:"goods_price";s:6:"300.00";s:9:"goods_pic";s:67:"upload/store/goods/2/ddfcab24bd812c466788eeba587f4057.jpg_small.jpg";}i:28;a:5:{s:8:"goods_id";s:2:"28";s:8:"store_id";s:1:"2";s:10:"goods_name";s:66:"正品都市时尚女装假两件优雅针织衫【演示数据】";s:11:"goods_price";s:6:"182.00";s:9:"goods_pic";s:67:"upload/store/goods/2/04fb225ea46bd1346f330400eedb7ef2.jpg_small.jpg";}}}}', '商品推荐'),
(9, 'array', 'goods_list', 'a:2:{s:4:"name";s:12:"商品排行";s:5:"goods";a:7:{i:67;a:5:{s:8:"goods_id";s:2:"67";s:8:"store_id";s:1:"2";s:10:"goods_name";s:77:"优之良品橡皮糖黄芒果橡皮糖软糖零食QQ糖250【演示数据】";s:11:"goods_price";s:5:"18.00";s:9:"goods_pic";s:67:"upload/store/goods/2/6f8ff741b6c12a2d6f9cce86eb6cf1ad.jpg_small.jpg";}i:69;a:5:{s:8:"goods_id";s:2:"69";s:8:"store_id";s:1:"2";s:10:"goods_name";s:84:"福建特产蜜饯话梅旺梅酸甜可口肉质爽甜健脾开胃【演示数据】";s:11:"goods_price";s:5:"22.80";s:9:"goods_pic";s:67:"upload/store/goods/2/52f831e8e55240c3f9d529976b88f8f0.jpg_small.jpg";}i:71;a:5:{s:8:"goods_id";s:2:"71";s:8:"store_id";s:1:"2";s:10:"goods_name";s:76:"梅怡馆大畈屋梅饴馆生梅老梅干礼盒1/1 160克【演示数据】";s:11:"goods_price";s:5:"39.00";s:9:"goods_pic";s:69:"upload/store/goods/2/6d9d3912f417bb1cd5c77264e35a7431.jpeg_small.jpeg";}i:30;a:5:{s:8:"goods_id";s:2:"30";s:8:"store_id";s:1:"2";s:10:"goods_name";s:75:"燕之坊即冲粗粮豆米浆补气黑芝麻味单包28g【演示数据】";s:11:"goods_price";s:4:"1.00";s:9:"goods_pic";s:67:"upload/store/goods/2/984600f5e9d1a07163cbe01e7500ad11.jpg_small.jpg";}i:23;a:5:{s:8:"goods_id";s:2:"23";s:8:"store_id";s:1:"2";s:10:"goods_name";s:76:"武陵泰味酱板系列酱板鸭脖礼盒装400g/内含40【演示数据】";s:11:"goods_price";s:5:"40.00";s:9:"goods_pic";s:69:"upload/store/goods/2/c9d06fe0d1bdbbef07b4a68fb6d826b8.jpeg_small.jpeg";}i:29;a:5:{s:8:"goods_id";s:2:"29";s:8:"store_id";s:1:"2";s:10:"goods_name";s:63:"Truffles德菲丝松露巧克力果仁味400g【演示数据】";s:11:"goods_price";s:5:"99.00";s:9:"goods_pic";s:67:"upload/store/goods/2/b3fa422271ee0e974af458a049ca7e77.jpg_small.jpg";}i:26;a:5:{s:8:"goods_id";s:2:"26";s:8:"store_id";s:1:"2";s:10:"goods_name";s:84:"法国进口德菲丝/德菲斯松露巧克力 浓情古典系列 【演示数据】";s:11:"goods_price";s:5:"99.00";s:9:"goods_pic";s:67:"upload/store/goods/2/8a0cfade0b152c137a6855c580efeaa9.jpg_small.jpg";}}}', '排行类型'),
(9, 'array', 'adv', 'a:2:{s:3:"pic";s:56:"upload/adv/web-1-18_4c91b4889516f10059e6ccf921542323.gif";s:3:"url";s:0:"";}', '广告图片'),
(9, 'array', 'brand_list', 'a:8:{i:11;a:3:{s:8:"brand_id";s:2:"11";s:10:"brand_name";s:6:"卡帕";s:9:"brand_pic";s:49:"upload/brand/354b80528d2fbeefbab33c563532517e.gif";}i:1;a:3:{s:8:"brand_id";s:1:"1";s:10:"brand_name";s:11:"耐克/Nike";s:9:"brand_pic";s:49:"upload/brand/1d2dfbead590510046a6522551db8139.gif";}i:15;a:3:{s:8:"brand_id";s:2:"15";s:10:"brand_name";s:6:"茵宝";s:9:"brand_pic";s:49:"upload/brand/26247430b09daa1b441b46008bfb6e6e.gif";}i:10;a:3:{s:8:"brand_id";s:2:"10";s:10:"brand_name";s:6:"匡威";s:9:"brand_pic";s:49:"upload/brand/a0ac8c6d2d3dc1470d5876923182a8e2.gif";}i:12;a:3:{s:8:"brand_id";s:2:"12";s:10:"brand_name";s:21:"新百伦/new balance";s:9:"brand_pic";s:49:"upload/brand/9c5dee77a6ecdafd9e152fed8c6a4e90.gif";}i:2;a:3:{s:8:"brand_id";s:1:"2";s:10:"brand_name";s:19:"阿迪达斯/adidas";s:9:"brand_pic";s:49:"upload/brand/b175883eba95e793affb1b1ebbbf85a5.gif";}i:9;a:3:{s:8:"brand_id";s:1:"9";s:10:"brand_name";s:6:"安踏";s:9:"brand_pic";s:49:"upload/brand/6e61a1c953e5bc8c5f1ffdac36862245.gif";}i:8;a:3:{s:8:"brand_id";s:1:"8";s:10:"brand_name";s:13:"李宁/LINING";s:9:"brand_pic";s:49:"upload/brand/209abd835cd2ce2208f2dc42ba10efb4.gif";}}', '品牌推荐');</pre>
这段代码好长，其实你可以自己尝试一下，导出shopnc_web_code表的记录，然后保留一组，修改一下web_id就变成我这个数据了噢。简单吧~~

执行完毕，然后回到管理界面，点击板块编辑，你会发现这个时候可以编辑了，然后，开始配置吧~然后你会看到前台出现自定义的板块了，但是如果你严格按照我的教程来做的话，你会发现好丑，因为没有样式，只有基本样式噢。

接着我们要定义一下样式，要不不好看噢。

请打开/templates/default/css/home_index.css文件，然后我们要找一下原先的样式来参考模仿吧，我这边参考style-gray样式：
<pre>/* abc自定义 */
.style-abc .leftbar, .style-gray .middle, .style-gray .tabs-nav, .style-gray .bottom-bar { border-color:#CECEBF;}
.style-abc .leftbar .category dl { border-color:#E7E7DC;}
.style-abc .leftbar, .style-gray .tabs-nav {background:#F8F8EF;}
.style-abc .tabs-nav li.tabs-selected { border-color:#79796A #CECEBF #FFF #CECEBF;}
.style-abc .tabs-nav li.tabs-selected a, .style-gray .leftbar .category dl dt a:hover {color:#79796A; }
.style-abc .bottom-bar ul.brands li:hover .brands-name a, .style-gray .leftbar .category dl dd a:hover { background-color: rgba(121,121,106,0.95); filter:progid:DXImageTransform.Microsoft.gradient(enabled='true',startColorstr='#D879796A', endColorstr='#D879796A')/*IE*/; text-shadow: 1px 1px 0 rgba(121,121,106,1);}
:root .style-abc .bottom-bar ul.brands li:hover .brands-name a, :root .style-abc .leftbar .category dl dd a:hover{filter:none;}/*for IE9*/</pre>
然后保存，你会发现，新增加的楼层板块的样式跟灰色板块样式是一样的~~~这个是废话吗~~哈，然后你就根据需要一个个的修改过去噢，我这边就不废话了。

补充：

2013.12.10 抽空二次开发了一下，增加了一个复制的功能，让你可以自动增加新的楼层了，有需要的同学可以加我的微信，然后索取。微信账号是：php2dev，二维码请查阅首页右侧栏。