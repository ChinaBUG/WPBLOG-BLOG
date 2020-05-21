---
ID: 4221
post_title: >
  ECShop二次开发研究日志之1.主页商品如何获取商品相册并显示及商品分类页获取商品相册相册列表
author: ChinaBUG
post_excerpt: >
  通过本文，您将学会如何去分析一个需求做ECShop的商城系统二次开发。本文通过一个简单的需求：主页商品如何获取商品相册并显示及商品分类页获取商品相册相册列表。来一步步分析如何去做开发。可以说本文是一个二次开发的入门篇，当然，你不懂得语法的话就不算入门篇了。
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecshop-2dev-diary-1-home-products-how-to-get-the-product-album-and-display-and-merchandise-categories-page-to-get-a-list-of-products.html
published: true
post_date: 2015-08-21 10:44:21
---
<p>
	序
</p>

<p>
	前几天，一个网友问我如何获取商品分类页的商品相册列表，然后给我一个教程《<a href="http://www.bokee.net/bloggermodule/blog_viewblog.do?id=18350530">ecshop商品分类页获取相册列表方法</a>》，他按照教程做，分页是可以的，不过要把这个功能弄到主页上就找不到修改的位置所在了。
</p>

<p>
	这个问题是典型的！！初学者问题。
</p>

<p>
	因为初学者最缺的就是一个开发思路，所以借此，正好最近项目忙，累死了，放松一下，写一下分享一下吧。有什么问题高手一笑而过吧。
</p>

<p>
	文
</p>

<p>
	开发思路分析，我们希望在主页显示商品的相册，那么我们就应该
</p>

<ol>
	<li>
		找到显示商品的所在；
	</li>
	<li>
		找到调用的方法或者变量所在；
	</li>
	<li>
		然后根据需要获取相册信息；
	</li>
	<li>
		然后输出相册信息到模板中
	</li>
</ol>

<p>
	我的分析思路如下：
</p>

<p>
	我先查看主页的&ldquo;精品推荐》全部&rdquo;这个区域，然后我查看源码，找到这边的关键词：id=&quot;show_best_area&quot;，然后我就去/themes/default/index.dwt文件中查找这个关键词，你会发现这边的输出逻辑是：
</p>

<p>
	&lt;!--{foreach from=$best_goods item=goods}--&gt;
</p>

<p>
	是的，就是best_goods这个变量名，那么我们自然就知道应该去根目录的index.php文件中查找这个变量了，然后你会找到：
</p>

