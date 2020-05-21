---
ID: 3967
post_title: >
  ShopNC二次开发研究日记13:项目从哪里开始，怎么做二次开发，二次开发文档下载
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/shopnc-2dev-study-of-diary-13-project-where-to-start-what-to-do-the-secondary-development.html
published: true
post_date: 2015-03-19 14:26:14
---
本文导读
<ol>
	<li>ShopNC项目从哪里开始看
<ul>
	<li>ShopNC项目的开始点</li>
	<li>怎么做二次开发之分类筛选功能只能在二级分类中显示</li>
</ul>
</li>
	<li>ShopNC开发文档下载</li>
</ol>
今天查看统计报告，发现有人查询：
<blockquote>“shopnc项目从哪里开始看”这个关键词还转到我的博客上的<a href="http://blog.ipodmp.com/archives/shopnc-2dev-study-of-diary-1-how-to-modify-the-number-of-commodity-classification/">ShopNC二次开发研究日记1:怎么修改商品分类的个数(解除商品分类8个的限制)
</a>“shopnc二次开发文档”这个关键词转到<a href="http://blog.ipodmp.com/archives/shopnc-2dev-study-of-diary-9-how-to-modify-and-develop-the-product-area-within-version-2-4/">ShopNC二次开发研究日记9:2.4版本内的商品地区怎么修改与开发</a>
“<span title="分类筛选功能只能在二级分类中显示 shopnc">分类筛选功能只能在二级分类中显示 shopnc</span>”这个关键词转到<a href="http://blog.ipodmp.com/archives/tag/shopnc/">标签：ShopNC</a></blockquote>
有点文不对题的感觉，正好，查询“<span title="分类筛选功能只能在二级分类中显示 shopnc">分类筛选功能只能在二级分类中显示 shopnc</span>”这位读者与我联系咨询相关问题，就借这个机会说一下上面三个问题吧。

<strong><span style="text-decoration: underline;">shopnc项目从哪里开始看</span></strong>

这个问题，其实我也不懂提问的人到底是想要问什么，只能我猜我猜，这边就当做：ShopNC项目从哪里开始开发，怎么做二次开发。

我们知道ShopNC是基于MVC框架，什么是MVC框架，请点击<a href="http://baike.baidu.com/link?url=9X1Q3jbcWg5pURpY8erZle5XyhpRj-YgPav3n_IdyBq9HEou966EtgNanlFRMQFSWQ-KGCxtTbG9yzujiFeweG9BC1NtwvApcxpvrBvvmgW1RCN102BFnRdJG_H-TIty81TMrfrhRbxGYNgzf7tn9K">查阅</a>，那么使用MVC框架的程序有一个特点，那就是有入口文件。

噢，现在知道了吧？

这个入口文件就是我们一切的开始，ShopNC这个程序的第一步初始化等操作均在index.php这个文件中，所以，要做开发的人一定要从这个文件开始看起，然后你会发现系统的通用方法库，系统的类库，系统的操作类，控制器，模块等之间的联系了。

index.php文件是我们做开发的一切的起点，但是，整整修改的地方却不是这个文件，而是在其他地方。

这个就是ShopNC项目从哪里开始开发，就是从index.php这个文件开始呗。^_^哈

怎么做ShopNC的二次开发，这个问题就复杂不复杂，简单不简单了。
<blockquote><span style="color: #999999;">如果大家做过ShopEX的二次开发，会觉得，那是一种享受，当然是相对的享受了，但是做ECShop还有ShopNC还有WordPress等系统的时候相当折磨。</span>
<span style="color: #999999;"> 折磨在于他们的任何修改，开发都是基于原代码，注意是原代码而不是源代码，什么意思？就是说这些程序一定是开源的，如果不开源真的没办法做开发了（你有源码的除外）。</span>
<span style="color: #999999;"> 开发完之后，你要是程序升级的话，那就是再累一次，重新开发了。这个就是缺点，他们开源什么的都是优点但是对于运营的人来说，这个绝对是缺点，因为不能升级，安全漏洞怎么处理？最后还的找专业人士来处理。</span>
<span style="color: #999999;"> 这个我会在另外的文章里面详细说说，这边就不占字数了。</span></blockquote>
复杂的话就是我需要告诉你很多的基础，你要是没基础，听起来不是很轻松，简单就是我讲的内容，你按我说的来做，相当的简单。

