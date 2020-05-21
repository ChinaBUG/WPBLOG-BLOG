---
ID: 2774
post_title: >
  ShopEX二次开发DIY日记10之怎么设置一人一个账号限制只能买一件商品数量
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  今天中午随便查看ShopEX官网论坛发现《[485/易开店]在哪可以设置 一人只能买一件？》然后你知道的，我们继续来DIY这个功能吧~
  思路分析：
  我们要对一个账号来限制他只能购买一个商品，而且数量只能是1的话，我们需要知道如下的信息：
  1、怎么判断会员是否登录（这边还要区分的是一个商品一次只能买一件还是购买总数只能一件）
  2、购物车的结构是什么样子的
  3、这边还要区分是在添加购物车时判断还是创建订单是判断，另外，商品如果是多规格的话，那么要不要按照多规格的来限制数量等等
  对比一下我们知道：
  1、会员是否登录这个跟需求有关系，如果商品一次只能购买一个的话，这个就没关系了，如果是购买总数只能一件，那么在做下面的判断前一定是要登录的。
  2、购物车的结构体，请看后面专门分析
  3、对于这个问题来说，如果我们是在添加购物车判断的话，那么根本不需要登录，如果是创建订单的话则需要登陆之后才能判断。
  总的来说，上面的分析都是屁话，结论就是：你一定要知道购物车的结构这个才是关键，那么我们来看看购物车的结构吧。
  购物车的结构是什么样子的呢？怎么才能知道呢？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-10-how-to-set-a-man-an-account-can-only-buy-a-limited-number-of-commodities.html
published: true
post_date: 2013-11-11 16:19:19
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

今天中午随便查看ShopEX官网论坛发现《<a href="http://bbs.shopex.cn/read.php?tid-312551.html">[485/易开店]在哪可以设置 一人只能买一件？</a>》然后你知道的，我们继续来DIY这个功能吧~

思路分析：

我们要对一个账号来限制他只能购买一个商品，而且数量只能是1的话，我们需要知道如下的信息：

1、怎么判断会员是否登录（这边还要区分的是一个商品一次只能买一件还是购买总数只能一件）

2、购物车的结构是什么样子的

3、这边还要区分是在添加购物车时判断还是创建订单是判断，另外，商品如果是多规格的话，那么要不要按照多规格的来限制数量等等

对比一下我们知道：

1、会员是否登录这个跟需求有关系，如果商品一次只能购买一个的话，这个就没关系了，如果是购买总数只能一件，那么在做下面的判断前一定是要登录的。

2、购物车的结构体，请看后面专门分析

3、对于这个问题来说，如果我们是在添加购物车判断的话，那么根本不需要登录，如果是创建订单的话则需要登陆之后才能判断。

总的来说，上面的分析都是屁话，结论就是：你一定要知道购物车的结构这个才是关键，那么我们来看看购物车的结构吧。

购物车的结构是什么样子的呢？怎么才能知道呢？

我一般都是直接输出的哈~

操作步骤如下：先添加几个商品，然后在core\shop\controller\ctl.cart.php里面找到function ctl_cart(&amp;$system){}这个方法，在最后面输入：
<blockquote>$this-&gt;gifts = $this-&gt;cart['f'];//这个是原先的代码
print_r($this-&gt;cart);
exit;</blockquote>
然后保存，点击进入购物车（我的访问地址是http://shopex326.ipodmp.com/?cart.html），然后你会看到如下代码：

Array ( [g] =&gt; Array ( [cart] =&gt; Array ( [152-394-na] =&gt; 1 [150-392-na] =&gt; 1 ) ) )

我们给他排版一下比较好看：
<pre>Array (
      [g] =&gt; Array (
            [cart] =&gt; Array (
                        [152-394-na] =&gt; 1
                        [150-392-na] =&gt; 1
                  )
      )
 )</pre>
