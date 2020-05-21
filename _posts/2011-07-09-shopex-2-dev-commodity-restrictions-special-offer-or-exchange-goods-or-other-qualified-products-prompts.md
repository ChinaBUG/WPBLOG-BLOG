---
ID: 1531
post_title: '密码保护：ShopEX-二次开发-商品满额换购限制(特价品或者兑换品或者其他有条件的商品提示)[商品标签应用]'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-commodity-restrictions-special-offer-or-exchange-goods-or-other-qualified-products-prompts.html
published: true
post_date: 2011-07-09 14:01:50
---
<ul>
	<li>文章内容：ShopEX-二次开发-商品满额换购限制(特价品或者兑换品或者其他有条件的商品提示)[商品标签应用]</li>
	<li>文章难度：中等</li>
	<li>修改难度：中等</li>
	<li>二次开发：部分可以</li>
</ul>
准备工作：

需要新建标签格式为：<strong>满额换购-</strong><span style="color: #ff0000;">58</span>    签名四个字加-必填，后面数字是价格

下面就是具体的代码：

第一步，我们先新建<strong title="这个为二次开发目录">core2</strong>\model\system\cmd.tag.php文件，然后写下下面的代码，我们主要是在这边需要定义几个方法，方便整个业务的执行，具体请看:

&lt;?php

include_once( "shopObject.php" );
class cmd_tag extends mdl_tag
{

var $tableName = "sdb_tags";
var $idColumn = "tag_id";
var $textColumn = "tag_name";
var $use_recycle = false;
//判断是否满额换购的商品状态
//是则返回 数字 证明是兑换品.需要检查购物金额，不是则返回0-不需要任何处理
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
function getCartStateByGoodID( $tag_id = 0 ){
//获取购物车的总金额
$sale = &amp;$this-&gt;system-&gt;loadModel('trading/sale');
$cart = &amp;$this-&gt;system-&gt;loadModel('trading/cart');
$this-&gt;cart = $cart-&gt;getCart('all');
$trad = $sale-&gt;getCartObject($this-&gt;cart,$GLOBALS['runtime']['member_lv'],true);
$ttP = $trad['totalPrice'];
$tag_name = $this-&gt;getTagAllbyID( $tag_id );
$price=explode("-",$tag_name['tag_name']);
if( $price[1] &lt;= $ttP ){
return array('1',$price[1]);
}else{
return array('0',$price[1]);
}
}
//根据标签ID获取标签名
function getTagAllbyID( $tag_id =0 )
{
$aRet = $this-&gt;db-&gt;selectrow( "SELECT * FROM ".DB_PREFIX."tags WHERE tag_id=".$tag_id );
return $aRet;
}
}

?&gt;

第二步，定义好方法，接下来，我们需要找地方来调用方法噢，请打开core\shop\controller\ctl.product.php文件，这个文件我在二次开发时，出现很神奇的一件事情，那就是，新建同样的函数，系统还是调用原方法，对于新建立的方法理都不理！有人知道为什么的，请联系偶。

找到位于index方法内的

$this-&gt;system-&gt;responseCode(404);
}

在“}”下面写我们自己的代码哈

// 扩展：满额就送活动
// 检测商品是否为赠品
$tagM = $this-&gt;system-&gt;loadModel('system/tag');
$aGoodState = $tagM-&gt;getStateByGoodID( $aGoods['goods_id'] );
//数字 - 禁止不符合条件(购物车是否足额)进入，0 -允许进入
if($aGoodState!='0'){
//检查购物车是否满足金额，不足则提示，足则进入
$cartstate = $tagM-&gt;getCartStateByGoodID( $aGoodState );
if( $cartstate[0]!='1'){
//不可以兑换
$this-&gt;splash('failed', 'back' , __("对不起，您的购物金额未符合兑换礼品的最低金额$cartstate[1]元"));
}
}
// 扩展结束

代码里面都有注释，相信不用我多说了吧，这样就能对商品进行限制，只要是不符合条件的，都不能打开，不打开商品信息页，根本就不能添加到购物车中了~当然，这个是针对一般客户来说滴。

这个其实还能扩展，如果你的商品仅仅是礼品，不想让人订购的话，那么添加上面的代码即可执行了。一般人，肯定不会跳过这一步滴。

下面的第三步我们就是来处理这些非一般的客户滴~~

第三步，我们需要在购物支付，生成订单之前再做一次检测，防止一些别有用心的人，通过cookies伪造等方法进入购物车，更主要的一个因素是防止业务的流程错误哈，如果大家多想想的话，就会发现，我们的整个流程是：

先判断购物车总金额，在开启商品添加功能，那么我要是先购买很大的金额，那么我什么兑换的商品都能购买了对吧，接下去，我只要把我不要的东西给删了，哇哈哈，我的购物金额不就降下来了，而兑换品还乖乖的呆在购物车里面哈，这个时候一提交，我的订单就生成了~~

所以，在这边我们需要做的事情就是，检测购物车是否存在兑换商品，金额是否满足条件，另外，这边控制每单的兑换件数，总不能让人无限制的兑换吧^_^

上代码了：

请找到core2\shop\controller\cct.cart.php文件，要是没有的话，请自己新建并按照二次开发的规范来写哈

我们新建或者扩展checkout()方法，在函数开头内写下面的代码：

function checkout () {
......
// 扩展：满额就送活动
// 定义兑换礼品件数
$mustnum = 0;
$tagM = $this-&gt;system-&gt;loadModel('system/tag');
foreach($this-&gt;cart['g']['cart'] as $cartkey =&gt; $cartval){
$aTmp = explode( "-", $cartkey );//$aTmp[0]==Good_ID;
//判断是不是 兑换品
$aGoodState = $tagM-&gt;getStateByGoodID( $aTmp[0] );
//兑换品 则需要检查的最低购买额
if($aGoodState!='0'){
//兑换品，获取最低购买额
$cartstate = $tagM-&gt;getCartStateByGoodID( $aGoodState );
//判断一旦总额小于兑换金额，则提示
if( $cartstate[0]!='1'){
//不可以兑换
$this-&gt;splash('failed', 'back' , __("对不起，您的购物金额未符合兑换礼品的最低金额$cartstate[1]元"));
}
$mustnum +=1;
}
}
//多件兑换品则提示
//单件商品则判断：兑换品的价格与总价格是否符合
if( $mustnum &gt; 1){
$this-&gt;splash( "failed", 'back', __( '对不起，您有多件兑换品，请返回检查后再提交。' ),array(),3 );
}
// 扩展结束

好了~真是累哈，都修改了，就清空缓存，设置一下商品的标签，你就能看到效果了~~