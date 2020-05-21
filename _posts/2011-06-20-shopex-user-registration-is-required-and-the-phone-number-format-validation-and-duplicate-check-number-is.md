---
ID: 1376
post_title: >
  密码保护：ShopEX-用户注册-手机号注册为必填项且验证格式并检查号码是否重复(唯一性验证)
author: ChinaBUG
post_excerpt: |
  今天在官方论坛看到有人在问，时间老久，都没人回答，在这边说一下怎么修改吧，记录一下哈
  原提问在这边  shopex  注册的时候手机号必填，怎么弄的？
  要设置注册时手机号码为必填项，需要修改三个地方，就是注册的界面视图，还有就是注册的功能函数，脚本的验证功能，其实，你只要简单的必填项，那么只需要修改视图文件即可。反正不用进行二次验证就是了。
  我们这次修改的目标是：注册时，手机号码为必填项，而且手机号码需要能验证格式，并且能与数据库验证是否存在相同号码。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-user-registration-is-required-and-the-phone-number-format-validation-and-duplicate-check-number-is.html
published: true
post_date: 2011-06-20 12:50:52
---
今天在官方论坛看到有人在问，时间老久，都没人回答，在这边说一下怎么修改吧，记录一下哈

原提问在这边  <a href="http://bbs.shopex.cn/read.php?tid-173163.html">shopex  注册的时候手机号必填，怎么弄的？</a>

要设置注册时手机号码为必填项，需要修改三个地方，就是注册的界面视图，还有就是注册的功能函数，脚本的验证功能，其实，你只要简单的必填项，那么只需要修改视图文件即可。反正不用进行二次验证就是了。

我们这次修改的目标是：<span style="text-decoration: underline;">注册时，手机号码为必填项，而且手机号码需要能验证格式，并且能与数据库验证是否存在相同号码。</span>

具体视图文件如下：

<span style="text-decoration: underline;"><em>core/shop/view/passport/index/signup_fast.html</em></span>

<em><span style="text-decoration: underline;">core/shop/view/passport/index/signup.html</span></em>

这两个文件，一个是完整页面的代码，一个是窗口时的代码，里面的代码都一样。<del>需要提醒的是，在窗口模式下，有些功能不能使用。</del>

具体修改代码如下，<span style="text-decoration: underline;">随便打开一个视图文件，做完一份直接拷贝相关的代码到另一份中即可</span>：

我们找到确认密码项，在下面填写手机项吧。

&lt;tr&gt;
&lt;th&gt;&lt;i&gt;*&lt;/i&gt;&lt;{t}&gt;确认密码：&lt;{/t}&gt;&lt;/th&gt;
&lt;td&gt;&lt;{input name="passwd_r" type="password" required="true" id="reg_passwd_r"}&gt;&lt;/td&gt;
&lt;/tr&gt;

添加上下面这段代码：
&lt;tr&gt;
&lt;th&gt;&lt;i&gt;*&lt;/i&gt;&lt;{t}&gt;移动手机：&lt;{/t}&gt;&lt;/th&gt;
&lt;td&gt;&lt;input type="text" <strong><span style="color: #ff0000;">vtype="mobile"</span></strong> required="true" name="mobile" id="reg_mobile" <span style="color: #ff0000;"><strong>onchange="mobileCheck(this)"</strong></span>/&gt;&lt;span&gt;&lt;/span&gt;
&lt;/td&gt;
&lt;/tr&gt;

这样第一步完成。

<del>步骤说明：</del>vtype这个项主要是脚本的验证，我们指定是mobile类型的，但是源文件里面是没有的，所以我们需要增加。onchange事件是当我们输入完成，执行检查函数，这个功能是用JSON与服务器交互，检查数据库内的手机是否存在并二次判断手机号码的准确性，防止注入提交出错滴。

在最后面找到：

function nameCheck(input){
new Request.HTML({update:$(input).getNext(),data:'name='+encodeURIComponent(input.value=input.value.trim())}).post('&lt;{link ctl=passport act=namecheck}&gt;');
}

添加下面的手机验证：
function mobileCheck(input){
new Request.HTML({update:$(input).getNext(),data:'mobile='+encodeURIComponent(input.value=input.value.trim())}).post('&lt;{link ctl=passport act=mobilecheck}&gt;');
}

