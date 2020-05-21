---
ID: 1290
post_title: >
  密码保护：ShopEX-二次应用开发/二次插件/二次模板研究日记
author: ChinaBUG
post_excerpt: |
  1、网站基本地址
  2、一些常用的变量/函数
  3、在ctl.xxxx.php文件内要调用本文件内的函数要怎么调用？
  4、在cct.xxxxx.php文件中怎么访问数据库
  5、表 sdb_member_attr 的作用
  6、父类方法/函数怎么调用哈
  7、如何转向/导向另外一个页面
  8、如何在shopex之中显示提示信息呢？在调试时很有用滴
  9、要是找不到输出变量怎么办？尝试一下输出变量吧，搞不好就成
  10、数组要如何显示出来？
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-secondary-development-plug-in-templates-of-diary.html
published: true
post_date: 2011-06-11 16:01:17
---
1、网站基本地址

"http://".$_SERVER['SERVER_NAME'."/" === $this-&gt;system-&gt;base_url()

2、一些常用的变量/函数

exit;//退出函数,if等

$this-&gt;member['member_id'];//会员的编号

$this-&gt;system-&gt;error(404);//错误提示：对不起，无法找到您访问的页面，请返回重新访问。

$_COOKIE['MEMBER'];//登陆的值，格式为$memberID."-".utf8_encode( $row['uname'] )."-".md5( $row['password'].STORE_KEY )."-".time( );

$_SESSION["RANDOM_CODE"]//登陆验证码的值(admin)

ob_start();//php ob_start 是 php 的缓冲输出函数。具体请参阅 <a href="http://www.nowamagic.net/php/php_ObStart.php" target="_blank">ob_start() 函数介绍</a>

3、在ctl.xxxx.php文件内要调用本文件内的函数要怎么调用？

一文件内有函数A跟check_mobile函数，要在A里卖弄调用需要像下面这样写，要不运行不了滴，靠，还是基础不行哈

$this-&gt;check_mobile();

4、在cct.xxxxx.php文件中怎么访问数据库

一般在cct.xxxx.php文件中不能直接操作数据库，要操作数据库需要到cmd.xxxxxx.php里面操作，那么非要在cct中操作呢？需要引用操作类，请看下面哈

  //引用数据库操作类
 $db = $this-&gt;system-&gt;database(); 
 $row = $db-&gt;selectrow( "......" );

5、表 sdb_member_attr 的作用

这个表是保存注册项资料的，我们可以通过admin\会员\会员注册项来管理。这个表现出来的是，在会员中心时个人信息里面的内容，只要取消，则不显示。在开发会员手机号码验证的时候需要用到，因为注册之后它会显示一个详细的注册项给大家填写会员信息，如果不填写，之前强制填写的项目就会被重置清空噢。

6、父类方法/函数怎么调用哈

在开发二次时，经常要重载原函数，怎么调用呢？代码如下，调用规则是：如果你的代码在原函数运行前执行，那么就把调用放在你的代码之后，反之亦然。

ctl_cart::checkout();  或者 parent::checkout();

7、如何转向/导向另外一个页面

if($url){
                header('Location: index.php?ctl=passport&amp;act=login');
                exit;
 }

8、如何在shopex之中显示提示信息呢？在调试时很有用滴

$this-&gt;splash('success','index.php?ctl=order/order&amp;act=index',"我就是调试信息");

9、要是找不到输出变量怎么办？尝试一下输出变量吧，搞不好就成

 $this-&gt;pagedata['order']['super'] = $this-&gt;system-&gt;op_is_super;

10、数组要如何显示出来？（在设计商品信息栏内显示销售量时用到哈）

读取一个函数，返回数据项，每次要处理时都很麻烦，怎么办？用下面的办法吧。

只要在ctl文件中

    $objC = &amp;$this-&gt;system-&gt;loadModel('utility/salescount');
    $aC = $objC-&gt;count_bybn($aGoods['bn']);
    $this-&gt;pagedata['data'] = $aC;

然后在html文件中这样子即可显示啦

        &lt;{foreach from=$data item=data key=key}&gt;
          &lt;tr&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$key+1}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.name}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.saleTimes}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.salePrice}&gt;&lt;/div&gt;&lt;/td&gt;
          &lt;/tr&gt;
        &lt;{/foreach}&gt;