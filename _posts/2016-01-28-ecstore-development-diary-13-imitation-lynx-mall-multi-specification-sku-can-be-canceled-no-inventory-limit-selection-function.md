---
ID: 4336
post_title: >
  ECStore二次开发日记之13.仿天猫商城多规格(SKU)可以取消，无库存限制选择功能
author: ChinaBUG
post_excerpt: |
  虫神曰：
  最近CEO特别烦躁，就是因为ECSTORE这个系统呀，没办法跟天猫一样，选择的商品没办法取消，因为我们的商品均都是低库存的，也就是有的抢手型号的库存量会很少，那么自然就是很快卖完，要是没有及时下架怎么办？客户会选择到，然后......
  其实嘛，我觉得这个不是大问题，不过他们的感觉不是很友好，非要做成跟天猫一样的效果，可以取消选择，可以重新选择......
  好吧~拿钱消灾，那就努力处理这个问题吧。下面就是解决方案，浪费我十天的时间。
layout: post
permalink: >
  http://blog.ipodmp.com/2016/01/ecstore-development-diary-13-imitation-lynx-mall-multi-specification-sku-can-be-canceled-no-inventory-limit-selection-function.html
published: true
post_date: 2016-01-28 12:20:45
---
虫神曰：

最近CEO特别烦躁，就是因为ECSTORE这个系统呀，没办法跟天猫一样，选择的商品没办法取消，因为我们的商品均都是低库存的，也就是有的抢手型号的库存量会很少，那么自然就是很快卖完，要是没有及时下架怎么办？客户会选择到，然后......

其实嘛，我觉得这个不是大问题，不过他们的感觉不是很友好，非要做成跟天猫一样的效果，可以取消选择，可以重新选择......

好吧~拿钱消灾，那就努力处理这个问题吧。下面就是解决方案，浪费我十天的时间。

更新记录：
<ol>



 	<li>2016.04.14
修复该功能不完善的地方，就是载入会刷新，另外就是选择有的商品规则是不存在的，则不显示的问题。修改后只要有的规格都是会显示并能根据需要做过滤。</li>
</ol>
效果截图：

<a href="http://blog.ipodmp.com/wp-content/uploads/2016/01/ecs_goods_info_selector.png" rel="attachment wp-att-4340"><img class="alignnone size-full wp-image-4340" src="http://blog.ipodmp.com/wp-content/uploads/2016/01/ecs_goods_info_selector.png" alt="ecs_goods_info_selector" width="658" height="512" /></a>

&nbsp;

思路分析：

在ShopEX4.85系列的时候，商品的多规格还是可以选择跟取消的，但是系统到ECStore的时候就变得不能取消了，然后分析一下京东跟天猫我们会发现：京东是每个SKU一个链接，不能选择跟取消的；而天猫则是全部的SKU都在一个页面，可以选择跟取消的。

从这个区别来看我们不能得出：要选择可以取消的必须是单页面的结论。

但是！我们可以得到一个思路，我们只要把全部的规格值的状态都在一个页面获取，然后就根据选择来显示就可以了。

正文

从上面的思路我们知道需要将我们需要的全部的规格的状态都获取出来，那么在哪里获取呢？我们打开/app/b2c/controller/site/product.php文件，经过分析我们知道这个的逻辑集中在：

//获取货品规格数据

function _get_goods_spec($aGoods){......}

方法中。

注：这边的主要逻辑是使用array_diff_assoc方法获取规格差集，这个思路很棒，前提是要先了解一下多规格的保存结构才能看懂哈。

