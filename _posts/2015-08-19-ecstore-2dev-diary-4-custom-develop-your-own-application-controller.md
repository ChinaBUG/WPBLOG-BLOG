---
ID: 4154
post_title: >
  ECStore二次开发日记之4.定制开发自己的应用控制器
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-4-custom-develop-your-own-application-controller.html
published: true
post_date: 2015-08-19 17:26:34
---
在上一篇《<a href="http://blog.ipodmp.com/archives/ecstore-development-diary-3-qr-code-application-mouse-moves-on-the-specification-shows-the-qr-code-scan-code-order/">ECStore二次开发日记之3.二维码应用：鼠标移到多规格上就显示二维码，扫码下单</a>》博文中，我们的二维码生成方法是放在product.php这个控制器内的，放在里面是可以，但是有时候会因为缓存原因，出现问题很不方便调试，而且为了便于管理，我们希望关于二维码的应用都放在一起，这个时候就需要我们自己来写一个控制器了。

其实，做二次开发入门很简单，简单到你只要懂得复制、粘帖就可以了。

现在请跟我来复制粘帖吧。

我们进入目录：app &gt; b2c &gt; controller &gt; site

然后右键拖动product.php文件到下面空白地方，放开鼠标，在弹出的菜单里面选择“复制到当前位置(C)”或者直接复制文件，粘帖，然后会自动新建一个副本文件。

我们将文件名改为qrcode.php，这个文件是我希望的控制器名称qrcode，要记得不要随便乱取也不要非法字符，正常一点都是可以的。

现在我们打开qrcode.php这个文件，然后我们会看到开头的如下代码：
<pre>&lt;?php
/**
 * ShopEx licence
 *
 * @copyright  Copyright (c) 2005-2010 ShopEx Technologies Inc. (http://www.shopex.cn)
 * @license  http://ecos.shopex.cn/ ShopEx License
 */

class b2c_ctl_site_product extends b2c_frontpage{</pre>
这个时候我们需要将product改为我们需要的qrcode了，现在代码变成如下：
<pre>&lt;?php
/**
 * ShopEx licence
 *
 * @copyright  Copyright (c) 2005-2010 ShopEx Technologies Inc. (http://www.shopex.cn)
 * @license  http://ecos.shopex.cn/ ShopEx License
 */

class b2c_ctl_site_qrcode extends b2c_frontpage{
      ......
}
</pre>
然后，请将上一讲开发日记的erweima方法整个拷贝到这个里面，变成如下：
<pre>&lt;?php
/**
 * ShopEx licence
 *
 * @copyright  Copyright (c) 2005-2010 ShopEx Technologies Inc. (http://www.shopex.cn)
 * @license  http://ecos.shopex.cn/ ShopEx License
 */

class b2c_ctl_site_qrcode extends b2c_frontpage{

	function __construct($app){
		parent::__construct($app);        
	}</pre>
/*
2015.07.22 ChinaBUG 二维码生成
用于前台商品详情的商品页面的二维码显示。
*/
function erweima( $pid = 29408,$gid = 0,$addToCart = 0,$debug = 0 )
{
header("Cache-Control:no-store, no-cache, must-revalidate"); // HTTP/1.1
header("Expires: Mon, 26 Jul 1997 05:00:00 GMT");// 强制查询etag
header('Progma: no-cache');
//
$url = array(
'app'=&gt;'b2c',
'ctl'=&gt;'wap_product',
'full'=&gt;1,
'arg0'=&gt;$pid,
);
$url = app::get('wap')-&gt;router()-&gt;gen_url( $url );
// 自动加入购物车
// $addToCart = false;
if( $addToCart ) $url .= '?qr=1';
// 缓存KEY
$cacheKey = 'qrcode_'.$pid.'_'.$addToCart;
if( !cachemgr::get( $cacheKey,$content ) )
{
cachemgr::co_start();
//二维码纠错级别
$errorCorrectionLevel = 'L'; //L M Q H
//二维码生成图片大小 1-10
$matrixPointSize = 4;
ob_start();
weixin_qrcode_QRcode::png( $url, false, $errorCorrectionLevel, $matrixPointSize, 2);
$content = ob_get_contents();
//ob_clean();
ob_end_clean();
cachemgr::set( $cacheKey, $content, cachemgr::co_end() );
}
//
if($debug == 0){
header("Content-type: image/png");
echo $content;
}else{
print_r(array(
'pid'=&gt;$pid,
'gid'=&gt;$gid,
'addToCart'=&gt;$addToCart,
'cacheKey'=&gt;$cacheKey,
'cachemgr::get(errorCorrectionLevel:L)'=&gt;$errorCorrectionLevel,
'content'=&gt;$content,
'preview'=&gt;'&lt;div style="height:132px;width:132px;background-image:url('data:image/png;base64,'.base64_encode($content).'')"&gt;&lt;/div&gt;'
));
}
}
}

保存，然后，你是不是急冲冲的准备去执行了呢？

你会发现，执行不了，提示找不到这个文件，是不是很奇怪呢~？

其实不奇怪，ECStore使用新的架构，所以，如果你没有配置的话，他是不会给你执行的。我们还需要配置一下他才会执行噢。

请打开app &gt; b2c &gt; site.xml文件，然后在最下面找到如下代码：
<pre>    &lt;module id='b2c' controller='site_paycenter' &gt;
        &lt;name&gt;paycenter&lt;/name&gt;
        &lt;title&gt;支付中心&lt;/title&gt;
        &lt;disable&gt;false&lt;/disable&gt;
    &lt;/module&gt;</pre>
复制一下，改为如下代码：
<pre>    &lt;module id='b2c' controller='site_paycenter' &gt;
        &lt;name&gt;paycenter&lt;/name&gt;
        &lt;title&gt;支付中心&lt;/title&gt;
        &lt;disable&gt;false&lt;/disable&gt;
    &lt;/module&gt;
    &lt;!--@2015.07.27 ChinaBUG--&gt;
    &lt;module id='b2c' controller='site_qrcode' &gt;
        &lt;name&gt;qrcode&lt;/name&gt;
        &lt;title&gt;二维码&lt;/title&gt;
        &lt;disable&gt;false&lt;/disable&gt;
    &lt;/module&gt;
    &lt;!--@2015.07.27 ChinaBUG--&gt;</pre>
然后保存，接下来是重点，因为我们改动了配置的文件，所以一定要刷新一下，让系统重新加载配置文件。

进入后台，点击右上方的“应用中心”然后点击“维护”或者“站点”，“模板列表”，“当前使用模板”，右下方有一个更多，点击里面的“维护”就可以了。

然后现在就可以检验我们的劳动成果的时候了。

直接在地址栏输入：http://locahost/qrcode-erweima-1.html就可以输出商品编号为1的商品二维码了。

当然，我在开发的时候我同时还有一个方法叫做getver的，代码如下：
<pre>    function getver( $date = null ){
        if($date == null){
            echo '2015.08.03.10.19';
        }else echo $date;
    }</pre>
那么我们的测试就可以是：http://locahost/qrcode-getver.html，然后就能看到输出时间了。

噢，完美收工，还不去试试看？~~^_^