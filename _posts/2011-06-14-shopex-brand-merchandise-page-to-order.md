---
ID: 1316
post_title: >
  密码保护：ShopEX-二次开发之品牌专区品牌页内的商品如何排序
author: ChinaBUG
post_excerpt: |
  ShopEX的品牌专区里面的品牌点击开后，查看商品列表时，排序怎么控制呢？本文就是解决这个问题滴。
  首先，查看商品列表是根据系统后台的商品浏览里面的展示方式来调用HTML文件模板滴，所以一定要在自己喜欢的展示方式下操作哈，要不可能会误会为代码没效果哈。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-brand-merchandise-page-to-order.html
published: true
post_date: 2011-06-14 12:28:11
---
ShopEX的品牌专区里面的品牌点击开后，查看商品列表时，排序怎么控制呢？本文就是解决这个问题滴。

首先，查看商品列表是根据系统后台的商品浏览里面的展示方式来调用HTML文件模板滴，所以一定要在自己喜欢的展示方式下操作哈，要不可能会误会为代码没效果哈。

现在我们先找到core/shop/controller/ctl.brand.php文件，在里面找到

$aProduct  = $objGoods-&gt;getList(null,$filter,$start,$pageLimit);

这一行，然后再修改为下面的代码即可：

$orderby = "price desc";//价格从高到低排序
//$orderby = array('price',' DESC',',goods_id',' DESC');//或者这个是先 价格 从高到低，然后是商品编号，从最新到最旧排序。
$aProduct  = $objGoods-&gt;getList(null,$filter,$start,$pageLimit,<strong><span style="text-decoration: underline;">$orderby</span></strong>);

至此，整个修改完成了，你查看时就能发现你的商品列表按照你的修改排列了。

注意：如果，你按照上面修改了代码，发觉没效果，别担心，请先到后台去<span style="text-decoration: underline;">清空缓存</span>吧，然后就能看到效果了。这样还不行？对不起，目前我是正常运行的，你有问题可以来交流。

<span style="text-decoration: underline;"><em>TIP:怎么清空缓存呢？请进后台，在左上角的登录信息上有一个 关于 项，点击，然后再选择 缓存系统，再点击 清空缓存 按钮。</em></span>

附录：getList()的原型

下面代码位于core\model\goods\mdl.products.php文件内

 function getlist( $cols, $filter = "", $start = 0, $limit = 20, $orderType = null )
 {
  if ( !function_exists( "goods_list" ) )
  {
   require( CORE_INCLUDE_DIR."/core/goods.list.php" );
  }
  return goods_list( $cols, $filter, $start, $limit, $orderType, $this );
 }

下面代码位于core\include\core\goods.list.php文件内

function goods_list( $cols, $filter = "", $start = 0, $limit = 20, $orderType = null, &amp;$object )
{
 $ident = md5( $cols.var_export( $filter, true ).$start.$limit.$orderType );
 if ( !$object-&gt;_dbstorage[$ident] )
 {
  if ( !$cols )
  {
   $cols = $object-&gt;defaultCols;
  }
  if ( $object-&gt;appendCols )
  {
   $cols .= ",".$object-&gt;appendCols;
  }
  $sql = "SELECT ".$cols." FROM ".$object-&gt;tableName." WHERE ".$object-&gt;_filter( $filter );
  if ( is_array( $orderType ) )
  {
   $orderType = trim( implode( " ", $orderType ) ) ? $orderType : $object-&gt;defaultOrder;
   if ( $orderType )
   {
    $sql .= " ORDER BY ".implode( " ", $orderType );
   }
  }
  else if ( $orderType )
  {
   $sql .= " ORDER BY ".$orderType;
  }
  else
  {
   $sql .= " ORDER BY ".implode( " ", $object-&gt;defaultOrder );
  }
  $count = $object-&gt;db-&gt;count( $sql );
  $rows = $object-&gt;db-&gt;selectlimit( $sql, $limit, $start );
  if ( isset( $filter['mlevel'] ) &amp;&amp; $filter['mlevel'] )
  {
   $oLv = $object-&gt;system-&gt;loadmodel( "member/level" );
   if ( $level = $oLv-&gt;getfieldbyid( $filter['mlevel'] ) )
   {
    foreach ( $rows as $k =&gt; $r )
    {
     $arrMp[$r['goods_id']] =&amp; $rows[$k]['price'];
     if ( 0 &lt; $level['dis_count'] )
     {
      $rows[$k]['price'] *= $level['dis_count'];
     }
    }
    if ( 0 &lt; count( $arrMp ) )
    {
     $sql = "SELECT goods_id,MIN(price) AS mprice FROM sdb_goods_lv_price WHERE goods_id IN (".implode( ",", array_keys( $arrMp ) ).") AND level_id=".intval( $filter['mlevel'] )." GROUP BY goods_id";
     foreach ( $object-&gt;db-&gt;select( $sql ) as $k =&gt; $r )
     {
      $arrMp[$r['goods_id']] = $r['mprice'];
     }
    }
   }
  }
  $object-&gt;_dbstorage[$ident] = $rows;
 }
 return $object-&gt;_dbstorage[$ident];
}