然后再打开<span style="text-decoration: underline;">statics\script_src\formplus.js</span>里面的文件，找到

'email':['请录入正确的Email地址:abc@abc.com',function(element,v){
return v==null || v=='' || /(\S)+[@]{1}(\S)+[.]{1}(\w)+/.test(v);
}],

然后在，后面添加如下代码

'mobile':['请录入正确的移动号码:1XXXXXXXXXX',function(element,v){
return v==null || v=='' || /^(13[0-9]|15[0-9]|18[0-9])\d{8}$/.test(v);
}],

记得那个，号噢，要不出错滴。把这个文件拷贝到<span style="text-decoration: underline;">statics\script</span>里面覆盖源文件。

这是第二步。提醒的是，如果你是只想手机号码必填，不想进行服务端的验证跟数据库是否重复的话，那么第二步红色部分跟上面的检测函数都可去掉，不需要修改，下面的第三步也就不需要弄了。

下面是第三步，服务端的验证跟手机号码的重复判断。我们来打开core/shop/controller/ctl.passport.php文件，找到如下地方：

function namecheck(){
$member=&amp;$this-&gt;system-&gt;loadModel('member/account');
$name = trim($_POST['name']);
if($member-&gt;check_uname($name,$message)){
echo '&lt;span&gt;&amp;nbsp;'.__('可以使用').'&lt;/span&gt;';
}else{
echo '&lt;span&gt;&amp;nbsp;'.$message.'&lt;/span&gt;';
}
}

在后面我们添加两个函数哈，代码如下：

//注册时手机是否存在
function mobilecheck(){
$mobile = trim($_POST['mobile']);
if(!preg_match('/^(13[0-9]|15[0-9]|18[0-9])\d{8}$/',$mobile)){
echo '&lt;span&gt;&amp;nbsp;请输入正确的手机号码.&lt;/span&gt;';
}else{
if($this-&gt;check_mobile($_POST['mobile'],$message)==false){
echo '&lt;span&gt;&amp;nbsp;'.$message.'&lt;/span&gt;';
}else{
echo '&lt;span&gt;&amp;nbsp;'.__('可以使用').'&lt;/span&gt;';
}
}
}

//检查移动号码的存在与否
function check_mobile( $mobile, &amp;$message )
{
//引用数据库操作类~~为什么要引用？请看另外一篇基础部分 <a href="http://blog.ipodmp.com/archives/shopex-secondary-development-plug-in-templates-of-diary/">ShopEX-二次应用开发/二次插件/二次模板研究日记</a>
$db = $this-&gt;system-&gt;database();
if(!preg_match('/^(13[0-9]|15[0-9]|18[0-9])\d{8}$/',$mobile)){
$message = __( "请输入正确的手机号码." );
return false;
}
$row = $db-&gt;selectrow( "select mobile from sdb_members where mobile ='".$mobile."'" );
if ( strlen($row['mobile'])&gt;0 )
{
$message = __( "该手机号码已经注册!" );
return false;
}else{
$message = __( "手机号码".$mobile."可以注册!" );
return true;
}
}

好了，现在前台的手机验证跟号码是否重复的检查就这样子完成了。

如果严禁一点的话，请找到下面所在：

}elseif($passwdLen&gt;20){
$this-&gt;splash('failed','back',__('密码长度不能大于20'),'','',$_POST['from_minipassport']);
}

在后面添加下面的验证代码：
//手机号码验证是否符合格式
if(!preg_match('/^(13[0-9]|15[0-9]|18[0-9])\d{8}$/',$_POST['mobile'])){
$this-&gt;splash('failed','back',__('请输入正确的手机号码'),'','',$_POST['from_minipassport']);
}
//检测手机号是否存在
//$chkmobil = &amp;$this-&gt;system-&gt;loadModel('member/member');
if($this-&gt;check_mobile($_POST['mobile'],$message)==false){
$sError = __('该手机号码已经注册!谢谢配合.');
$this-&gt;splash('failed','back',$sError,'','',$_POST['from_minipassport']);
}

好了。完美结局问题啦。

赶紧去尝试一下吧。