然后我们就根据我们的思路，改写这个方法：
<blockquote>//获取货品规格数据
// 2016.01.13 ChinaBUG
function _get_goods_spec_two( $aGoods, $curspec=null ){
$goodsSpec = array();
// 商品的全部规格列表
$products = app::get('b2c')-&gt;model('products')-&gt;getList('product_id,spec_desc,store,freez,marketable',array('goods_id'=&gt;$aGoods['goods_id']));
if($aGoods['spec_desc']){//商品的多规格信息
foreach($aGoods['spec_desc'] as $specId=&gt;$specValue){ $arrSpecId['spec_id'][] = $specId;}
$arrSpecName = app::get('b2c')-&gt;model('specification')-&gt;getList('spec_name,spec_id,spec_type',$arrSpecId,0,-1,'p_order ASC');
foreach($arrSpecName as $specItem){
$goodsSpec['specification']['spec_name'][$specItem['spec_id']] = $specItem['spec_name'];
$goodsSpec['specification']['spec_type'][$specItem['spec_id']] = $specItem['spec_type'];
}
$goodsSpec['specification']['spec_count'] = count($goodsSpec['specification']['spec_name']);
$goodsSpec['goods'] = $aGoods['spec_desc'];
// 当前商品的规格信息
$goodsSpec['product'] = $aGoods['product']['spec_desc']['spec_private_value_id'];
foreach($products as $row){
$products_spec = $row['spec_desc']['spec_private_value_id'];
$diff_class = array_diff_assoc( $products_spec, $goodsSpec['product'] );//求出当前货品和其他货品规格的差集
if(count($diff_class) === 1){
$goodsSpec['goods'][key($diff_class)][current($diff_class)]['product_id'] = $row['product_id'];
$goodsSpec['goods'][key($diff_class)][current($diff_class)]['marketable'] = $row['marketable'];
if($row['store'] === '' || $row['store'] === null){
$product_store = '999999';
}else{
$product_store = $row['store']-$row['freez'];
}
$goodsSpec['goods'][key($diff_class)][current($diff_class)]['store'] = $product_store;
}
}
}
return $goodsSpec;
}</blockquote>
这个就是根据我们传入当前的规格来排查相关的规格的具体值，这个当前值有两个状态：一个是都是没有，都没有，那么过滤出来的规格就是全部的型号值了，一个是单独的规格不同的状态。

这个地方最好是画一张图，方便自己理解噢。

<a href="http://blog.ipodmp.com/wp-content/uploads/2016/01/ecstore_spec.png" rel="attachment wp-att-4347"><img class="alignnone size-full wp-image-4347" src="http://blog.ipodmp.com/wp-content/uploads/2016/01/ecstore_spec.png" alt="ecstore_spec" width="425" height="272" /></a>