从上面我们可以获得购物车的结构,这个结构其实是普通商品，不包含赠品什么的，具体看下面的），下面的结构说明图来自某开发手册噢：
<pre>Array(
    [g] =&gt; Array(//普通商品部分
            [cart] =&gt; Array(//购物明细
                    [1-169-161_0_1|161_1_1|162_1_1|] =&gt; 1 
                    //key值：&lt;商品ＩＤ&gt;-&lt;货品id&gt;- na^ [[[ &lt;配件1id&gt;_&lt;配件1栏位&gt;_&lt;配件1购买数&gt;|[ &lt;配件2id&gt;_&lt;配件2栏位&gt;_&lt;配件2购买数&gt;|]]…]
                    //value值：购买数量
                )
            [pmt] =&gt; Array(//相关商品的营销规则
                    [1] =&gt; 140//key:商品id；value:营销规则id
                )
        )
    [f] =&gt; Array(//赠品部分
            [149] =&gt; Array(//key:赠品id
                    [num] =&gt; 1//兑换量
                )
        )
    [p] =&gt; Array(//捆绑商品部分
            [35] =&gt; Array (//捆绑商品数量
                    [num] =&gt; 1
                )
        )
    [c] =&gt; Array(//应用的coupon部分
            [Bfff0868600038] =&gt; Array(//key:coupon编号
                    [pmt_id] =&gt; 109//营销规则id
                    [type] =&gt; order//相关订单/商品的营销规则&lt;order|goods&gt;
                )
        )
)</pre>
备注：该数据结构保存在sdb_member表内的addon字段内。

作为一个PHP开发者，你知道了购物车的结构你可以将需要的信息读出来嘛？

好了，看完购物车的结构，我们现在来写一下代码获取一下客户选择的商品的编号及数量吧。

在上面的PHP代码修改为如下：
<blockquote>
<pre>$this-&gt;gifts = $this-&gt;cart['f'];//这个是原先的代码
print_r($this-&gt;cart);
//循环分解购物车项目
foreach($this-&gt;cart['g']['cart'] as $ckey =&gt; $cval){
	$aTmp = explode( "-", $ckey);
	echo '商品ID:'.$aTmp[0].',货品ID:'.$aTmp[1].',购买数量:'.$cval.'&lt;br/&gt;';
}
exit;</pre>
</blockquote>
运行您将看到我们已经获得了，客户挑选的商品的ID跟货品ID还有购买的数量了。

作为一个程序员，灵活的必须的，举一反三也是一定要的，所以我们能当程序员哈，下面就来具体问题具体开发吧。

1、在<strong><span style="text-decoration: underline;">添加购物车时</span></strong>就判断一种商品购物数量不得大于1与判断同种商品不同规格的购物数量限制

思路分析：这边难点在于我们要找到添加购物车的控制所在。

文件所在：core\shop\controller\ctl.cart.php

涉及方法：function addGoodsToCart($gid=0, $pid=0, $stradj='', $pmtid=0, $num=1) {}

具体开发：

我们找到方法addGoodsToCart，然后我们查看这个方法的参数，我们能明显看到，他可以接受$gid，$pid，$num的值，然后在方法内部我们也能看到方法也接受$_POST['goods']的参数传递，不管怎样，我们都能确定$gid，$pid，$num的值，那么就可以利用上面得到的购物车项目来做限制了噢。

我们的目标是：只要客户添加商品，我就要判断添加的商品的数量是否超出限制。超出则不添加，没有超出则添加。

分析方法代码之后我们看到 $status = $this-&gt;objCart-&gt;addToCart('g', $aParams, $num);这一行，可以知道这一行就是添加商品到购物车内的主要方法，那么我们的判断就要在这个之前。好了，上代码了：

