---
ID: 1712
post_title: >
  密码保护：ShopEX 二次开发
  商品列表
  过滤商品列表-如何按地区显示相同商品
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-filter-the-list-of-goods-list-for-product-development-how-to-display-the-same-merchandise-by-region.html
published: true
post_date: 2011-07-28 22:12:10
---
//扩展：限制地区 @ ChinaBUG 2011.07.20
//注意：编码必须为Utf8方可工作
//过滤不符合条件的记录
$area_map = array_flip( array('厦门'=&gt;'xm','晋江'=&gt;'jj','石狮'=&gt;'ss','长泰'=&gt;'ct','泉州'=&gt;'qz','漳州'=&gt;'zz') );
//$system = $GLOBALS['system'];
if( !isset($system-&gt;area) || !$_COOKIE['area'] ){
$system-&gt;area = 'jj';
$_COOKIE['area'] = 'jj';
}
$filter[$i]['tag']='限售-'.$area_map[$system-&gt;area];
//强制的改变生成的SQL代码：在 in() 语句前加 not 字
$o-&gt;idColumn = "goods_id not ";
//扩展结束

&nbsp;

&nbsp;

==

core2\model\system\cmd.tag.php

&lt;?php

include_once( "shopObject.php" );
class cmd_tag extends mdl_tag
{

var $tableName = "sdb_tags";
var $idColumn = "tag_id";
var $textColumn = "tag_name";
var $use_recycle = false;
//判断是否满额换购的商品状态
//是则返回 数字 证明是兑换品.(==tag_id)-需要检查购物金额，不是则返回0-不需要任何处理
function getStateByGoodID( $good_id = 0 ){
$sql = 'select t.tag_name,t.tag_id FROM '.DB_PREFIX.'tags t LEFT JOIN '.DB_PREFIX.'tag_rel r ON r.tag_id=t.tag_id where rel_id='.$good_id;
$row = $this-&gt;db-&gt;select( $sql );
if($row){
foreach ( $row as $key =&gt; $val )
{
if ( strstr($val['tag_name'],'满额换购-') ){
return $val['tag_id'];
}
}
}
return '0';
}
//指定标签ID获取是否满足满额换购的资格
function getCartStateByGoodID( $tag_id = 0,&amp;$Cartinfo='' ){
//获取购物车的总金额
$sale = &amp;$this-&gt;system-&gt;loadModel('trading/sale');
$cart = &amp;$this-&gt;system-&gt;loadModel('trading/cart');
$this-&gt;cart = $cart-&gt;getCart('all');
$Cartinfo = $this-&gt;cart;
$trad = $sale-&gt;getCartObject($this-&gt;cart,$GLOBALS['runtime']['member_lv'],true);
//return $trad;
//print_r('=====================');
//exit;
$ttP = $trad['totalPrice'];
$tag_name = $this-&gt;getTagAllbyID( $tag_id );
$price=explode("-",$tag_name['tag_name']);
if( $price[1] &lt;= $ttP ){
//可以兑换
return array('1',$price[1],$ttP);
}else{
//不能兑换
return array('0',$price[1],$ttP);
}
}
//根据标签ID获取标签名
function getTagAllbyID( $tag_id =0 )
{
$aRet = $this-&gt;db-&gt;selectrow( "SELECT * FROM ".DB_PREFIX."tags WHERE tag_id=".$tag_id );
return $aRet;
}

//判断销售地区限制
//标签格式：限售-泉州
//返回 1 为允许在该地销售 0 为不允许
function getAreaBytag( $good_id = 0 ){
//定义地区与Ip自动获取地区相关
$area_map = array('厦门'=&gt;'xm','晋江'=&gt;'jj','石狮'=&gt;'ss','长泰'=&gt;'ct','泉州'=&gt;'qz','漳州'=&gt;'zz');
$sql = 'select t.tag_name,t.tag_id FROM '.DB_PREFIX.'tags t LEFT JOIN '.DB_PREFIX.'tag_rel r ON r.tag_id=t.tag_id where rel_id='.$good_id;
$row = $this-&gt;db-&gt;select( $sql );

$this-&gt;system = $GLOBALS['system'];
//$this-&gt;system-&gt;area;
//$now_area = $_COOKIE['area'] ? $_COOKIE['area'] : 'jj';//xm
$now_area = $this-&gt;system-&gt;area ? $this-&gt;system-&gt;area : 'jj';
if($row){
foreach ( $row as $key =&gt; $val )
{
if ( strstr($val['tag_name'],'限售-') ){
$tn=explode("-",$val['tag_name']);
$area2 = $tn[1];//如 泉州
if($now_area!==$area_map[$area2]){
//return '不同地区';//允许销售
//return array('0',$area2,$now_area);
}else{
//return '同一个地区';//不允许销售
return array('1',$area2,$now_area);
}
}
}
}
return array('0','全国地区',$now_area);
}
//判断销售地区限制
//标签格式：限售-泉州
function getAllAreaBytag( $good_id = 0 ){
$rstr = "";
$area_map = array('厦门'=&gt;'xm','晋江'=&gt;'jj','石狮'=&gt;'ss','长泰'=&gt;'ct','泉州'=&gt;'qz','漳州'=&gt;'zz');
$sql = 'select t.tag_name,t.tag_id FROM '.DB_PREFIX.'tags t LEFT JOIN '.DB_PREFIX.'tag_rel r ON r.tag_id=t.tag_id where rel_id='.$good_id;
$row = $this-&gt;db-&gt;select( $sql );
$this-&gt;system = $GLOBALS['system'];
//$this-&gt;system-&gt;area;
//$now_area = $_COOKIE['area'] ? $_COOKIE['area'] : 'jj';//xm
$now_area = $this-&gt;system-&gt;area ? $this-&gt;system-&gt;area : 'jj';
if($row){
foreach ( $row as $key =&gt; $val )
{
if ( strstr($val['tag_name'],'限售-') ){
$tn=explode("-",$val['tag_name']);
$rstr .=$tn[1].'城区 ';
}
}
}
if($rstr){
//$rstr = '全站(除'.$rstr.')';
//$rstr = "除 $rstr 地区外，全站销售";
$rstr = "$rstr 暂不提供销售。";
}else{
$rstr = '全站销售';
}
return $rstr;
}
////////////////////////
}

?&gt;