仿写完毕，那么我们就需要调用，在前台整个的流程应该是，当我点击一个规格时，那么这个规格的值直接提交给我后台处理，辨别此时应该显示的规格的完整信息，下面就是处理这个逻辑的方法：
<blockquote>/*
* 2016.01.13 ChinaBUG
* 根据GID获取全部规格，并按照规格获取全部的状态
*/
function ajax_get_spec_all( $goods_id ){
$this-&gt;_response-&gt;set_header('Cache-Control', 'no-store, no-cache');
$f_goods = 'goods_id,spec_desc';
$aGoods = app::get('b2c')-&gt;model('goods')-&gt;getrow( $f_goods, array('goods_id'=&gt;$goods_id) );
$f_pro = 'product_id,spec_desc,store,freez,marketable';
$products = app::get('b2c')-&gt;model('products')-&gt;getList( $f_pro, array('goods_id'=&gt;$aGoods['goods_id']) );

$spec_param = $_POST['p']?$_POST['p']:$_GET['p'];
$spec_check = false;//设定假设录入的规格数据都是错误的

// 多规格信息
foreach($aGoods['spec_desc'] as $specId=&gt;$specValue){
$arrSpecId['spec_id'][] = $specId;
}
$arrSpecName = app::get('b2c')-&gt;model('specification')-&gt;getList('spec_name,spec_id,spec_type',$arrSpecId,0,-1,'p_order ASC');
foreach($arrSpecName as $specItem){
$goodsSpec['specification']['spec_name'][$specItem['spec_id']] = $specItem['spec_name'];
$goodsSpec['specification']['spec_type'][$specItem['spec_id']] = $specItem['spec_type'];
}
//
$aGoods['product']['spec_desc']['spec_private_value_id'] = $spec_param;//在这边输入我们当前的规格的唯一编码
$tmp_goods_spec = $this-&gt;_get_goods_spec_two( $aGoods );
// 检查输入的规格值是否存在，不存在则需要显示全部的规格值
// 分两步
// 1.现在规格内检查规格的编号是否存在
// 2.检查规格的值是否存在
foreach( $tmp_goods_spec['goods'] as $gskey=&gt;$gsVal ){
if( isset($spec_param[ $gskey ]) &amp;&amp; in_array($spec_param[ $gskey ], array_keys($gsVal)) ){
$spec_check = true;
}
foreach($gsVal as $gsKey2=&gt;$gsVal2){
$JSON['goods'][ $gskey ][ $gsKey2 ] = array(
'product_id' =&gt; $gsVal2['product_id']?$gsVal2['product_id']:0,
'marketable' =&gt; $gsVal2['marketable']?$gsVal2['marketable']:0,
'store'      =&gt; $gsVal2['store']?$gsVal2['store']:0,
);
}
}
//
$JSON['isexist'] = $goodsSpec['specification']['isexist'] = !$spec_check; // 如果true 则需要在视图那边显示全部
echo json_encode($JSON);
exit;
}</blockquote>
然后，我们就需要在前台视图上做一下改造了，请先打开/app/b2c/view/site/product/info/spec.html文件，然后改为如下：
<blockquote>&lt;div id="product_spec" class="product-spec"&gt;
&lt;ul class="spec-area"&gt;
&lt;{foreach from=$page_product_basic.spec.goods key=key item=spec}&gt;
&lt;li class="spec-item"&gt;
&lt;span class="item-label"&gt;&lt;i&gt;&lt;{$page_product_basic.spec.specification.spec_name.$key}&gt;&lt;/i&gt;：&lt;/span&gt;
&lt;span class="item-content"&gt;
&lt;ul class="clearfix" <strong>data-sp-id="&lt;{$key}&gt;"</strong>&gt;
&lt;{foreach from=$spec key=item_key item=item}&gt;
&lt;{if $item_key == $page_product_basic.spec.product.$key || $item.product_id}&gt;
&lt;li class="spec-attr&lt;{if $item_key==$page_product_basic.spec.product.$key}&gt; selected&lt;{else}&gt;<strong>&lt;{if $item.store &lt;1}&gt; ecs-not-store&lt;{/if}&gt;</strong>&lt;{/if}&gt;" <strong>date-spi-id="&lt;{$item_key}&gt;"</strong>&gt;
<strong>&lt;{if $item_key!=$page_product_basic.spec.product.$key &amp;&amp; $item.store &lt;1}&gt;&lt;div class="product-not-allowed"&gt;&lt;/div&gt;&lt;{/if}&gt;</strong>
&lt;{if $item_key==$page_product_basic.spec.product.$key}&gt;
&lt;a href="javascript:void(0);"&gt;
&lt;{elseif $item.product_id}&gt;
&lt;a href="&lt;{link app=b2c ctl=site_product act=index arg0=$item.product_id}&gt;" rel="&lt;{$item.product_id}&gt;"&gt;
&lt;{/if}&gt;
&lt;{if $page_product_basic.spec.specification.spec_type.$key == 'image'}&gt;
&lt;img src="&lt;{$item.spec_image|storager:'s'}&gt;" alt="&lt;{$item.spec_value}&gt;" title="&lt;{$item.spec_value}&gt;"&gt;
&lt;{else}&gt;
&lt;span&gt;&lt;{$item.spec_value}&gt;&lt;/span&gt;
&lt;{/if}&gt;
&lt;i&gt;&lt;/i&gt;
&lt;/a&gt;
&lt;/li&gt;
&lt;{/if}&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;/span&gt;
&lt;/li&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;/div&gt;

