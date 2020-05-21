---
ID: 4357
post_title: >
  ECStore二次开发日记之14.API接口直连实例之添加/更新购物车没库存出错
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2016/02/api-interface-to-connect-directly-to-the-example-of-the-add-update-cart-no-inventory-error.html
published: true
post_date: 2016-02-22 19:27:06
---
新年好~年后又开工了~

去年跟益跑网的合作项目还没完成，今年继续。

项目涉及到API的调用，当时官方的那边的文档只有部分，返回值什么的都没有说明，有的接口还没有开放，实在是无语呀。

自己受累一点了，把一些常用的接口简单封装一下吧，方便调用，顺便文档注释一下，方便未来的合作跟即将，可能做得APP开发的调用，那个可是真的需要这个接口的。

磨刀不负砍材工~

~

在测试购物车模块的时候，调用添加商品进入购物车接口b2c.member.add_cart，调用参数是商品没有库存的情景下，提示居然是“配件验证失败！”好奇怪噢，这个是可预期的错误，但是错误原因却是错误的。

分析进入到/app/b2c/lib/cart/object/goods.php这个文件内的check_store方法，我们可以看到错误提示就在里面，但是在上面的代码中有检查库存的，居然跳过去了，很显然，经检测错误就是在_check_products_with_add这个方法内。

然后一步步的跟进去，发现在

$arr_product['store'] = ( $aGoods['nostore_sell'] ? $this-&gt;__max_goods_store : ( empty($arr_product['store']) ? ($arr_product['store']===0 ? 0 : $this-&gt;__max_goods_store) : $arr_product['store'] -$arr_product['freez']) );

这边出现了逻辑错误了！

重点问题是：如果商品允许无库存购买，值1，不允许，值0

所以，这个代码改为：

$arr_product['store'] = ( $aGoods['nostore_sell']==1 ? $this-&gt;__max_goods_store : ( empty($arr_product['store']) ? ($arr_product['store']===0 ? 0 : $this-&gt;__max_goods_store) : $arr_product['store'] -$arr_product['freez']) );

就应该是正确的，结果发现居然还是错误的，为什么呢？

经过断点发现问题就出现在

$arr_product['store']===0

这个地方，经过调试输出我们知道了这个的值就是0呢，难道这个不对？

好吧，测试一下呗，测试下面表达式的值

$arr_product['store']===0

输出居然是空，也就是这个值是false为啥呀？！

好吧，再使用

<span class="methodname"><strong>gettype($arr_product['store'])</strong></span>

输出的是“string”类型的，哎，没办法，代码修改为：

$arr_product['store'] = ( $aGoods['nostore_sell']==1 ? $this-&gt;__max_goods_store : ( empty($arr_product['store']) ? ($arr_product['store']===<strong>'0'</strong> ? 0 : $this-&gt;__max_goods_store) : $arr_product['store'] -$arr_product['freez']) );

保存，测试，好了，终于提示“该商品已无库存！”

同样的问题还出现在更新购物车商品数量的时候，如果购买的数量很大的话，则没有任何提醒，返回成功true哎。

搜索_check($aData, $_check_adjunct=true)方法所在，然后找到上面相关的地方，修改一下就好了。

$arr_product['store'] = ( $aGoods['nostore_sell']==1 ? $this-&gt;__max_goods_store : ( empty($arr_product['store']) ? ($arr_product['store']==='0' ? 0 : $this-&gt;__max_goods_store) : $arr_product['store'] -$arr_product['freez']) );

保存，测试，额~~怎么还是错误的信息呢？！

然后又从头看了一下代码，才发现：

/app/b2c/lib/apiv/apis/response/member/cart.php

里面的代码

$_flag = $mCartObject-&gt;update_object($type,$obj_ident,$arr_object );
$return['status'] = $obj_ident ? 'true' : 'false';

好吧，代码写错了~这个永远是true，只要参数指定了的话，改为下面代码：

$_flag = $mCartObject-&gt;update_object($type,$obj_ident,$arr_object );
// 2016.02.23 ChinaBUG fix 变量错误
// $return['status'] = $obj_ident ? 'true' : 'false';
$return['status'] = $_flag['status']==='true' ? 'true' : 'false';
$return['message'] = $_flag['msg'];
<blockquote>注：至于为什么update_object返回的是数组，这个需要查看源码跟踪之后你会发现是数据形式，要不，还真的不知道怎么处理，一开始我也是简单修改一下，最后发现不行。</blockquote>
保存，测试，结果还是不是预期的，一定有问题！

又再次从头跟进，然后在/app/b2c/lib/cart/object/goods.php里面发现问题所在了，请找到：

if( true!==($return=$this-&gt;_check($aSave,$_check_adjunct)) ) {
if( $append )
return $return;
else return false;
}

我们经过跟踪可以知道_check方法返回的都是一个数组，不可能是布尔值，所以这个逻辑一定是执行的，然后里面的逻辑就悲剧了，没有将_check方法的错误信息反馈给上级调用的方法，然后......

修改一下吧

// 2016.02.23 ChinaBUG fix 逻辑错误
// _check返回数组，逻辑永远是成立的
// if( true!==($return=$this-&gt;_check($aSave,$_check_adjunct)) ) {
// if( $append )
// return $return;
// else return false;
// }
$return = $this-&gt;_check($aSave,$_check_adjunct);
if( 'false'===$return['status'] ) {
return $return;
}
return array('status'=&gt;$this-&gt;oCartObjects-&gt;save($aSave),'msg'=&gt;'');

保存，测试，同样测试通过，提示错误信息“购买数量超出库存”

这个地方的逻辑有点复杂，最好是自己一步步的跟进，才知道里面的逻辑跟返回值。

~.~END~.~