---
ID: 1947
post_title: 'ShopEX-Fatal error: Call to a member function getfields() on a non-object'
author: ChinaBUG
post_excerpt: |
  最近一段时间，ShopEX商城会偶尔性的在编辑订单时会出现这个提示：
  访问网址：
  http://localhost/shopadmin/index.php?ctl=order/order&act=showEdit&p[0]=20120221186735
  错误提示：
  Fatal error: Call to a member function getfields() on a non-object in G:\PHPnow\htdocs\core\model_v5\trading\mdl.payment.php on line 954
layout: post
permalink: >
  http://blog.ipodmp.com/2012/02/shopex-fatal-error-call-to-a-member-function-getfields-on-a-non-object.html
published: true
post_date: 2012-02-22 11:57:06
---
最近一段时间，ShopEX商城会偶尔性的在编辑订单时会出现这个提示：

访问网址：

http://localhost/shopadmin/index.php?ctl=order/order&amp;act=showEdit&amp;p[0]=20120221186735

错误提示：

<strong>Fatal error</strong>: Call to a member function getfields() on a non-object in <strong>G:\PHPnow\htdocs\core\model_v5\trading\mdl.payment.php</strong> on line <strong>954</strong>

&nbsp;

原意是这个函数调用失败，一般的情况是指定的参数不存在或者不正确就会有这样的提示。

经过层层深入的调试，偶终于找到原因了~下面提供解决方法，可能不适用您的系统，仅供参考噢

经过对比，发现订单的表里面的extend字段会变成类似下面的值：a:1:{s:6:"bankId";s:4:"1002";}而正常的可编辑的订单却不是这样的字符串噢。绝大部分的值为false。

只要把这个值改为false就可以编辑订单了噢。

经过代码的研究发现设置这个变量的是99bill支付方式里面。但是，这个支付方式我们并没有启用，具体是为什么噢？不研究了，能用就好~~有研究的人可以留言告诉我结果滴