---
ID: 3758
post_title: >
  财付通-ShopNC商城即时到帐中介担保双接口的开发后续之网银直联功能开发
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/12/money-immediately-to-the-account-shopnc-mall-intermediary-guarantee-the-development-of-interfaces-the-following-online-banking-direct-feature-development.html
published: true
post_date: 2014-12-30 10:42:38
---
ShopNC商城即时到帐中介担保双接口开发完之后，老大看了老觉得缺少了啥，一对比ShopEX之后发现，ShopEX的财付通插件有网银直联功能，这个功能挺牛逼的，比较贴切，人性化......省略诸多赞美之词之后，该开工了。

如果有什么看不懂的地方请返回去查看上一篇文章《<a title="财付通-ShopNC商城即时到帐中介担保双接口的开发" href="http://blog.ipodmp.com/archives/money-immediately-to-the-account-shopnc-mall-mediation-guarantee-the-development-of-interfaces/">财付通-ShopNC商城即时到帐中介担保双接口的开发</a>》。

现在先来看看我们的手头上的资源吧，我们打开b2b.zip这个压缩包，很显眼的我们就能看到有一个文件夹叫做网银直联样式，然后点击进去，你会发现，里面都包含着你需要的东西了，比如网银直联实现方法.doc还有pay.html演示文档。

现在说一下怎么跟ShopNC集合在一起哈。

前期工作：
<ol>
	<li>需要将用到的图片资源放到ShopNC内，方便调用，我是将图片资源tenpayimages文件夹直接复制到ShopNC的资源文件夹内，路径如下：/resource/images/tenpayimages/</li>
	<li>复制pay.html里面的代码待用（第二个table标签整个内容复制噢），其实吧，如果你是直接抄的话是不需要这步的。</li>
</ol>
然后请打开\templates\default\home\cart_step2.php这个文件，大约82行处，找到如下代码：