因为我自己不是科班毕业的，所以，大家怎么个开发思路我是不懂，所以从我自己的开发习惯来说，我称自己的开发方法为“解答式思维编程”，这个我有时间会专门写一个系列文章来分享的，其实，对于经常看我文章的读者来说，我的文章不仅仅是帮你解决一个问题，而是教你分析一个问题，这个方式，就是解答式思维编程的最佳体现了。

那么怎么做二次开发呢？

就拿“<span title="分类筛选功能只能在二级分类中显示 shopnc">分类筛选功能只能在二级分类中显示 shopnc</span>”这个需求来说吧，我们知道ShopNC的分类，多规格的筛选在分类页里的二级分类时会出现，但是在一级分类却没有显示。

请打开官方的测试站，<a href="http://www.shopnctest.com/c2c/2013/demo/index.php?act=search&amp;cate_id=18">点我打开</a>。

右边上方，黄色框住的地方。

我们的目标是将这个内容在一级分类的时候也能显示出来（目前能不能显示出来我们还不知道噢，我写这篇文章的时候也是不知道的！因为这个模块我没看，ShopEX的这个区域是可以的，本人已做开发，有需要的可以联系噢^_^）。

那么我们该怎么做呢？

第一步

啥都别想，直接查看这块的结构。我用火狐浏览器，你用什么都不要紧，要紧的是能查看他的HTML代码。

我查看到这个区域的class名是module_filter，你的不是的话我就不知道为啥了，请自行找问题（只要从官方下载的就会跟我一致）。然后使用编辑器，查找module_filter这个关键词。

然后你会找到好多东西，其中最重要的当然是

\templates\default\home\search.php

这个文件中。

你会看到

