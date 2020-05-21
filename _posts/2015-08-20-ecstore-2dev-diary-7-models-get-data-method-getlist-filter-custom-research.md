---
ID: 4213
post_title: >
  ECStore二次开发日记之7.模型获取数据方法getList的过滤条件定制研究
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-7-models-get-data-method-getlist-filter-custom-research.html
published: true
post_date: 2015-08-20 15:50:57
---
<p>
	在第6篇开发日志《<a href="http://blog.ipodmp.com/archives/ecstore-2dev-diary-5-advanced-back-office-product-management-to-increase-the-discount-virtual-fields-and-filters-can-be-based-on-the-virtual-field-discount-data/">ECStore二次开发日记之5.高级 后台商品管理增加折扣虚拟字段并可以根据虚拟字段筛选折扣数据</a>》中，可能大家对我在_filter中使用的$filter['filter_sql']变量有点奇怪，可能有的童靴想知道这个是怎么来的，今天就专门来分析这个问题的，除了这个还有什么是我们可以使用的。
</p>

<p>
	引
</p>

<p>
	我们先来看app&gt;b2c&gt;controller&gt;site&gt;product.php这个文件，这个是前端商品的控制器，我们在访问http://shopex.ipodmp.com/product-100.html时访问到的数据就是由这个控制器控制的。
</p>

<p>
	找到index方法处，我们会看到如下代码：
</p>

<p>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $productsModel = $this-&gt;app-&gt;model(&#39;products&#39;);<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $goodsModel = $this-&gt;app-&gt;model(&#39;goods&#39;);
</p>

<p>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ......
</p>

<p>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $itemProduct = $productsModel-&gt;getList(&#39;*&#39;,array(&#39;product_id&#39;=&gt;$productId));
</p>

<p>
	这段代码的用途是getList方法根据传入的过滤条件获取当前的商品信息噢。
</p>

<p>
	是不是觉得很简单，很高端呢？
</p>

<p>
	如果，我们在正常的运营中需要获取的数据是多个的怎么办？
</p>

<p>
	比如只需要获取规格中的其中两个的数据呢？
</p>

<p>
	你可以使用上面的getList方法获取到吗？
</p>

<p>
	今天就是来讲解一下要如何根据实际情况来获取这个数据的，当然，除了用getList方法之外，另外有一个比较直接的方法就是直接调用数据库，。直接写SQL语句获取，这个不是本篇日志需要讨论的，就不多说了。
</p>

<p>
	文
</p>

<p>
	下面分析的代码在：
</p>

<p>
	app &gt; dbeav &gt; lib &gt; filter.php
</p>

<p>
	中，有源码的朋友可以对照看。
</p>

<p>
	首先，我们来看一下系统定义的几类筛选类型：
</p>

<table border="1" cellpadding="1" cellspacing="1" style="width: 500px;">
	<tbody>
		<tr>
			<td>
				大于
			</td>
			<td>
				than
			</td>
			<td>
				&gt; $var
			</td>
		</tr>
		<tr>
			<td>
				小于
			</td>
			<td>
				lthan
			</td>
			<td>
				&lt; $var
			</td>
		</tr>
		<tr>
			<td>
				等于
			</td>
			<td>
				nequal
			</td>
			<td>
				= $var
			</td>
		</tr>
		<tr>
			<td>
				不等于
			</td>
			<td>
				noequal
			</td>
			<td>
				&lt;&gt; $var
			</td>
		</tr>
		<tr>
			<td>
				等于
			</td>
			<td>
				tequal
			</td>
			<td>
				= $var
			</td>
		</tr>
		<tr>
			<td>
				小于等于
			</td>
			<td>
				sthan
			</td>
			<td>
				&lt;= $var
			</td>
		</tr>
		<tr>
			<td>
				大于等于
			</td>
			<td>
				bthan
			</td>
			<td>
				&gt;= $var
			</td>
		</tr>
		<tr>
			<td>
				包含
			</td>
			<td>
				has
			</td>
			<td>
				like &#39;%$var%&#39;
			</td>
		</tr>
		<tr>
			<td>
				头部包含
			</td>
			<td>
				head
			</td>
			<td>
				like &#39;$var%&#39;
			</td>
		</tr>
		<tr>
			<td>
				尾部包含
			</td>
			<td>
				foot
			</td>
			<td>
				like &#39;%$var&#39;
			</td>
		</tr>
		<tr>
			<td>
				不包含
			</td>
			<td>
				nohas
			</td>
			<td>
				not like &#39;%$var%&#39;
			</td>
		</tr>
		<tr>
			<td>
				两者之间
			</td>
			<td>
				between
			</td>
			<td>
				{field}&gt;=$var[0] and {field}<=$var[1]
			</td>
		</tr>
		<tr>
			<td>
				在这个范围
			</td>
			<td>
				in
			</td>
			<td>
				in (&quot;.implode(&quot;,&quot;,(array)$var).&quot;) &quot;
			</td>
		</tr>
		<tr>
			<td>
				不在这个范围
			</td>
			<td>
				notin
			</td>
			<td>
				not in (&quot;.implode(&quot;,&quot;,(array)$var).&quot;) &quot;
			</td>
		</tr>
	</tbody>
</table>

<p>
	有了上面定义的筛选类型，我们就可以开始编写我们的过滤条件了。
</p>

<p>
	》获取没有多规格商品
</p>

<p>
	要获取没有多规格的商品，就是goods表内&#39;spec_desc&#39;字段为null那就是没有多规格。要怎么设置这个呢？
</p>

<p>
	$filter['spec_desc'] = null;
</p>

<p>
	就可以了~然后执行getList方法，获取到的数据就是没有规格商品了。
</p>

<p>
	》获取有规格的商品
</p>

<p>
	查看了代码发现没有一个直接的方法，所以这个只能使用定制的SQL语句了，那么要定制SQl语句就要使用我们的超级定制变量filter_sql了：
</p>

<p>
	$filter['filter_sql'] = &#39; {table}.spec_desc is not NULL &#39;;
</p>

<p>
	然后执行getList方法。
</p>

<p>
	》获取商品ID为100的商品
</p>

<p>
	我们知道商品的ID是用goods_id表示的，然后是100，那么就是
</p>

<p>
	$filter['goods_id'] = 100;
</p>

<p>
	执行之后就获取到了。
</p>

<p>
	》获取商品ID为100,1001的商品
</p>

<p>
	这个是多条件了要怎么做呢？其实，是支持数组的，然后系统会自动转换为IN语句：
</p>

<p>
	$filter['goods_id'] = array(100,1001);
</p>

<p>
	当然你也可以自定义$filter['filter_sql']了，自己试试看。
</p>

<p>
	》获取商品ID大于100的商品列表呢？
</p>

<p>
	这个时候我们需要使用到筛选类型了。
</p>

<p>
	$filter['goods_id|than'] = 100;
</p>

<p>
	这样子就可以了，在字段的后面增加一个筛选类型，用竖线隔开。当然，你还可以使用小于，等于，大于等于等等，具体参数是上方表格内设定好的。
</p>

<p>
	当然，我们还能使用下面的方式来设定，上面的语句我们可以变成下面的代码：
</p>

<p>
	$filter['goods_id'] = 100;
</p>

<p>
	$filter['_goods_id_search'] = &#39;than&#39;;
</p>

<p>
	这个的使用方式的前提是需要数据库定义，否则是无效的，简单的说就是默认的字段是你一般都支持。
</p>

<p>
	好了，本篇日志到此就完成了，总的其实最有用的还是$filter['filter_sql']变量哈。
</p>

<p>
	赶紧去试试看吧~^_^
</p>