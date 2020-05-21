---
ID: 3728
post_title: >
  财付通-ShopNC商城即时到帐中介担保双接口的开发
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/money-immediately-to-the-account-shopnc-mall-mediation-guarantee-the-development-of-interfaces.html
published: true
post_date: 2014-11-28 17:53:51
---
最近，公司的开发由ShopEX系统改为ShopNC系统，避免不了需要对一些东西做二次修改，但是，但是，没想到，ShopNC的财付通支付方式居然是旧的接口，而且还用不了，都没听说别人用不了，为啥我们却不行呢？

人品？运气？

这些都不要紧，要紧的是我们又有活儿可以干了~

首先，测试过程中的几个问题先解答一下，然后再进入开发教程。

常见问题解答：
<ol>
	<li>签名验证错误（大概是这个意思，原话懒得恢复现场了）：
在ShopNC都是使用utf-8编码的，所以在传值的时候都是以utf-8编码传输的，在你保证！保证你的代码没有错误的情况下，请检查传递的input_charset参数的值，看看值是不是utf-8，或者没有指定，如果没有指定请添加上去吧，值为utf-8即可。
或许你会跟我想的一样，这个值不是非必填的项吗？为什么还要指定值呢？有这个疑问的人跟我一样就是没有看说明，文档说明这个值默认值是gbk的，你传入的是utf-8值，只能是错误的。
PS：2015.01.26 今天查看文档又看到这个说明，有问题的童靴可以尝试看看。
<blockquote>8、  如果你的是铁通，电信的网络，在提交支付请求时报“验证签名失败”的错误，请把sp_create_ip字段的值.修改为%2E，签名时还是按.，这样可以解决问题</blockquote>
</li>
	<li>您正在进行的交易存在较大风险，为了您的资金安全，您暂时不能支付此次交易，您可以拨打财付通热线0755-86013860咨询详情。
一开始，我还以为真的是我的账号没有权限。想打电话去问，结果手贱复制支付链接直接打开，却发现，根本没有这个提示，完全正常支付！然后我就知道了，不是账号限制什么的，仅仅是我的打开方式不对，不支持没有来源的直接转向。弄个页面自动转向就可以了。
备注:这个只能是处理小金额的问题，如果你按照上面做了，还是不行，那么就一定就是需要打电话去咨询的。</li>
</ol>
接口开发教程正式开始

开发准备：
<ol>
	<li>去财付通的网站下载SDK开发包：
<a href="http://mch.tenpay.com/market/index.shtml">http://mch.tenpay.com/market/index.shtml</a>
PS：在最下面 或者 即时到账开发文档下载</li>
	<li>下载ShopNC2.4源码：
<a href="http://pan.baidu.com/s/1gd1i1Dx">http://pan.baidu.com/s/1gd1i1Dx</a></li>
	<li>安装ShopNC2.4然后搭建好本地服务器噢</li>
	<li>解压下载的b2c.zip文件。</li>
</ol>
现在，请打开ShopNC目录，进入api\payment\tenpay（下面简称为A）将里面的class整个目录删除，然后进入b2c文件夹内的B2C\财付通即时到帐中介担保双接口\测试例子(可直接运行,测试前需先在tenpay_config文件上配置您的商户号，商家密钥，回调地址)\php(utf8)（下面简称B），将里面的classes整个拷贝到A目录里面。

请打开目录A与B里面的tenpay.php文件，将B目录的tenpay.php文件内的代码从下面开头位置到结尾位置复制：

[php]
/* 创建支付请求对象 */
......
echo &quot;&lt;br/&gt;&quot; . $debugInfo .&quot;&lt;br/&gt;&quot;;
[/php]

打开A目录的tenpay.php文件，找到下面代码处：

[php]
/* 商品名称 */
$desc = $transaction_id;
......
return $reqUrl;
[/php]

将这开头跟结尾之间的代码删除，替换成上面复制的代码，然后修改引入的class文件，再将一些没用的参数剔除，整理一下代码，完整代码如下：

