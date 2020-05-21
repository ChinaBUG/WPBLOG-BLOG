---
ID: 2424
post_title: >
  密码保护：ShopEX-4.8.5.78056~78660支付方式内没办法查看更多的支付方式
author: ChinaBUG
post_excerpt: |
  ShopEX从最新版的开始，在支付方式管理里面查看支付方式会发现里面以前有的更多选项尽然没有了，受影响的版本测试4.8.5.78056、4.8.5.78660两个版本均不正常。
  不需要其他的支付方式的话，那倒还好，不显示就不显示吧，要是需要到其他的支付方式怎么办呢？
  自己动手解决吧~~
  您只需要按照下面的顺序修改，还你一个更多的世界~~~
  1、打开core\admin\view\payment\pay_index.html文件
  2、打开core\admin\controller\trading\ctl.payment.php文件
  3、清空缓存，查看，噢耶~~问题解决。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/04/shopex-4-8-5-78056-78660-payment-can-not-see-more-payment-methods.html
published: true
post_date: 2013-04-24 14:23:11
---
ShopEX从最新版的开始，在支付方式管理里面查看支付方式会发现里面以前有的更多选项尽然没有了，受影响的版本测试4.8.5.78056、4.8.5.78660两个版本均不正常。

不需要其他的支付方式的话，那倒还好，不显示就不显示吧，要是需要到其他的支付方式怎么办呢？

自己动手解决吧~~

您只需要按照下面的顺序修改，还你一个更多的世界~~~

1、打开core\admin\view\payment\pay_index.html文件

找到98行，添加如下代码：

&lt;div style="text-align:right; margin:5px 0; padding:0 20px;"&gt;
&lt;span onclick="new Request.HTML({url:'index.php?ctl=trading/payment&amp;act=index', update:$('payment-china'),data:'china=true'}).post();"&gt;查看更多&amp;gt;&amp;gt;&lt;/span&gt;
&lt;/div&gt;

保存。

2、打开core\admin\controller\trading\ctl.payment.php文件

找到$this-&gt;page('payment/pay_index.html');所在行，将这行注释，并按照下面方式添加下面的代码：

//$this-&gt;page('payment/pay_index.html');
if($_POST['china'] == 'true'){
$this-&gt;display('payment/pay_index_china.html');
}elseif($_POST['other'] == 'true'){
$this-&gt;display('payment/pay_index_other.html');
}else{
$this-&gt;page('payment/pay_index.html');
}

保存。

3、清空缓存，查看，噢耶~~问题解决。