在 $status = $this-&gt;objCart-&gt;addToCart('g', $aParams, $num);一行的上方我们增加如下代码：
<blockquote>
<pre>//需要限制的商品的GID/PID，在实际运用中这个值一般都是程序自动获取的
$mylimitGID = array(152,151,150);
$mylimitPID = array(394,393,392);
foreach($this-&gt;cart['g']['cart'] as $ckey =&gt; $cval){
	$aTmp = explode( "-", $ckey);
	if(in_array($aTmp[0],$mylimitGID) &amp;&amp; $cval&gt;1){
		exit('商品超过限制');
	}
	if(in_array($aTmp[1],$mylimitPID) &amp;&amp; $cval&gt;1){
		exit('商品的该规格超过限制');
	}
}</pre>
</blockquote>
现在我们点击添加到购物车的时候，只要有商品存在购物车的话，那么就会提醒了。上面的两种方式可以一起使用也可以分开，但是建议最好不要同时使用了~要同时使用最好是制定好促销方式之后好好考虑清楚才可。防止出现相冲突的情况。

2、在结算的时候判断商品是否符合数量限制

思路分析：这边难点在于我们要找到购物车结算的控制所在。

文件所在：core\shop\controller\ctl.cart.php

涉及方法：function checkout($isfastbuy=0){}

具体开发：通过第一例的分析，你知道这个时候应该怎样了呢？看方法的实现逻辑！找到适当的注入点，加上上面的判断代码即可。那么还需要我帮你嘛？这个是课后作业噢。

3、同一种商品不管下几次单，总的购物数限制不能大于1

看了上面的例子，不知道你按照实际做了吗，没有的话就赶紧去~别在这边耽误自己的时间，现在来分析一下怎么做总的购物数限制噢。

正常我们的促销活动出来之后，客户会很聪明的想：你一次不让我买多，那我就多买几次呗，反正我时间多，看你能拿我怎样？

怎么办？堵住这个漏洞吧。

思路分析：

在前面的实例上我们知道了每次限制的数量，那么现在要求的是全部的购物数量，全部的购物数量怎么来？这个是个问题！我们很容易想到，如果这是个会员，那么他购买的该商品的数量我们是不是可以获取得到呢？肯定可以！

那么，从最上面我们分析的要实现这个需求必须要知道的3个信息来说，我们这边的需求能够做到嘛？有什么必须的强制性要求嘛？

如果这个客户没有登录的时候一直下订单，你可以判断什么？明显只能判断他当前添加的商品的数量！因为你不知道他是不是会员，更不知道他以前购买过多少的商品，这个时候你要限制总购买数，可以实现嘛？！

明显不能，所以如果要总购买数限制的话，最保险的地方应该是结算之后的流程之中做判断。如果你在添加购物车的时候做判断，那么记得在结算时还需要再做一次的判断，否则不准确。

PS：思考一下还有什么限制没？

说了判断的时机之后就是该怎么获取该会员的购物总数量了。

我们要明白会员的购物总数量那么我们需要对数据库有所了解，要不是获取不到自己需要的数据的噢。

我们先来看订单相关的数据表：sdb_orders、sdb_order_items根据查看分析我们知道sdb_orders存放的是订单的一些必须的信息，而sdb_order_items则是存放我们的订单的子项，另外还保存着订单的发货状况信息（商品时在发送途中还是在仓库，完全发货还是部分发货等）。

所以，很多小朋友就很快的写出了一个SQL语句来查询噢：

<code>SELECT * FROM sdb_order_items WHERE order_id IN(SELECT order_id FROM sdb_orders WHERE member_id=1)</code>

上面代码的意思是获取会员编号1的会员全部的订单的子项，那么会员ID怎么来呢？很简单的，请看下面科普内容。
<blockquote>二开科普：

