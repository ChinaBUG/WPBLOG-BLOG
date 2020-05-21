---
ID: 4060
post_title: >
  alipay-怎么实现充值只能使用账户余额银行卡不能使用信用卡防止套现
author: ChinaBUG
post_excerpt: |
  用于控制收银台支付渠道显示，该值的取值范围请参见“11.3 支付渠道”。可支持多种支付渠道显示，以“^”分隔。可空，如directPay^bank
  Pay^cartoon^cash。
  
  参考11.3支付渠道，有如下取值：directPay 支付宝账户余额；cartoon 卡通；bankPay 网银；cash 现金；creditCardExpress 信用卡快捷；debitCardExpress 借记卡快捷；coupon 红包；point 积分；voucher 购物券。
layout: post
permalink: >
  http://blog.ipodmp.com/2015/06/alipay-how-can-only-use-the-account-bank-card-credit-cards-cannot-be-used-to-prevent-arbitrage.html
published: true
post_date: 2015-06-15 10:25:20
---
支付宝一直致力于防止信用卡套现的业务。但是，我们的支付宝要是开通了信用卡支付之后，要是集成到我们自己的独立商城上，这个时候会员同样是可以使用信用卡支付，这个就可能会涉及信用套现的风险，对商家的管理等方面极其不利。

我们网站也是，怎么限制使用信用卡给会员账户预付款充值呢？

想偷懒，去支付宝官网问客服，七扯八扯的，给我一个客服电话，打了，一大早一直占线，真是烦躁呀~怎么办呢？

只能是自救了，下载了集成文档，从头看起，然后就发现了一个参数，居然可以控制支付方式哈。

大公司就是大公司，原来都替我们考虑好了噢。

这个参数就是enable_paymethod支付渠道参数，属于业务参数。

文档说明：

用于控制收银台支付渠道显示，该值的取值范围请参见“11.3 支付渠道”。可支持多种支付渠道显示，以“^”分隔。可空，如directPay^bank
Pay^cartoon^cash。

参考11.3支付渠道，有如下取值：directPay 支付宝账户余额；cartoon 卡通；bankPay 网银；cash 现金；creditCardExpress 信用卡快捷；debitCardExpress 借记卡快捷；coupon 红包；point 积分；voucher 购物券。

注意，使用的时候提示参数错误的话，就要检查一下参数是否是如上的取值噢。

~.~ 感悟 ~.~

以后开发的时候一定要将开发文档通篇的仔细看看，了解API接口具体提供的功能，特别是大公司出品的。

2015.06.17 补充

因为原先我没有绑定信用卡，所以没有测试出来，经测试发现，信用卡快捷支付是会显示出来，即使使用directPay参数也是不行的。

经过咨询技术，告知我们的需求是不能直接通过api来调用的，需要自己设置界面来操作，具体例子可以查看携程网站的支付，我是不知道携程是不是那样的哈，不过我知道他给我一个sdk开发包哈~

例子下载：http://download.alipay.com/public/api/base/alipaydirect_bankpay_single.zip

等研究完再来继续~~