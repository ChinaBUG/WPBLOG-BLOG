---
ID: 2067
post_title: >
  ShopEX-登陆时验证码显示错误造成不能登陆
author: ChinaBUG
post_excerpt: |
  最近不知道是为了啥，我的ShopEX出现错误了！
  前台跟后台全部不能显示验证码~大家都知道，验证码很重要滴，怎么办~？
  还是老话，有些时候不能等、不能老指望ShopEX的老东家~~要靠我们自己的双手呀~
  经过源码的查阅跟无数次的实验在经过漫长的学习，终于让我找到解决的办法了~~
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/shopex-landing-verification-code-error-caused-by-not-landing.html
published: true
post_date: 2012-05-28 21:11:46
---
最近不知道是为了啥，我的ShopEX出现错误了！

前台跟后台全部不能显示验证码~大家都知道，验证码很重要滴，怎么办~？

还是老话，有些时候不能等、不能老指望ShopEX的老东家~~要靠我们自己的双手呀~

经过源码的查阅跟无数次的实验在经过漫长的学习，终于让我找到解决的办法了~~

原来只需要一行代码就可以解决的问题，TNN的那帮人是吃什么的~~尽然不速度解决问题。

解决办法是：

在输出前清除缓存即可。

ob_clean();

搞定~

附录：如果出现这种情况的话，请事先先检查自己的二次开发内容是否存在编码问题，特别是BOM文件头！具体原因请看本博客的 <a title="PHP-Unicode签名(BOM)影响缓存输出实例" href="http://blog.ipodmp.com/archives/php-unicode-signature-bom-affect-cache-output-example/">PHP-Unicode签名(BOM)影响缓存输出实例</a> <a title="PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋" href="http://blog.ipodmp.com/archives/php-oh-bom-header-you-scoundrel/">PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋</a>