<p>
	$smarty-&gt;assign(&#39;best_goods&#39;,&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; get_recommend_goods(&#39;best&#39;));&nbsp;&nbsp;&nbsp; // 推荐商品
</p>

<p>
	这一行代码，有点基础的话，你就懂得这个get_recommend_goods方法是关键，我们全站搜索一下这个方法，你会在下面的位置发现这个方法所在的文件：
</p>

<p>
	/includes/lib_goods.php
</p>

<p>
	然后我们会在这个方法里面找到如下代码处：
</p>

<p>
	$goods[$idx]['url']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = build_uri('goods', array('gid' => $row['goods_id']), $row['goods_name']);
</p>

<p>
	剩下的就是想办法获取商品相册列表了，这边直接用上面教程内使用的方法，做了一点点修改：
</p>

<p>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $goods[$idx]['url']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = build_uri('goods', array('gid' => $row['goods_id']), $row['goods_name']);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;// 2015.08.20 ChinaBUG 前台调用商品相册<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$sql = &#39;SELECT img_url,thumb_url FROM &#39; . $GLOBALS['ecs']->table('goods_gallery') .' WHERE goods_id = ' . $row['goods_id'];<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods_photo_gallery = $GLOBALS['db']-&gt;getAll($sql);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods[$idx]['gpic']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = $goods_photo_gallery;<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;//<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if (in_array($row['goods_id'], $type_array['best']))
</p>

<p>
	保存，然后查看一下你会发现啥效果也没有。这个当然的事情，因为使用MVC架构的，都是这个鸟样子，你改了控制器，你还需要修改一下视图文件，也就是模板文件。
</p>

<p>
	请打开/themes/default/index.dwt文件，找到
</p>

<p>
	&nbsp; &lt;!--{foreach from=$best_goods item=goods}--&gt;
</p>

<p>
	所在的位置，在这个循环体里面找一个你希望加入相册的地方，把下面的代码加上去就可以了，这边我是加在价格下方：
</p>

<p>
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&lt;/font&gt;<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&lt;div&gt;
</p>

<p>
	&nbsp;&nbsp;&nbsp; &lt;!-- {foreach from=$goods.gpic item=picture name=no}--&gt;<br />
	&nbsp;&nbsp;&nbsp; &lt;!-- {if $smarty.foreach.no.iteration &lt; 5}&nbsp; --&gt;<br />
	&nbsp;&nbsp;&nbsp; &lt;img class=&quot;galleryitem&quot; src=&quot;{if $picture.thumb_url}{$picture.thumb_url}{else}{$picture.img_url}{/if}&quot; width=&quot;30&quot; alt=&quot;{$goods.goods_name}&quot; data-s=&quot;{if $picture.thumb_url}{$picture.thumb_url}{else}{$picture.img_url}{/if}&quot; /&gt;<br />
	&nbsp;&nbsp;&nbsp; &lt;!--{/if}--&gt; &nbsp;<br />
	&nbsp;&nbsp;&nbsp; &lt;!--{/foreach}--&gt;
</p>

<p>
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&lt;/div&gt;
</p>

<p>
	这个代码也是来至上面说的教程，我就修改一个样式名哈，用于调整图片大小。
</p>

<p>
	然后，请打开/library/recommend_best.lbi这个文件，在相应的位置上也增加如上代码。
</p>

<p>
	<em><u>注：这个文件的内容跟index.dwt相关位置是一致的。</u></em>
</p>

<p>
	好了，请保存，然后进后台，清空缓存。刷新首页，你会发现，商品下面多了一些图片了。
</p>

<p>
	然后，你要是点击全部边上的分类你会发现，居然还是没有增加图片相册，这个说明这个地方获取的数据你没改到，我们再来改改哈。
</p>

<p>
	我们先查看这个的源码你会发现他是调用一个JS的方法get_cat_recommend来获取数据的，有经验的童靴肯定猜到这个地方是使用了AJAX来获取数据的，然后我们再来查看一下这个是怎么调用的，具体调用哪个方法。打开index.php文件我们会发现在74行：
</p>

<p>
	//判断是否有ajax请求<br />
	$act = !empty($_GET['act']) ? $_GET['act'] : &#39;&#39;;<br />
	if ($act == &#39;cat_rec&#39;)<br />
	......
</p>

<p>
	然后看一下就知道了，原来调用的是get_category_recommend_goods方法获取数据的，然后我们又一次查找这个方法，会发现原来还是在/includes/lib_goods.php文件中，那么还是按照之前的办法，找到里面的：
</p>

<p>
	$goods[$idx]['url']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = build_uri('goods', array('gid' => $row['goods_id']), $row['goods_name']);
</p>

<p>
	修改为：
</p>

<p>
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods[$idx]['url']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = build_uri('goods', array('gid' => $row['goods_id']), $row['goods_name']);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;// 2015.08.20 ChinaBUG 前台调用商品相册<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$sql = &#39;SELECT img_url,thumb_url FROM &#39; . $GLOBALS['ecs']->table('goods_gallery') .' WHERE goods_id = ' . $row['goods_id'];<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods_photo_gallery = $GLOBALS['db']-&gt;getAll($sql);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods[$idx]['gpic']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = $goods_photo_gallery;
</p>

<p>
	保存，然后清空缓存，查看一下原来结果还是可以的哈。
</p>

<p>
	<u><em><strong>友情提示：其实，其他一些商品相关的方法均可以按照以上方法直接添加代码的，具体相关代码都包含在/includes/lib_goods.php文件中，请从头到尾，自己添加一次吧。然后，你就会发现原来功能都实现了，修改一下模板，清空缓存就可以看到效果了。</strong></em></u>
</p>

<p>
	首页效果已经完成，那么商品分类的要怎么获取呢？原先的教程写的有点点的复杂，可能大家看不懂吧，我还是再简单分析一下吧。
</p>

<p>
	我们还是先打开http://shop.ipodmp.com/category.php?id=2然后，查找你需要添加的位置，我这边是：
</p>

<p>
	&lt;!--{foreach from=$best_goods item=goods}--&gt;
</p>

<p>
	然后还是去找这个变量所在的程序，我们可以获取方法是get_category_recommend_goods，位置如下：
</p>

<p>
	$smarty-&gt;assign(&#39;best_goods&#39;,&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; get_category_recommend_goods(&#39;best&#39;, $children, $brand, $price_min, $price_max, $ext));
</p>

<p>
	然后会发现这个方法原来还是在/includes/lib_goods.php文件中，还记得我上面说的，这个文件相关的方法都按照我说的去修改一下就可以了吧？
</p>

<p>
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; // 2015.08.20 ChinaBUG 前台调用商品相册<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$sql = &#39;SELECT img_url,thumb_url FROM &#39; . $GLOBALS['ecs']->table('goods_gallery') .' WHERE goods_id = ' . $row['goods_id'];<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods_photo_gallery = $GLOBALS['db']-&gt;getAll($sql);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$goods[$idx]['gpic']&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; = $goods_photo_gallery;
</p>

<p>
	改一下，然后我们还是去模板那边找到要放的地方，把我们的输出语句给放进去，记得要修改两个地方噢，具体是那两个就看你有没有认真看本文了。只要上面的教程你看了，那么这边很简单就可以自己改好了。
</p>

<p>
	去试试吧~GOOD LUCK......^_^
</p>