更新2016.04.14 修改之前程序出现的错误，不能无刷新载入，还有就是不能显示全部的规格并按照组合自动隐藏

**************************

&lt;div id="product_spec" class="product-spec v3"&gt;
&lt;ul class="spec-area"&gt;
&lt;{foreach from=$page_product_basic.spec.goods key=key item=spec}&gt;
&lt;li class="spec-item"&gt;
&lt;span class="item-label"&gt;&lt;i&gt;&lt;{$page_product_basic.spec.specification.spec_name.$key}&gt;&lt;/i&gt;：&lt;/span&gt;
&lt;span class="item-content"&gt;
&lt;ul class="clearfix" data-sp-id="&lt;{$key}&gt;"&gt;
&lt;{foreach from=$spec key=item_key item=item}&gt;
&lt;{* &amp;&amp; ($item.store&gt;0)*}&gt;
&lt;{*if ($item_key == $page_product_basic.spec.product.$key || $item.product_id)*}&gt;
&lt;li class="spec-attr&lt;{if $item_key==$page_product_basic.spec.product.$key}&gt; selected&lt;{else}&gt;&lt;{if $item.store &lt;1}&gt; ecs-not-store&lt;{/if}&gt;&lt;{/if}&gt;" date-spi-id="&lt;{$item_key}&gt;" id="sp_&lt;{$key}&gt;_&lt;{$item_key}&gt;"&gt;
&lt;{if $item_key!=$page_product_basic.spec.product.$key &amp;&amp; $item.store &lt;1}&gt;&lt;div class="product-not-allowed"&gt;&lt;/div&gt;&lt;{/if}&gt;
&lt;{if $item_key==$page_product_basic.spec.product.$key}&gt;
&lt;a href="javascript:void(0);" data-select="true" rel="&lt;{$page_product_basic.product_id}&gt;"&gt;
&lt;{elseif $item.product_id}&gt;
&lt;a href="&lt;{if $item.store &gt;0}&gt;&lt;{link app=b2c ctl=site_product act=index arg0=$item.product_id}&gt;&lt;{else}&gt;javascript:void(0);&lt;{/if}&gt;" rel="&lt;{$item.product_id}&gt;" data-store="&lt;{$item.store|default:0}&gt;"&gt;
&lt;{else}&gt;
&lt;a href="javascript:void(0);" rel="&lt;{$item.product_id}&gt;"&gt;
&lt;{/if}&gt;
&lt;{if $page_product_basic.spec.specification.spec_type.$key == 'image'}&gt;
&lt;img src="&lt;{$item.spec_image|storager:'s'}&gt;" alt="&lt;{$item.spec_value}&gt;" title="&lt;{$item.spec_value}&gt;"&gt;&lt;span&gt;&lt;{$item.spec_value}&gt;&lt;/span&gt;
&lt;{else}&gt;
&lt;span&gt;&lt;{$item.spec_value}&gt;&lt;/span&gt;
&lt;{/if}&gt;
&lt;i&gt;&lt;/i&gt;
&lt;/a&gt;
&lt;div class="pre-qr" style="width:0px;height:0px;overflow: hidden;"&gt;
&lt;div style="float:left;width:89px;height:89px;"&gt;
&lt;!--&lt;img style="width:132px;height:132px;" src="&lt;{link app=b2c ctl=site_product act=erweima arg0=$item.product_id?$item.product_id:$page_product_basic.product_id arg1=$page_product_basic.goods_id arg2=1}&gt;"/&gt;--&gt;
&lt;img style="width:89px;height:89px;" src="&lt;{link app=b2c ctl=site_qrcode2 act=erweima arg0=$item.product_id?$item.product_id:$page_product_basic.product_id arg1=$page_product_basic.goods_id arg2=1}&gt;"/&gt;
&lt;/div&gt;
&lt;div style="float:right;padding-top: 40px;"&gt;扫一扫&lt;br/&gt;直接通过二维码下单&lt;/div&gt;
&lt;/div&gt;
&lt;/li&gt;
&lt;{*/if*}&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;/span&gt;
&lt;/li&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;/div&gt;