在ShopEX中，凡是涉及到会员信息的操作都可以使用一个变量来访问（常用信息），这个变量的名字是$this-&gt;member
<pre>Array (
 [member_id] =&gt; 1
 [member_lv_id] =&gt; 2
 [email] =&gt; admin@ipodmp.com
 [uname] =&gt; admin
 [b_year] =&gt;
 [b_month] =&gt;
 [b_day] =&gt;
 [unreadmsg] =&gt; 4
 [cur] =&gt; CNY
 [lang] =&gt;
 [point] =&gt; 1000
 [experience] =&gt; 2000
)
其他相关的文章：<a title="ShopEX-二次开发-如何在开发时获取系统的一些值如用户名会员编号经验积分等(cookies)" href="http://blog.ipodmp.com/archives/shopex-2-dev-how-to-get-the-system-in-the-development-of-some-value-when-the-user-name-if-membership-numbers-etc/">ShopEX-二次开发-如何在开发时获取系统的一些值如用户名会员编号经验积分等(cookies)</a></pre>
</blockquote>
看完科普的信息后我们立即就知道，在这边我们需要使用的是$this-&gt;member['member_id']来获取登陆会员的用户ID噢，请注意，只有在订单相关的页面一定是有这个变量，其他有的页面是没有的，请在调用时注意检验！

那么现在的SQL语句可以修改为下面的形式：

$sql ="<code>SELECT * FROM sdb_order_items WHERE order_id IN(SELECT order_id FROM sdb_orders WHERE member_id={$this-&gt;member['member_id']})</code>";

PS：很多人会疑问为什么有花括号括住？这个是为了区分变量的，要不去掉的话，双引号有替换的效果，会出现不正确的替换形式。当然也可以使用字符串组合也是可以的。

然后，我们会发现，其实就只要<code>sdb_order_items</code>里面的几个字段而已，没必要全部获取，然后SQl又可以修改为下面的形式：

$sql ="<code>SELECT product_id as pid,nums FROM sdb_order_items WHERE order_id IN(SELECT order_id FROM sdb_orders WHERE member_id={$this-&gt;member['member_id']})</code>";

从上面我们看到我们获取到pid跟nums两个的值，那么gid呢？<code>sdb_order_items</code>表里面是没有的，那么怎么办？

这个时候需要区分一个情况，就是你希望做的是什么限制！如果你的商品全部都是单规格的话，那么都没有影响，但是如果你的商品有多规格的话，那么就需要区分了，针对不同规格做限制的话，就可以直接使用，如果是针对一个大的商品项的话，则需要获取gid了，这边就全部获取出来吧，需要的人自己决定怎么删减吧。SQL代码如下：

$sql4gid = "SELECT goods_id as gid  FROM sdb_products WHERE product_id= {$pid}";

准备工作好了，接着我们直接上代码吧，思路好讲，代码可不好分析呀~

我们在之前的代码出删除之前的代码换成如下代码：
<pre>//获取会员已购商品数@ChinaBUG 2013.11.11
$this-&gt;db = $this-&gt;system-&gt;database();
$sql ="SELECT product_id as pid,nums FROM sdb_order_items WHERE order_id IN(SELECT order_id FROM sdb_orders WHERE member_id={$this-&gt;member['member_id']})";
$orderList = $this-&gt;db-&gt;select($sql);
foreach($orderList as $k1=&gt;$v1){
    $sql4gid = "SELECT goods_id as gid  FROM sdb_products WHERE product_id= {$v1['pid']}";
    $gid = $this-&gt;db-&gt;selectrow($sql4gid);
    $smGIDs[$v1['pid']] = $smPIDs[$gid['gid']] = $v1['nums'];
}
//需要限制的商品的GID/PID，在实际运用中这个值一般都是程序自动获取的
$mylimitGID = array(152,151,150);
$mylimitPID = array(394,393,392);
//
foreach($this-&gt;cart['g']['cart'] as $ckey =&gt; $cval){
    $aTmp = explode( "-", $ckey);
    if(in_array($aTmp[0],$mylimitGID) &amp;&amp; ($cval+$smGIDs[$aTmp[0]])&gt;1){
        exit('商品超过限制');
    }
    if(in_array($aTmp[1],$mylimitPID) &amp;&amp; ($cval+$smPIDs[$aTmp[1]])&gt;1){
        exit('商品的该规格超过限制');
    }
}</pre>
保存，执行，哇咧完美了噢~~

PS：请按照自己的需求做修改，代码虽然没什么问题，但是架不住您的需求奇特。