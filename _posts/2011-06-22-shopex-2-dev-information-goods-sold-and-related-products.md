---
ID: 1391
post_title: >
  密码保护：ShopEX-二次开发之商品信息-商品销售量(分型号统计与总销售量)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-2-dev-information-goods-sold-and-related-products.html
published: true
post_date: 2011-06-22 11:01:55
---
二次开发之商品信息-商品销售量

　　这次我们的目标是增加商品的销售量，只要按照下面的做，就能够在任何地方添加一个区域，显示商品的销售量，这个销售量是区分型号等，就是说你的商品不同类型的，这边都能显示出来，如果需要总计的销售量请看后面

1.分类型显示销售量

首先，请修改 core/shop/view/product/index.html 文件，在需要显示的地方添加下方代码：

&lt;!--ADD:ChinaBUG@20110621:商品详细-商品销售量--&gt;    
&lt;{if $isshow==true}&gt;
&lt;div&gt;
 &lt;div&gt;
        &lt;{foreach from=$data item=data key=key}&gt;
   &lt;div align="center" style="float:left"&gt;&lt;{$data.name}&gt;&lt;/div&gt;
            &lt;div align="center" style="float:right"&gt;&lt;{$data.saleTimes}&gt;&lt;{$goods.unit|default:件}&gt;&lt;/div&gt;
            &lt;div style="clear:both;"&gt;&lt;/div&gt;
        &lt;{/foreach}&gt;
 &lt;/div&gt;
&lt;/div&gt;
&lt;{/if}&gt;

然后，请找到 core/shop/controller/ctl.product.php 文件，并在index函数里面随便找个地方写下下面的代码（如果你怕插错地方，那就跟我一样添加在$objCat = &amp;$this-&gt;system-&gt;loadModel('goods/productCat');这段代码上面吧）：

////这边增加商品的销售量
    $objC = &amp;$this-&gt;system-&gt;loadModel('utility/salescount');
    $aC = $objC-&gt;count_bybn($aGoods['bn']);
 if($aC){
  $this-&gt;pagedata['isshow'] = true;
  }
 $this-&gt;pagedata['data'] = $aC;

<a name="salescount"></a>然后请到自己的二次开发目录内新建 core2/model/utility/cmd.salescount.php 文件（文件存在则打开），将下面代码添加进去：

 function count_bybn( $bn){
  $ordertype = array( "1" =&gt; "saleTimes", "2" =&gt; "salePrice" );
  $method = array( "1" =&gt; "ASC", "2" =&gt; "DESC" );
  if ( is_array( $order ) )
  {
   $order = " order by ".$ordertype[$order['order']]." ".$method[$order['method']];
  }
  else
  {
   $order = " order by ".$ordertype[1]." ".$method[2];
  }
  $sql="SELECT O.name, sum( O.nums ) AS saleTimes, sum( O.amount ) AS salePrice
    FROM sdb_payments AS P
    LEFT JOIN sdb_order_items AS O ON P.order_id = O.order_id
    LEFT JOIN sdb_orders AS Q ON P.order_id = Q.order_id
    LEFT JOIN sdb_members AS M ON P.member_id = M.member_id
    WHERE P.disabled = \"false\"
    AND P.status = \"succ\"
    and O.bn in (SELECT ps.bn FROM sdb_products as ps where ps.goods_id in (SELECT gs.goods_id FROM sdb_goods as gs where gs.bn = \"".$bn."\"))
    GROUP BY O.name".$order;
    
  $resu = $this-&gt;db-&gt;select( $sql );
  return $resu;
 }

2.不分类型只显示总销售量

修改 core/shop/view/product/index.html 文件，找到想要显示的地方，我这边是显示在销售价格后面，所以找到&lt;li&gt;&lt;span&gt;&lt;{t}&gt;销售价：&lt;{/t}&lt;/span&gt;在&lt;/li&gt;前，加上下面这段代码：

&lt;!--ADD:ChinaBUG@20110621:商品详细-商品销售量--&gt;
&lt;{if $isshow==true}&gt;
&lt;{t}&gt;&amp;nbsp;销售量：&lt;{/t}&gt;&lt;{$all_count}&gt;&amp;nbsp;&amp;nbsp;&lt;{$goods.unit|default:件}&gt;
&lt;{/if}&gt;

找到 core/shop/controller/ctl.product.php 文件内，将我们修改的代码加进去：

////这边增加商品的销售量
    $objC = &amp;$this-&gt;system-&gt;loadModel('utility/salescount');
    $aC = $objC-&gt;count_bybn($aGoods['bn']);
    //$this-&gt;pagedata['xsl2011'] = "返回值是：".$aGoods['bn'];
 if($aC){
  $this-&gt;pagedata['isshow'] = true;
  }
  //$this-&gt;pagedata['isshow'] = true;
 foreach($aC as $row){
  $a_c += $row['saleTimes'];
  }
 $this-&gt;pagedata['all_count'] = $a_c;

其他的core2/model/utility/cmd.salescount.php的代码跟<a href="#salescount">上面</a>的代码一样，我就不重复了。