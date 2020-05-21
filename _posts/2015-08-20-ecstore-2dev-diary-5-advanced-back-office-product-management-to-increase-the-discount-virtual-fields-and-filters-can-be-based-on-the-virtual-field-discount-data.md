---
ID: 4167
post_title: >
  ECStore二次开发日记之5.高级
  后台商品管理增加折扣虚拟字段并可以根据虚拟字段筛选折扣数据
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-5-advanced-back-office-product-management-to-increase-the-discount-virtual-fields-and-filters-can-be-based-on-the-virtual-field-discount-data.html
published: true
post_date: 2015-08-20 11:21:20
---
<p>
	商品管理的同仁希望能够根据市场价与销售价自动算出折扣，并且按照折扣来查询商品，方便做活动的时候挑选商品。
</p>

<p>
	一听这个需求，感觉好复杂呀，难道要修改数据库？
</p>

<p>
	好吧，其实不用，请出我们的猪脚吧：虚拟字段。
</p>

<p>
	ECStore,ShopEX4.85系列的都是有虚拟字段这个概念，虚拟字段指的是没有在数据库中定义的字段。
</p>

<p>
	那么我们怎么解决这个需求呢？
</p>

<p>
	思路是：
</p>

<ol>
	<li>
		先设定虚拟字段，然后显示折扣
	</li>
	<li>
		根据虚拟字段查询折扣值
	</li>
</ol>

<p>
	请先打开app &gt; b2c &gt; lib &gt; finder &gt; goods.php这个文件，然后在
</p>

<p>
	&nbsp; &nbsp; function __construct($app){<br />
	&nbsp; &nbsp; &nbsp; &nbsp; $this-&gt;app = $app;<br />
	&nbsp; &nbsp; }
</p>

<p>
	这个方法下面增加下面代码，其中FilterDiscount就是我们为这个需求定义的字段名，请遵照规范自己取一个吧。
</p>

<p>
	&nbsp; &nbsp; function __construct($app){<br />
	&nbsp; &nbsp; &nbsp; &nbsp; $this-&gt;app = $app;<br />
	&nbsp; &nbsp; }
</p>

<p>
	&nbsp;&nbsp; &nbsp;// 2015.08.07 ChinaBUG 需要增加一栏折扣，用以筛选。<br />
	&nbsp;&nbsp; &nbsp;var $column_FilterDiscount = &#39;折扣&#39;;<br />
	&nbsp;&nbsp; &nbsp;var $column_FilterDiscount_order = COLUMN_IN_HEAD;<br />
	&nbsp;&nbsp; &nbsp;var $column_FilterDiscount_width = 100;<br />
	&nbsp; &nbsp; function column_FilterDiscount($row){<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;return &#39;&lt;span style=&quot;color:red;font-weight: bold;&quot;&gt;&#39;.(round( $row['price']/$row['mktprice'] ,2 )*10) . &#39;&lt;/span&gt; 折&#39;;<br />
	&nbsp; &nbsp; }&nbsp;&nbsp; &nbsp;<br />
	&nbsp;&nbsp; &nbsp;//
</p>

<p>
	这个代码的功能就是增加一个字段，字段名为FilterDiscount，请按照前缀为&ldquo;$column_字段名&rdquo;来构造虚拟字段的属性，如上面的顺序是，虚拟字段的显示名，排序，宽度。他的数据来源请用&ldquo;function&nbsp;column_字段名($row)&rdquo;来定义方法，其中$row参数是系统自动获取符合的数据列。
</p>

<p>
	上面的方法定义一个虚拟字段，前台显示折扣，显示的内容是输出红色的xx折。具体请看下面的图片吧。
</p>

<p>
	<img alt="" src="http://blog.ipodmp.com/wp-content/uploads/2015/08/20150810-后台商品管理-增加折扣筛选.png" />
</p>

<p>
	漂亮吧^_^
</p>

<p>
	现在我们再来弄快速筛选区的功能。
</p>

<p>
	打开app &gt; b2c &gt; model &gt;goods.php文件，找到
</p>

<p>
	function searchOptions(){<br />
	......<br />
	}
</p>

<p>
	方法，然后在里面找到
</p>

<p>
	&#39;keyword&#39;=&gt;app::get(&#39;b2c&#39;)-&gt;_(&#39;商品关键字&#39;),
</p>

<p>
	在下面增加如下代码：
</p>

<p>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; $arr = array_merge($arr,array(<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &#39;bn&#39;=&gt;app::get(&#39;b2c&#39;)-&gt;_(&#39;货号&#39;),<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &#39;keyword&#39;=&gt;app::get(&#39;b2c&#39;)-&gt;_(&#39;商品关键字&#39;),<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;/* 2015.08.07 ChinaBUG 需要增加一栏折扣，用以筛选。 */<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&#39;FilterDiscount&#39; =&gt; &#39;折扣筛选&#39;,<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; // &#39;barcode&#39;=&gt;app::get(&#39;b2c&#39;)-&gt;_(&#39;条码&#39;),<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ));
</p>

<p>
	然后刷新一下，就会发现，在快速筛选框内会出现&ldquo;折扣筛选&rdquo;的选项了。
</p>

<p>
	就这么简单？！可以搜索了？
</p>

<p>
	是这么简单，但是还不能搜索，我们还需要作进一步的修改，但是！请放心，真的很简单~~
</p>

<p>
	我们再来找一下_filter()方法，然后在方法内写下面代码：
</p>

<p>
	function _filter($filter, $tbase = &#39;&#39;, $baseWhere = NULL){<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; /* 2015.08.10 ChinaBUG 需要增加一栏折扣，用以筛选。 */<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;if( isset( $filter['FilterDiscount'] ) &amp;&amp; $filter['FilterDiscount']&gt;0 ){<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;if( stristr($filter['FilterDiscount'],&#39;,&#39;)===false ){<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$filter['filter_sql'] = '((price / mktprice) *10) < ' . $filter['FilterDiscount'];<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;}else{<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$FilterDiscount = explode(&#39;,&#39;,$filter['FilterDiscount']);<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;$filter['filter_sql'] = '((price / mktprice) *10) BETWEEN ' . $FilterDiscount[0] . ' AND ' . $FilterDiscount[1];<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;}&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;unset( $filter['FilterDiscount'] );<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;}<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;<br />
	&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;foreach(kernel::servicelist(&#39;b2c_mdl_goods.filter&#39;) as $k=&gt;$obj_filter){<br />
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ......
</p>

<p>
	}
</p>

<p>
	保存，然后运行吧。
</p>

<p>
	现在是真的可以运行了。
</p>

<p>
	输入 5 则搜索 5 折以下的商品，如果输入 5,5.5 的话则查询 5-5.5 折之间的商品。是不是很简单呢？
</p>

<p>
	还不去赶紧试试看？！
</p>