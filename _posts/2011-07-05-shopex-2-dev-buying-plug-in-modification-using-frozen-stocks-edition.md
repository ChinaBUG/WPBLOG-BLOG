---
ID: 1495
post_title: >
  密码保护：ShopEX-二次开发-秒杀抢购插件修改（使用冻结库存版）
author: ChinaBUG
post_excerpt: |
  原先ShopEX的秒杀抢购插件是根据商品的库存量来控制是否已售完，在我们的商城内出现的一个很奇怪的问题，就是明明有库存，却提示你，对不起这个商品已经售完，或者明明没有库存了，可是秒杀抢购那边却还是可以点击购买。。。
  
  所以，为了避免这个麻烦，我就对他做了修改，直接用冻结的库存量与总库存来综合判断。
  
  ~~~PS：还是那句话呀~~代码写好，可是太乱了，找时间整理再发布吧~先来占位置，提醒自己一下。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-buying-plug-in-modification-using-frozen-stocks-edition.html
published: true
post_date: 2011-07-05 23:23:09
---
<ul>
	<li>文章内容：ShopEX-二次开发-秒杀抢购插件修改（使用冻结库存版）</li>
	<li>文章难度：中等</li>
	<li>修改难度：高难|主要是对库存这个流程的处理比较头痛</li>
	<li>二次开发：可以</li>
</ul>
<a href="http://u.ipodmp.com/viewfile.php?file_id=246&amp;file_key=MwEMK535">秒杀限时抢购插件-原版</a>　　<a href="http://u.ipodmp.com/viewfile.php?file_id=247&amp;file_key=fUlIfySO">秒杀限时抢购插件-ChinaBUG修改版</a>

原先ShopEX的秒杀抢购插件是根据商品的库存量来控制是否已售完，在我们的商城内出现的一个很奇怪的问题，就是明明有库存，却提示你，对不起这个商品已经售完，或者明明没有库存了，可是秒杀抢购那边却还是可以点击购买。。。

所以，为了避免这个麻烦，我就对他做了修改，直接用冻结的库存量与总库存来综合判断。

在主要程序widget_qgou2side.php里面我们找到

$menusetting['buytarget'] = $system-&gt;getConf('site.buy.target');
$goods = $o-&gt;getList($o-&gt;defaultCols.',small_pic,view_count,buy_count',$filter);

在下面添加自己的代码：

///修改说明：
///抢购时，读取的是商品的库存字段，该字段必须在订单完成时方可更新。
///抢购插件需要实时的库存数量，即只要下订单即要扣除库存。
///订单废除或者删除则冻结库存恢复。
$objGoods = $system-&gt;loadModel('trading/goods');
$aGoods = $objGoods-&gt;getGoods( $goods_id );
$zkc = $aGoods['store'];
$zkc_dj = $aGoods['freez'];
$djkc = 0;
if( !isset($zkc_dj) || $zkc_dj==0){
for($i=0;$i&lt;count($aGoods['products']);$i++){
$djkc +=$aGoods['products'][$i]['freez'];
}
//print_r('外部没有值：'.$djkc);
}else{
$djkc = $zkc_dj;
//print_r('外部有值：'.$djkc);
}

//这边的运算符有点莫名其妙的，false的时候值都不定义
//默认有货
$ko = true;
if(($zkc - $djkc) &gt;=0){
//有库存
if(($zkc - $djkc) == 0){
//没货
$ko = false;
}else{
//有货
$ko = true;
}
}else{
//没货了
$ko = false;
}
$result['ko'] = $ko;

然后打开视图程序default.html，在里面找到

show_left_time('__time&lt;{$g.goods_id}&gt;',&lt;{$data.begin_lefttime}&gt;,&lt;{$data.end_lefttime}&gt;);

并将这段代码修改为

//alert("控制变量的值：&lt;{$data.ko}&gt;总库存量：&lt;{$data.zkc}&gt;总冻结量：&lt;{$data.djck}&gt;");
&lt;{if $data.ko==false}&gt;
show_left_time('__time&lt;{$g.goods_id}&gt;',0,0);
&lt;{else}&gt;
show_left_time('__time&lt;{$g.goods_id}&gt;',&lt;{$data.begin_lefttime}&gt;,&lt;{$data.end_lefttime}&gt;);
&lt;{/if}&gt;

然后保存，就可以使用了。

PS：经过验证，这个修改在一些应用时还是会出现问题滴，还是莫名其妙的出现库存不足的提示，但是明显比之前的好很多，肯定不是插件的问题，但是什么问题，目前猜测可能是商品表里面的冻结数量有误，具体，也没那个时间检测，暂时就这样用着吧。