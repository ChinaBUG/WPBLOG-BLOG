---
ID: 2538
post_title: >
  ShopEX-smtp发邮件没办法发送错误提示Failed
  to connect to
  server(fsockopen发邮件错误编码10061解决方案)
author: ChinaBUG
post_excerpt: |
  昨天不知道怎么回事发现ShopEX的后台发送不了邮件了，邮箱设置是使用SMTP发送邮件，试了好几个账号都不行，老是出现Failed to connect to server的错误提示，而且还是乱码噢。但是本地操作却可以，目标指向服务器，那是什么问题噢？
  查看mdl.smtp.php发现这个提示信息是在调用fsockopen方法之后如果返回空则提示错误。
  根据经验方法返回false或者空值可能的问题有：1.fsockopen方法被限制 2.方法调用之后返回值真的为空。
  先判断方法fsockopen是不是真的被限制，检查配置文件发现这个是开启的，没有限制，那么是什么问题呢？
  于是写了一个测试的代码哈，代码大部分是直接拷贝至mdl.smtp.php内的，代码如下：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/08/shopex-smtp-mail-can-not-send-an-error-message-failed-to-connect-to-server-fsockopen-mail-error-code-10061-solution.html
published: true
post_date: 2013-08-21 10:49:08
---
昨天不知道怎么回事发现ShopEX的后台发送不了邮件了，邮箱设置是使用SMTP发送邮件，试了好几个账号都不行，老是出现Failed to connect to server的错误提示，而且还是乱码噢。但是本地操作却可以，目标指向服务器，那是什么问题噢？

查看mdl.smtp.php发现这个提示信息是在调用fsockopen方法之后如果返回空则提示错误。

根据经验方法返回false或者空值可能的问题有：1.fsockopen方法被限制 2.方法调用之后返回值真的为空。

先判断方法fsockopen是不是真的被限制，检查配置文件发现这个是开启的，没有限制，那么是什么问题呢？

于是写了一个测试的代码哈，代码大部分是直接拷贝至mdl.smtp.php内的，代码如下：

&lt;?php
$smtp_conn = fsockopen('smtp.qq.com',    # the host of the server
'25',    # the port to use
$errno,   # error number if any
$errstr,  # error message if any
'30');   # give up after ? secs
if(empty($smtp_conn)) {
$error = array("error" =&gt; "Failed to connect to server",
"errno" =&gt; $errno,
"errstr" =&gt; $errstr);
echo '失败啦....'.$errno;
}else echo 'OK................';
?&gt;

然后就会发现错误码是10061，这个代码在shopex错误时没有显示呢？不知道为什么噢。。。

然后google了一下，发现一篇文章，正好说明如何解决这个问题的，按照操作发现，问题解决噢。

解决方法摘抄如下：

用fsockopen连接25端口，提示“由于目标机器积极拒绝,无法连接。(10061)”。百度N久，才发现是该死的McAfee，去掉”<span style="text-decoration: underline;">访问保护</span>“里的”<span style="text-decoration: underline;">防病毒标准保护</span>“=》”<span style="text-decoration: underline;">禁止群发邮件蠕虫发送邮件</span>“前的阻止的勾去掉就好了。。

原文链接：<a href="http://www.phpec.org/php/fsockopen.html">fsockopen,由于目标机器积极拒绝,无法连接。(10061)的解决方案</a>