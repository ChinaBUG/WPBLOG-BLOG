---
ID: 1304
post_title: >
  密码保护：ShopEX-二次开发之获取购物车(购物篮)的订单总金额/购物车合计/商品数量/价格/种类
author: ChinaBUG
post_excerpt: |
  今天接到老大交代，要为ShopEX添加 订单金额超过3000元的单子，不允许货到付款 的任务，我折腾了大半个下午，再次深切体会到，不开源，正版客户算啥。
  折腾半天终于自己搞定了这个问题。最主要的是需要获取订单的金额。下面就是最要的获取代码哈。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-for-the-total-amount-of-the-shopping-cart.html
published: true
post_date: 2011-06-13 16:11:02
---
今天接到老大交代，要为ShopEX添加 订单金额超过3000元的单子，不允许货到付款 的任务，我折腾了大半个下午，再次深切体会到，不开源，正版客户算啥。

折腾半天终于自己搞定了这个问题。最主要的是需要获取订单的金额。下面就是最要的过程修改与获取代码哈。

首先，我在cheeckout_base.html这个文件里面查看了整个的布局，找到能找到的，就是没找到配货方式的控制代码的入口，烦恼，后来问了客服技术，才知道这个就是那个shipping函数控制的，位于core/shop/controller/ctl.cart.php里面。然后又去看了checkout_shipping.html才找到需要修改的地方噢。

找到$s['pad']==0?$aShippings[$k]['has_cod'] = 0:$aShippings[$k]['has_cod'] = 1;这行，把下面的代码加上去，作用就是进行判断哈

//根据总金额来判断是否禁止 货到付款
if($trading['totalPrice'] &gt;3000){
//根据ID来增加判断，要禁止那个配货方式
if($s['dt_id']=="<strong><span style="text-decoration: underline;">9</span></strong>"){
$aShippings[$k]['disab'] = true;
}else{
$aShippings[$k]['disab'] = false;
}
}

然后再到checkout_shipping.html文件去，在$shippings的循环里面修改如下：

&lt;input type="radio" name="delivery[shipping_id]" id='shipping_&lt;{$shipping.dt_id}&gt;' value="&lt;{$shipping.dt_id}&gt;" onclick="Order.shippingChange(this,event)" has_cod="&lt;{$shipping.has_cod}&gt;" <span style="text-decoration: underline;"><strong>&lt;{if $shipping.disab}&gt;disabled="disabled"&lt;{/if}&gt;</strong></span> /&gt;

重点是加粗的地方噢。

然后，你结账时就会提示，只要你的金额超过3000元，那么指定ID的那个配货方式就会被禁止掉，这边我的该项ID为9。在需要时需要进行修改滴。

下面是完全版的获取购物车内的金额的代码，出了获取金额，还有其他的选项，自己调整哈。

//获取购物车的总金额
$sale = &amp;$this-&gt;system-&gt;loadModel('trading/sale');
$cart = &amp;$this-&gt;system-&gt;loadModel('trading/cart');
$this-&gt;cart = $cart-&gt;getCart('all');
$trad = $sale-&gt;getCartObject($this-&gt;cart,$GLOBALS['runtime']['member_lv'],true);
$this-&gt;pagedata['trading'] = $trad['totalPrice'];

在view的文件内弄个&lt;{$trading.totalPrice|cur}&gt;即可显示。