---
ID: 1641
post_title: >
  密码保护：ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)
author: ChinaBUG
post_excerpt: |
  ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)
  大家在做二次开发时，会发现，你新建了cct.product.php后，ShopEX系统还是依旧运行ctl.product.php里面的index方法，cct里面的index方法就是不运行。
  不信，请将下面的代码保存为cct.product.php放进正确的文件夹，测试一下即可。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-ctl-product-php-2-cct-product-php.html
published: true
post_date: 2011-07-15 20:33:51
---
ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)

大家在做二次开发时，会发现，你新建了cct.product.php后，ShopEX系统还是依旧运行ctl.product.php里面的index方法，cct里面的index方法就是不运行。

不信，请将下面的代码保存为cct.product.php放进正确的文件夹，测试一下即可。

&lt;?php
require_once CORE_DIR."/shop/controller/ctl.product.php";
class cct_product extends ctl_product{
function index($gid,$specImg='',$spec_id='') {
print_r($_COOKIE);
exit;
}
}
?&gt;

结果：

正常的情况应该是会输出整个COOKIES的数组对吧，但是，这个代码运行之后我们会发现，运行正常，并没有中断。

难道这个文件没有运行？我们在这个基础上增加一段代码，把上面的代码修改为：

&lt;?php
require_once CORE_DIR."/shop/controller/ctl.product.php";
class cct_product extends ctl_product{
<span style="color: #ff0000;">    function cct_product(){</span>
<span style="color: #ff0000;">        print_r($COOKIE);</span>
<span style="color: #ff0000;">        exit;</span>
<span style="color: #ff0000;">    }</span>
function index($gid,$specImg='',$spec_id='') {
print_r($_COOKIE);
exit;
}
}
?&gt;

注意红色部分，这个就是这个类最先运行的方法，我们在这边输出COOKIE的值，运行之后，很神奇，工作正常，你会看到COOKIE的数组值。

怎么会这样呢？

暂时不管为什么会这样子呢~现在我们肯定能想到是既然这个方法可以运行，那我就在这边调用index方法不就成功？

所以，我们就把上面的代码修改为下面的样子哈

&lt;?php
require_once CORE_DIR."/shop/controller/ctl.product.php";
class cct_product extends ctl_product{
<span style="color: #ff0000;">    function cct_product(){</span>
<span style="color: #ff0000;">        $this-&gt;index($gid,$specImg,$spec_id);</span>
<span style="color: #ff0000;">    }</span>
function index($gid,$specImg='',$spec_id='') {
print_r($_COOKIE);
exit;
}
}
?&gt;

满怀喜悦的运行，却发现提示错误，排查之后你会发现$gid这个参数是没有值得！与ctl.product.php调用的对比，$gid没有传值进来。（经过调试，这个函数就只有第一个参数有值。）

没有值传入，那我们只要在cct_product()方法调用之前，传入这个$gid的值就行了，你肯定这样思考的，对吧，现在关键是$gid这个值是怎么来的？你知道吗？！

不知道吧，这个值涉及到底层的代码了~封装起来了。

怎么得到这个值，请查看本博客的 <a title="ShopEX-内核程序研究-如何在方法之间传递参数的？" href="http://blog.ipodmp.com/archives/shopex-kernel-research-how-to-pass-parameters-to-the-method/">ShopEX-内核程序研究-如何在方法之间传递参数的？</a>你就知道来龙去脉了~

现在我直接告诉你怎么调用这个参数哈~

我们只要把cct.product.php的代码修改为下面的代码，你就能让他正常工作了！

&lt;?php
require_once CORE_DIR."/shop/controller/ctl.product.php";
class cct_product extends ctl_product{
function cct_product(){
parent::ctl_product();
$this-&gt;title    = $this-&gt;system-&gt;getConf('site.goods_title');
$this-&gt;keywords = $this-&gt;system-&gt;getConf('site.goods_meta_key_words');
$this-&gt;desc     = $this-&gt;system-&gt;getConf('site.goods_meta_desc');
<span style="color: #ff0000;">    $this-&gt;system = $GLOBALS['system'];</span>
<span style="color: #ff0000;">     $gid = $this-&gt;system-&gt;request['action']['args'][0];</span>
<span style="color: #ff0000;">     $this-&gt;index($gid);</span>
}
function index($gid,$specImg='',$spec_id='') {
print_r($_COOKIE);
exit;
ctl_product::index($gid);
}
}
?&gt;

请注意红色部分，这个是关键的地方！整个文件的最精华的就是这个方法了噢。

试试看吧，正常运行了噢，现在开始你的二次开发吧~