[php]&lt;div class=&quot;module_filter&quot;&gt;
&lt;div class=&quot;module_filter_line&quot;&gt;
&lt;?php if((isset($output['checked_spec']) &amp;&amp; is_array($output['checked_spec']))
......
[/php]

这个就是控制我们规格，品牌等筛选信息的所在了。

那么这个数据是怎么产生的呢？因为，你不知道这个数据的来源的话，那么你就没办法控制他显示出来。

第二步

直接打开文件

\control\search.php

查找关键词checked_spec，然后你会看到如下地方噢

[php]    /**
* 标识被选中的属性
*
*/
private function sign_checked(&amp;$param,&amp;$data){
......[/php]

必须的一些关键词都在里面噢，然后看了一下会发现，这个方法只是处理这些数据的，而这个数据如何生成的都没，看样子又要跑起来了，我们直接查找sign_checked，看看这个文件里面哪些地方调用到这个方法。

[php]    /**
* 生成商品分类详细缓存
* 文件格式  分类id@规格id_规格id@品牌id_品牌id@属性id_属性id
*
* @param array $param 需要的参数内容
* @return array 数组类型的返回结果
*/
private function get_goods_info_by_attr(){
//得到分类ID
$cate_id = intval($_GET['cate_id']) ? intval($_GET['cate_id']) : 0 ;
//得到规格id
$spec_id = $_GET['s_id'];
......
//标识被选中的规格,品牌,属性
$this-&gt;sign_checked($param,$_cache);
return $_cache;
}
......
//抛出搜索属性
Tpl::output('spec_array',$data['spec_array']);
Tpl::output('brand_array',$data['brand_array']);
Tpl::output('attr_array',$data['attr_array']);

//标识被选中的规格,品牌,属性
$this-&gt;sign_checked($param,$data);
[/php]

看样子生成数据的原理都在这个方法里面了。我们需要慢慢的阅读方法的代码，然后分析出我们需要的生成原理，然后修改为我们需要的东西噢。

第三步

3.1 推测一

get_goods_info_by_attr方法的开头如下代码：

[php]        //得到分类ID
$cate_id = intval($_GET['cate_id']) ? intval($_GET['cate_id']) : 0 ;
//得到规格id
$spec_id = $_GET['s_id'];
//得到品牌id
$brand_id = $_GET['b_id'];
//得到属性id
$attr_id = $_GET['a_id'];
//得到地区id
if(intval($_GET['area_id']) &gt; 0){
$area_id = intval($_GET['area_id']);
}
//由规格型号组合取得商品ID
$param = array();
$param['gc_id']        = $cate_id;
if($spec_id != ''){
$param['spec_id']    = explode(',', $spec_id);
}
if($brand_id != ''){
$param['brand_id']    = explode(',', $brand_id);
}
if($attr_id != ''){
$param['attr_id']    = explode(',', $attr_id);
}
if($area_id &gt; 0){
$param['area_id']    = $area_id;
}
[/php]

这个就是获取传入的参数，然后根据参数来获取数据的，那么我们就可以猜测一下，尝试一下，比如

[php]$cate_id = intval($_GET['cate_id']) ? intval($_GET['cate_id']) : 0 ;[/php]

这个默认情况下是0，那么如果我们给他默认为18可以吗？可以，运行一下看看结果。
<blockquote><span style="color: #999999;">提示：</span>
<span style="color: #999999;"> 18这个值是什么呢？这个值就是上方给出的官方测试站的cate_id的值，也就是分类的ID。你本地测试的时候请改为你知道的那个分类的ID噢，方便你的测试。</span></blockquote>
请在return $data; 之前将结果输出，查看一下结构。一步步的深入，一步步的将他的算法给破解了噢。

具体的就不多说了，请需要这个功能的童靴自行按照这个思路跟下去噢。

3.2 扩展

如果顺利的话，你现在应该已经获取他这个算法的具体逻辑了，那么你还不懂的改吗？

在这边我就说一下思路呗，具体代码还是靠你自己了噢。

懂得逻辑之后，不管多复杂，肯定是获取分类等参数参与计算，我们可以将默认的值用我们需要的值来替代即可。

比如在三级分类才会出现的筛选选项，要在二级显示怎么办呢？

请看下面的示意图噢
<table>
<tbody>
<tr>
<td colspan="3">A1</td>
</tr>
<tr>
<td>A11</td>
<td>A12</td>
<td>A13</td>
</tr>
<tr>
<td>A111.1 A111.2 A111.3</td>
<td>A112.1 A112.2 A112.3</td>
<td>A113.1 A113.2 A113.3</td>
</tr>
</tbody>
</table>
假设，我们目前知道的是A111.3的分类筛选项。而A11的分类筛选是不知道的，但是我们可以知道吗？

当然！

A111.3的计算是传入一个参数就是他的分类编号，那么A11就是有三个这样子的编号，对吧，只要将这三个都传入，那么获取的数据是不是就是我们需要的呢？

恩，纯理论说明完毕。上面的思路是按照ShopEX的逻辑来思考的，因为这个获取的算法逻辑我没有详细去看，所以，上面的仅仅是也许，不保证正确性。

注：

1、下午本来想安装一个演示版的，顺便分析一下，结果发现，我忘记原版的是加密的，因为IX的服务器，超级慢，就算了，至于本地的，正在做开发，也懒得在这个版本上做修改了，就分享一下思路好了。懒就是一个字哈~

2、等待有时间跟闲情逸致的时候在认真分析一下再来补充分享吧。

<strong><span style="text-decoration: underline;">ShopNC开发文档</span></strong>

其实我想说的是，如果你够勤奋，等你把整个ShopNC都看完，你也就了解了，这个时候是不需要开发文档的，但是，我知道你们一定是会唾弃我的，虽然早期我没有这份文档真的是自己一个个看底层，一边自己总结的。

站在巨人肩上可以看得更高，走的更远，但是也可能摔下来~

希望有了文档，大家还能坚持不懈，努力专研，共同进步。

下面就是咱的宝贝，非常棒的一份文档，别人的怎样，我不知道，但是我这份绝对是超值的，有了它，开发就是这么潇洒~

本站下载：<a href="http://files1.91zhaota.com/doc/201503/shopnc24handbook.rar">ShopNC2.4二次开发完全文档手册.chm</a>