[php]
require_once (&quot;classes/RequestHandler.class.php&quot;);
$bargainor_id = $this-&gt;payment['payment_config']['tenpay_account'];
$key = $this-&gt;payment['payment_config']['tenpay_key'];
/* 返回处理地址 */
$return_url = SiteUrl.&quot;/api/payment/tenpay/return_url.php&quot;;
$strDate = date(&quot;Ymd&quot;);
$strTime = date(&quot;His&quot;);
//4位随机数
$randNum = rand(1000, 9999);
//10位序列号,可以自行调整。
$strReq = $strTime . $randNum;
/* 商家订单号,长度若超过32位，取前32位。财付通只记录商家订单号，不保证唯一。 */
$out_trade_no = $sp_billno = $this-&gt;order['order_sn'];
/* 财付通交易单号，规则为：10位商户号+8位时间（YYYYmmdd)+10位流水号 */
$transaction_id = $bargainor_id . $strDate . $strReq;
/* 商品价格（包含运费），以分为单位 */
$total_fee = floatval($this-&gt;order['order_amount'])*100;
/* 商品名称 */
$desc = $transaction_id;
/* 创建支付请求对象 */
$reqHandler = new RequestHandler();
$reqHandler-&gt;init();
$reqHandler-&gt;setKey($key);
$reqHandler-&gt;setGateUrl(&quot;https://gw.tenpay.com/gateway/pay.htm&quot;);
//设置支付参数
$reqHandler-&gt;setParameter(&quot;partner&quot;, $bargainor_id);
$reqHandler-&gt;setParameter(&quot;out_trade_no&quot;, $out_trade_no);
$reqHandler-&gt;setParameter(&quot;total_fee&quot;, $total_fee);
//总金额
$reqHandler-&gt;setParameter(&quot;return_url&quot;, $return_url);
$reqHandler-&gt;setParameter(&quot;notify_url&quot;, $return_url);
$reqHandler-&gt;setParameter(&quot;body&quot;, $desc);
//银行类型，默认为财付通
$reqHandler-&gt;setParameter(&quot;bank_type&quot;, &quot;DEFAULT&quot;);
//用户ip
//客户端IP
$reqHandler-&gt;setParameter(&quot;spbill_create_ip&quot;, $_SERVER['REMOTE_ADDR']);
//币种
$reqHandler-&gt;setParameter(&quot;fee_type&quot;, &quot;1&quot;);
//商品名称，（中介交易时必填）
$reqHandler-&gt;setParameter(&quot;subject&quot;,$desc);
//字符集
$reqHandler-&gt;setParameter(&quot;input_charset&quot;, &quot;utf-8&quot;);
//请求的URL
$reqUrl = $reqHandler-&gt;getRequestURL();
return $reqUrl;
[/php]

OK这个修改完毕了~然后，提交之后就会出现最前面说的，那个错误提示2了。

如果你修改了，发现直接可以用了，那么恭喜你了，不行的话，出现错误提示跟我一样的话，那么请接下来做修改吧。

请打开control\payment.php文件，大约156行，下面代码处：

[php]
@header(&quot;Location:&quot;.$payment_api-&gt;get_payurl());
[/php]

把这行的代码换成下面的代码：

[php]
//@header(&quot;Location:&quot;.$payment_api-&gt;get_payurl());
//2014.11.28 ChinaBUG
Tpl::output('url',$payment_api-&gt;get_payurl());
Tpl::showpage('payment.index.cft','null_layout');
exit;
[/php]

保存，然后新建一个php文件，名字叫做payment.index.cft.php保存在\templates\default\home\中即可，完整路径是：\templates\default\home\payment.index.cft.php记得不要弄错位置噢。

payment.index.cft.php文件的内容就一行：

[code lang="html"]
&lt;meta http-equiv=&quot;refresh&quot; content=&quot;0;url=&lt;?php echo $output['url'];?&gt;&quot;&gt;
[/code]

然后，执行支付吧，问题解决。

神清气爽，过个好周末吧。

附录

前段时间周末过得还算舒服，可惜这两星期就不好了，因为财付通支付方式出现问题了，一直到今天终于找到原因了。

原因其实很简单，就是财付通支付之后返回验证错误，没办法设置支付状态。

整个一步步分析下来，出现的问题就是在判断腾讯签名上。

在方法return_verify()里面的$resHandler-&gt;isTenpaySign()方法，一直会告诉你验证失败。

其实就是多了两个参数，删除就可以了。