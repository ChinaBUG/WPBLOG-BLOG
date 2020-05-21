---
ID: 4278
post_title: >
  ECStore-怎么检查SMTP发不了邮件问题
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/10/ecstore-how-to-check-smtp-cant-send-mail-problem.html
published: true
post_date: 2015-10-27 12:08:51
---
今天同事反应ECSTORE没办法发送邮件，不知道为什么，我用后台的“测试配置”功能提醒是：

DATA END command failed

然后，没有啥提示了，头痛。

根据提示感觉应该是命令执行错误，那么第一反应就是是不是这个SMTP要使用的fsockopen函数的问题，找了一下我博客的之前的一篇文章《<a href="http://blog.ipodmp.com/archives/shopex-smtp-mail-can-not-send-an-error-message-failed-to-connect-to-server-fsockopen-mail-error-code-10061-solution/">ShopEX-smtp发邮件没办法发送错误提示Failed to connect to server(fsockopen发邮件错误编码10061解决方案)</a>》把里面的代码保存文件测试，成功通过，那就不是这个问题了，再来测试一下发送的吧，网上找一个代码，一试还是可以的嘛，马上就出现错误提示了。

OK................DATA(end)error: 550 Connection frequency limited

然后查找一下，在腾讯家帮助中心中找到《<a href="http://service.mail.qq.com/cgi-bin/help?subtype=1&amp;id=20022&amp;no=1000722">550 Connection frequency limited</a>》，很好，原来是被限制了，问题解决。

下面是源码：

&lt;?php
$smtp_conn = fsockopen(
'smtp.qq.com',    # the host of the server
'25',    # the port to use
$errno,   # error number if any
$errstr,  # error message if any
'30');   # give up after ? secs
if(empty($smtp_conn)) {
$error = array(
"error" =&gt; "Failed to connect to server",
"errno" =&gt; $errno,
"errstr" =&gt; $errstr);
echo '失败啦....'.$errno;
}else echo 'OK................';

//

echo send_mail('abc@qq.com','发信测试','测试测试测试测试测试测试');

function send_mail($to, $subject = 'No subject', $body) {
$loc_host = "test";                 //发信计算机名，可随意
$smtp_acc = "abcd@qq.com";        //Smtp认证的用户名，类似fuweng@im286.com，或者fuweng
$smtp_pass="abcd";              //Smtp认证的密码，一般等同pop3密码
$smtp_host="smtp.qq.com";    //SMTP服务器地址，类似 smtp.tom.com
$from="abcd@qq.com";              //发信人Email地址，你的发信信箱地址
$headers = "Content-Type: text/plain; charset="gb2312"rnContent-Transfer-Encoding: base64";
$lb="rn";                         //linebreak

$hdr = explode($lb,$headers);     //解析后的hdr
if($body) {
$bdy = preg_replace("/^./","..",explode($lb,$body));   //解析后的Body
}

$smtp = array(
//1、EHLO，期待返回220或者250
array("EHLO ".$loc_host.$lb,"220,250","HELO error: "),
//2、发送Auth Login，期待返回334
array("AUTH LOGIN".$lb,"334","AUTH error:"),
//3、发送经过Base64编码的用户名，期待返回334
array(base64_encode($smtp_acc).$lb,"334","AUTHENTIFICATION error : "),
//4、发送经过Base64编码的密码，期待返回235
array(base64_encode($smtp_pass).$lb,"235","AUTHENTIFICATION error : ")
);
//5、发送Mail From，期待返回250
$smtp[] = array("MAIL FROM: &lt;".$from."&gt;".$lb,"250","MAIL FROM error: ");
//6、发送Rcpt To。期待返回250
$smtp[] = array("RCPT TO: &lt;".$to."&gt;".$lb,"250","RCPT TO error: ");
//7、发送DATA，期待返回354
$smtp[] = array("DATA".$lb,"354","DATA error: ");
//8.0、发送From
$smtp[] = array("From: ".$from.$lb,"","");
//8.2、发送To
$smtp[] = array("To: ".$to.$lb,"","");
//8.1、发送标题
$smtp[] = array("Subject: ".$subject.$lb,"","");
//8.3、发送其他Header内容
foreach($hdr as $h) {$smtp[] = array($h.$lb,"","");}
//8.4、发送一个空行，结束Header发送
$smtp[] = array($lb,"","");
//8.5、发送信件主体
if($bdy) {foreach($bdy as $b) {$smtp[] = array(base64_encode($b.$lb).$lb,"","");}}
//9、发送“.”表示信件结束，期待返回250
$smtp[] = array(".".$lb,"250","DATA(end)error: ");
//10、发送Quit，退出，期待返回221
$smtp[] = array("QUIT".$lb,"221","QUIT error: ");

//打开smtp服务器端口
$fp = @fsockopen($smtp_host, 25);
if (!$fp) echo "Error: Cannot conect to ".$smtp_host."";
while($result = @fgets($fp, 1024)){
if(substr($result,3,1) == " ") { break; }
}

$result_str="";
//发送smtp数组中的命令/数据
foreach($smtp as $req){
//发送信息
@fputs($fp, $req[0]);
//如果需要接收服务器返回信息，则
if($req[1]){
//接收信息
while($result = @fgets($fp, 1024)){
if(substr($result,3,1) == " ") { break; }
};
if (!strstr($req[1],substr($result,0,3))){
$result_str.=$req[2].$result."";
}
}
}
//关闭连接
@fclose($fp);
return $result_str;
}