[php]&lt;?php if(!empty($output['online_array'])) { ?&gt;
      &lt;!-- online begin --&gt;[/php]

整个修改为如下代码：

[php]      &lt;?php if(!empty($output['online_array'])) { ?&gt;
      &lt;!-- online begin --&gt;
      &lt;div class=&quot;tabs-panel&quot;&gt;
        &lt;ul class=&quot;cart-defray&quot;&gt;
        &lt;?php // 2014.12.16 ChinaBUG 增加财付通的网银直连功能   ?&gt;
          &lt;?php foreach($output['online_array'] as $val) { ?&gt;
          &lt;?php if($val['payment_code'] != 'tenpay'){?&gt;
          &lt;li&gt;
            &lt;label class=&quot;radio&quot;&gt;
              &lt;input id=&quot;payment_&lt;?php echo $val['payment_code']; ?&gt;&quot; type=&quot;radio&quot; name=&quot;payment_id&quot; value=&quot;&lt;?php echo $val['payment_id']; ?&gt;&quot; extendattr=&quot;&lt;?php echo $val['payment_code']; ?&gt;&quot;/&gt;
            &lt;/label&gt;
            &lt;span class=&quot;logo&quot;&gt;&lt;img src=&quot;api/payment/&lt;?php echo $val['payment_code']; ?&gt;/logo.gif&quot; alt=&quot;&lt;?php echo $val['payment_name']; ?&gt;-&lt;?php echo $val['payment_info']; ?&gt;&quot; title=&quot;&lt;?php echo $val['payment_name']; ?&gt;-&lt;?php echo $val['payment_info']; ?&gt;&quot; onload=&quot;javascript:DrawImage(this,125,50);&quot; /&gt;&lt;/span&gt;
            &lt;dl class=&quot;explain&quot;&gt;
              &lt;dt&gt;&lt;/dt&gt;
              &lt;dd&gt;&lt;?php echo $val['payment_info']; ?&gt;&lt;/dd&gt;
            &lt;/dl&gt;
          &lt;/li&gt;
          &lt;?php }else{ ?&gt;
          &lt;li&gt;
            &lt;label class=&quot;radio&quot;&gt;
              &lt;input id=&quot;payment_&lt;?php echo $val['payment_code']; ?&gt;&quot; type=&quot;radio&quot; name=&quot;payment_id&quot; value=&quot;&lt;?php echo $val['payment_id']; ?&gt;&quot; extendattr=&quot;&lt;?php echo $val['payment_code']; ?&gt;&quot;/&gt;
            &lt;/label&gt;
            &lt;!--&lt;span class=&quot;logo&quot;&gt;&lt;img src=&quot;api/payment/&lt;?php echo $val['payment_code']; ?&gt;/logo.gif&quot; alt=&quot;&lt;?php echo $val['payment_name']; ?&gt;-&lt;?php echo $val['payment_info']; ?&gt;&quot; title=&quot;&lt;?php echo $val['payment_name']; ?&gt;-&lt;?php echo $val['payment_info']; ?&gt;&quot; onload=&quot;javascript:DrawImage(this,125,50);&quot; /&gt;&lt;/span&gt;--&gt;
            &lt;dl class=&quot;explain&quot;&gt;
              &lt;dt&gt;&lt;/dt&gt;
              &lt;dd&gt;
              &lt;!----&gt;
&lt;table style=&quot;FONT-SIZE: 12px&quot; cellspacing=&quot;1&quot; cellpadding=&quot;3&quot; width=&quot;762&quot; align=&quot;center&quot; bgcolor=&quot;#D8D9DB&quot; border=&quot;0&quot;&gt;
&lt;tr&gt;
    &lt;td width=&quot;754&quot; height=&quot;36&quot; style=&quot;background:#F1F3F2&quot;&gt;
        &amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/logo.gif&quot;/&gt;为您提供以下网上支付服务&lt;span style=&quot; color:#868686&quot;&gt;（财付通是腾讯旗下第三方支付平台）&lt;/span&gt;
    &lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
    &lt;td height=&quot;36&quot; style=&quot;background:#FFFFFF; padding-left:20px &quot; id=&quot;payment_tenpay_btype&quot;&gt;
        &lt;br/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1002&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/10.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1001&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/17.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1003&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/2.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1005&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/1.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1004&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/12.gif&quot;/&gt;
        &lt;br/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1008&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/8.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1009&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/27.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1032&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/11.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1022&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/3.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1006&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/4.gif&quot;/&gt;
        &lt;br/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1021&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/7.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1027&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/6.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1010&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/18.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1052&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/16.gif&quot;/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;1020&quot; style=&quot;vertical-align:top&quot;/&gt;&amp;nbsp;&lt;img src=&quot;/resource/images/tenpayimages/5.gif&quot;/&gt;
        &lt;br/&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;8001&quot;/&gt;&amp;nbsp;&amp;nbsp;财付通账户支付&lt;span style=&quot; color:#868686&quot;&gt;（财付通账户余额支付，一点通支付）&lt;/span&gt;
        &lt;input name=&quot;bank_type&quot; type=&quot;radio&quot; value=&quot;0&quot; style=&quot;display:none;&quot;/&gt;
    &lt;/td&gt;
&lt;/tr&gt;
&lt;/table&gt;
              &lt;!----&gt;
              &lt;/dd&gt;
            &lt;/dl&gt;
          &lt;/li&gt;
          &lt;?php } ?&gt;
          &lt;?php } ?&gt;
        &lt;/ul&gt;
      &lt;/div&gt;
      &lt;!-- online end --&gt;[/php]

&nbsp;

&nbsp;

保存一下，然后界面方面的就完成了噢。你可以看到很牛逼的一个界面了。

不过要是你点击的时候你会发现，呵呵支付方式前面的那个单选框没有办法选中，也就是说，如果你点击完网银的时候那个支付方式ID的单选框是没办法自动选择的，如果你提交的话，系统就会给你一个大大的白眼的，怎么办呢？

其实很简单，我们来写一个jQuery脚本，让系统自动选择呗。

[code lang="javascript"]$(function() {
    //2014.12.16 ChinaBUG 财付通增加网银直连
    var payment_tenpay = $('#payment_tenpay');
    $('#payment_tenpay_btype').children(&quot;input&quot;).each(function(index,ele){
        $(ele).click(function(){payment_tenpay.attr({checked:true});});
    });
    //
}); [/code]

然后保存，浏览，你会发现点击任何一个网银都会自动选中支付方式的单选框了，系统再也不用担心找不到支付方式了。

这样就好了吗？NO....

我们还需要在把网银的编码传给支付端口噢。

请找到\api\payment\tenpay\tenpay.php文件并打开，然后查找到下面的代码处：

[php]//银行类型，默认为财付通
$reqHandler-&gt;setParameter(&quot;bank_type&quot;, &quot;DEFAULT&quot;);[/php]

然后改为下面的代码哈

[php] //2014.12.16 ChinaBUG 财付通增加网银直连
        //银行类型，默认为财付通
        if(isset($_POST['bank_type'])){
            $reqHandler-&gt;setParameter(&quot;bank_type&quot;, intval($_POST['bank_type']));
        }else $reqHandler-&gt;setParameter(&quot;bank_type&quot;, &quot;DEFAULT&quot;);[/php]

&nbsp;

保存，然后就彻底完工了，随便选择一个网银提交之后就会自动转向相应的网银界面处理了。

后记

财付通的客服的相应速度实在不行，不是我在嫌弃，真的不行！就那个错误提示出现，我反馈之后，我就再也懒得问了，因为不懂得找谁！

我按照提示打电话给客服，客户很认真客气的跟我沟通了一下，然后要求我把这个提示！截图给他们的QQ客服人员处理，然后我加QQ（88881535）了，那个QQ死活是加不了的，不知道为啥，而且最关键是查找是找不到的，不过没事，我们还是有办法的嘛，终于能说上话了，结果TMD就一直提示不是上班时间什么的，好吧，我明明是上班时间好吗！

其实我一开始发现1535的加不了，电话客服给我另外一个号，那个号更加坑爹！QQ号是88881560，这个号不是人工号！既然不是人工号MD还给我！！我发消息就是那个几句，只能选择那些业务类型。。。

悲剧呀~其实吧，相比支付宝，财付通的服务就有点不够看了，这个就不展开了。

写这个文章的时候我再次测试了一下，88881535终于是可以查找到。然后我呢还是留言，然后他还是一如既往的给我一个大白眼的提示：您好，客服暂时离开，马上回来，请稍候。

我等，我等，有消息了会来更新的。