**************************</blockquote>
改造视图文件主要是为了让我们在JS中调用我们需要的数据并按照我们预设的结构输入输出数据。

下面请打开/app/b2c/view/site/product/index.html文件，然后找到    'click:relay(.spec-attr)': function(e){ 所在位置，下面增加代码如下：
<blockquote>        // 限制的规格是不允许操作的
if(this.hasClass('ecs-not-store')){
e.preventDefault();
return;
}
// 当前已经选择的，点击后取消选择，重新分配规格状态
if(this.hasClass('selected')){
this.removeClass('selected');
renderLayout();
return;
}
// 检查是否同一个规格多个值
var liObj = this.getParent('ul.clearfix').getChildren('li.selected');
if( !this.hasClass('selected') &amp;&amp; liObj.length&gt;0 ){
liObj.erase(this).removeClass('selected');
this.addClass('selected');
}
// 当当前规格没有选择的状态，点击当前规格商品重新选择则直接增加选择样式，不做其他处理
if( liObj.length==0 &amp;&amp; this.getChildren('a').get('href')=='javascript:void(0);' ) {
this.addClass('selected');
renderLayout();
return;
}
// 当前全部没有选择，点击一个之后
var ulObj = this.getParent('ul.spec-area').getChildren('li.selected');
if( ulObj.length == 0 ){
this.addClass('selected');
renderLayout();
return;
}

更新2016.04.14 修改之前程序出现的错误，不能无刷新载入，还有就是不能显示全部的规格并按照组合自动隐藏

**************************

// 限制的规格是不允许操作的
if(this.hasClass('ecs-not-store')){
e.preventDefault();
return;
}
// 当前已经选择的，点击后取消选择，重新分配规格状态
if(this.hasClass('selected')){
this.removeClass('selected');
renderLayout();
return;
}
// 检查是否同一个规格多个值
var liObj = this.getParent('ul.clearfix').getChildren('li.selected');
if( !this.hasClass('selected') &amp;&amp; liObj.length&gt;0 ){
liObj.erase(this).removeClass('selected');
this.addClass('selected');
// ??
//renderLayout();
//return;
}
// 当当前规格没有选择的状态，点击当前规格商品重新选择则直接增加选择样式，不做其他处理
if( liObj.length==0 &amp;&amp; this.getChildren('a').get('href')=='javascript:void(0);' ) {
this.addClass('selected');
renderLayout();
return;
}
// 当前全部没有选择，点击一个之后
var ulObj = this.getParent('ul.spec-area').getElements('li.selected');
if( ulObj.length == 0 ){
this.addClass('selected');
renderLayout();
return;
}

**************************
更新2016.04.22 修改之前程序出现的错误，如果选择一个规格的时候，库存0的都会被限制不肯选择，造成同一类的不可二次点击，还需要点击取消才可以选择其他规格值，这个处理之后就不需要多此一举了。

**************************

        // 限制的规格是不允许操作的
        if(this.hasClass('ecs-not-store')){
            e.preventDefault();
            return;
        }
        // 当前已经选择的，点击后取消选择，重新分配规格状态
        if(this.hasClass('selected')){
            this.removeClass('selected');
            renderLayout();
            return;
        }
        // 检查是否同一个规格多个值
        var liObj = this.getParent('ul.clearfix').getChildren('li.selected');
        if( !this.hasClass('selected') && liObj.length>0 ){
            liObj.erase(this).removeClass('selected');
            this.addClass('selected');
            // 2016.04.22
            renderLayout();
            console.log( '检查是否同一个规格多个值' );
            return;
        }
        // 当当前规格没有选择的状态，点击当前规格商品重新选择则直接增加选择样式，不做其他处理
        if( liObj.length==0 && this.getChildren('a').get('href')=='javascript:void(0);' ) {
            this.addClass('selected');
            renderLayout();
            return;
        }
        // 当前全部没有选择，点击一个之后
        var ulObj = this.getParent('ul.spec-area').getElements('li.selected');
        if( ulObj.length == 0 ){
            this.addClass('selected');
            renderLayout();
            return;
        }

**************************
</blockquote>
然后在上面随便找个地方放上renderLayout方法就好：
<blockquote>/* 2016.01.15 */
function renderLayout(){
var pData=Array(),myJson;
$$('#product_spec .spec-attr.selected').each(function(el,idx){
pData.push('p['+el.getParent('ul.clearfix').get('data-sp-id')+']=' + el.get('date-spi-id'));
});
selectNum = pData.length;
myJson = new Request.HTML(
{
url: '&lt;{link ctl=site_product act=ajax_get_spec_all arg0=$page_product_basic.goods_id}&gt;',
data:pData.join("&amp;"),
onSuccess:function(rspTree, rspEl, rspHTML, rspJS){
rspObj = JSON.decode(rspHTML);
if(rspObj['isexist']){
var obj = $('product_spec');
var obj2 = obj.getElements('.spec-attr.ecs-not-store .product-not-allowed');
if( obj2[0] != undefined ){
obj2[0].getParent('.ecs-not-store').removeClass('ecs-not-store');
obj2[0].destroy();
}
}
Object.each(rspObj['goods'],function(item,idx){
Object.each(item,function(subitem,subidx){
var href = 'javascript:void(0);';
var li = $( 'sp_'+idx+'_'+subidx );
if( li == null ) return;
li_a = li.getChildren('a')[0];
if(subitem.product_id&gt;0) href = '/product-'+subitem.product_id+'.html';
li_a.set('href',href).set('data-store',subitem.store);
if( (subitem.product_id==0 &amp;&amp; li.hasClass('ecs-not-store')) || (subitem.store&gt;0 &amp;&amp; subitem.product_id&gt;0) ){
li.removeClass('ecs-not-store');
if(li.getNext('product-not-allowed')!=null) li.getNext('product-not-allowed').destroy();
}else if( subitem.store==0 &amp;&amp; subitem.product_id&gt;0 ){
li.addClass('ecs-not-store');
(new Elements('div#product-not-allowed')).inject( li.getNext('a'), 'top');
}
});
});
if(selectNum!=specTotal){
$('product-action').getElements('.product-buy-quantity .p-store').set('html','');
}
}
}).post();
}

更新2016.04.14 修改之前程序出现的错误，不能无刷新载入，还有就是不能显示全部的规格并按照组合自动隐藏

**************************

// 2016.04.14 ChinaBUG
function renderLayout(){
var pData=Array(),myJson;
$$('#product_spec .spec-attr.selected').each(function(el,idx){
pData.push('p['+el.getParent('ul.clearfix').get('data-sp-id')+']=' + el.get('date-spi-id'));
});
selectNum = pData.length;
// console.log( 'pData : ' + pData.join("&amp;"));
// console.log( 'pData LENGTH : ' + selectNum);
myJson = new Request.HTML(
{
url: '&lt;{link ctl=site_product act=ajax_get_spec_all arg0=$page_product_basic.goods_id}&gt;',
data:pData.join("&amp;"),
onSuccess:function(rspTree, rspEl, rspHTML, rspJS){
rspObj = JSON.decode(rspHTML);
// console.log('START---'+rspObj['isexist']);
if(rspObj['isexist']){
var obj = $('product_spec');
// var obj2 = obj.getElements('.spec-attr .product-not-allowed');
var obj2 = obj.getElements('.spec-attr');
// console.log( '.spec-attr : ' + obj2.length);
obj2.each(function(ele,index){
// console.log(ele+ ' - ' +index);
ele.removeClass('ecs-not-store');
// console.log( 'getChildren : ' +  ele.getChildren('a').length);
ele.getChildren('a').set('href','javascript:void(0);');

});
return true;
}
//流程：
// 1.先设置链接
// 2.如果没有库存则检查是否有.product-not-allowed，没有增加
// 3.有库存则删除.product-not-allowed
Object.each(rspObj['goods'],function(item,idx){
Object.each(item,function(subitem,subidx){
var href = 'javascript:void(0);',rel='';
// console.log('当前操作的ID : '+'sp_'+idx+'_'+subidx);
var li = $( 'sp_'+idx+'_'+subidx );
// console.log( li );
if( (li == null) || (typeof(li)=='undefined') ) return;
li_a = li.getChildren('a')[0];
if( (li_a == null) || (typeof(li_a)=='undefined') ) return;
// 有库存且不是当前规格值则赋予新的商品链接
if(subitem.product_id&gt;0) {
href = '/product-'+subitem.product_id+'.html';
rel = subitem.product_id;
}else{
href = 'javascript:void(0);';
rel = '';
}
li_a.set('href',href).set('rel',rel).set('data-store',subitem.store);
// 没有商品编码的处理，默认直接开启
console.log('&gt;&gt;'+subitem);
if( li.hasClass('selected') ){
li.removeClass('ecs-not-store');
}else if( (subitem.product_id==0 &amp;&amp; subitem.marketable==0 &amp;&amp; subitem.store==0) || (subitem.store==0 &amp;&amp; subitem.product_id&gt;0)  ){//(subitem.store==0) &amp;&amp;
li.addClass('ecs-not-store');
(new Elements('div#product-not-allowed')).inject( li.getNext('a'), 'top');
}else if( (subitem.product_id==0 &amp;&amp; li.hasClass('ecs-not-store')) || (subitem.store&gt;0 &amp;&amp; subitem.product_id&gt;0) || (subitem.product_id&gt;0 &amp;&amp; subitem.marketable=="true") ){
li.removeClass('ecs-not-store');
if(li.getNext('product-not-allowed')!=null) li.getNext('product-not-allowed').destroy();
}
// (subitem.product_id&gt;0) ||
//
console.log( idx + " - " + subidx + " - " + subitem.product_id + " - " + subitem.store );
});
});
// 根据实际设置库存:规格选择不完整则库存值清空
if(selectNum!=specTotal){
$('product-action').getElements('.product-buy-quantity .p-store').set('html','');
}
// console.log('END');
}
}).post();
}

**************************
更新2016.04.22 修改之前程序出现的错误，如果选择一个规格的时候，库存0的都会被限制不肯选择，造成同一类的不可二次点击，还需要点击取消才可以选择其他规格值，这个处理之后就不需要多此一举了。

**************************

/* 2016.04.22 */
function renderLayout(){
    var pData=Array(),myJson;
    $$('#product_spec .spec-attr.selected').each(function(el,idx){
        pData.push('p['+el.getParent('ul.clearfix').get('data-sp-id')+']=' + el.get('date-spi-id'));
    });
    selectNum = pData.length;
    // console.log( 'pData : ' + pData.join("&"));
    // console.log( 'pData LENGTH : ' + selectNum);
    myJson = new Request.HTML(
        {
        url: '<{link ctl=site_product act=ajax_get_spec_all arg0=$page_product_basic.goods_id}>',
        data:pData.join("&"),
        onSuccess:function(rspTree, rspEl, rspHTML, rspJS){
            rspObj = JSON.decode(rspHTML);
            // console.log('START---'+rspObj['isexist']);
            if(rspObj['isexist']){
                var obj = $('product_spec');
                // var obj2 = obj.getElements('.spec-attr .product-not-allowed');
                var obj2 = obj.getElements('.spec-attr');
                // console.log( '.spec-attr : ' + obj2.length);
                obj2.each(function(ele,index){
                    // console.log(ele+ ' - ' +index);
                    ele.removeClass('ecs-not-store');
                    // console.log( 'getChildren : ' +  ele.getChildren('a').length);
                    ele.getChildren('a').set('href','javascript:void(0);');
                    
                });
                return true;
            }
            //流程：
            // 1.先设置链接
            // 2.如果没有库存则检查是否有.product-not-allowed，没有增加
            // 3.有库存则删除.product-not-allowed
            Object.each(rspObj['goods'],function(item,idx){
                Object.each(item,function(subitem,subidx){
                    var href = 'javascript:void(0);',rel='';
                    // console.log('当前操作的ID : '+'sp_'+idx+'_'+subidx);
                    var li = $( 'sp_'+idx+'_'+subidx );
                    // console.log( li );
                    if( (li == null) || (typeof(li)=='undefined') ) return;
                        li_a = li.getChildren('a')[0];
                    if( (li_a == null) || (typeof(li_a)=='undefined') ) return;
                    // 有库存且不是当前规格值则赋予新的商品链接
                    if(subitem.product_id>0) {
                        href = '/product-'+subitem.product_id+'.html';
                        rel = subitem.product_id;
                    }else{
                        href = 'javascript:void(0);';
                        rel = '';
                    }
                    li_a.set('href',href).set('rel',rel).set('data-store',subitem.store);
                    // 没有商品编码的处理，默认直接开启
                    console.log('>>'+subitem);
                    if( li.hasClass('selected') ){
                        li.removeClass('ecs-not-store');
                    }else if( (subitem.product_id==0 && subitem.marketable==0 && subitem.store==0) || (subitem.store==0 && subitem.product_id>0)  ){//(subitem.store==0) &&
                        li.addClass('ecs-not-store');
                        (new Elements('div#product-not-allowed')).inject( li.getNext('a'), 'top');
                    }else if( (subitem.product_id==0 && li.hasClass('ecs-not-store')) || (subitem.store>0 && subitem.product_id>0) || (subitem.product_id>0 && subitem.marketable=="true") ){
                        li.removeClass('ecs-not-store');
                        if(li.getNext('product-not-allowed')!=null) li.getNext('product-not-allowed').destroy();
                    }
                    // (subitem.product_id>0) || 
                    //
                    console.log( idx + " - " + subidx + " - " + subitem.product_id + " - " + subitem.store );
                });
            });
            // 2016.04.22 ChinaBUG 检查规格中，如果只有一个规格有选择的话，则，有选择的哪个规格的其他不能选择的项目都解锁
            var selects = $$('.spec-area .spec-item li.selected');
            if(  selects.length==1  ){
              selects[0].getParent(".clearfix").getChildren("li").removeClass("ecs-not-store");
            }
            // 根据实际设置库存:规格选择不完整则库存值清空
            if(selectNum!=specTotal){
                $('product-action').getElements('.product-buy-quantity .p-store').set('html','');
            }
            // console.log('END');
        }
    }).post();
}
**************************</blockquote>
保存，收工，测试一下，就会发现问题解决了~不过这个修改会造成单页的重新载入，原先的载入是感觉不到刷新的，现在这个修改一下就会发现，单页应用的代码被忽略了，代码位置就是下面的：

备注：2016.04.14 这个问题在后面的版本已经更正。

<blockquote>if (window.history &amp;&amp; history.pushState) {...}</blockquote>
需要做一些处理，时间有限就不处理了，大家有空自己处理，处理好就通知我呗。感谢~

注：写完这个修改，才发现自己对脚本的掌控能力很差，年后再来好好重新学习一下吧，有优化的朋友可以给一份否？^_^