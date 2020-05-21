---
ID: 3922
post_title: >
  伪静态-将子目录转向新的站点规则怎么写
author: ChinaBUG
post_excerpt: |
  规则如下：
  RewriteEngine On
  RewriteBase /
  RewriteRule ^(news)(.*) http://www.91zhaota.com$2 [L,R=301]
  或者是
  RewriteEngine On
  RewriteBase /news
  RewriteRule ^(.*) http://www.91zhaota.com/$1 [L,R=301]
layout: post
permalink: >
  http://blog.ipodmp.com/2015/02/pseudo-static-subdirectories-to-new-site-rules-how-to-write.html
published: true
post_date: 2015-02-03 11:50:35
---
今天，老大终于觉悟了，要将主站内的子目录单独出一个域名，而我们就只能努力的去想办法实现这个需求了噢。

其实挺简单的，就是对伪静态规则不是很熟悉，需要折腾一下哈。

我们的目标是：

原站：http://www.ipodmp.com/news/......

改为：http://www.91zhaota.com/......

比如：

http://www.ipodmp.com/news/sell/2015/2/3/1.html

要自动转向

http://www.91zhaota.com/sell/2015/2/3/1.html

这样子的形式，不从SEO角度上来说，其实就是伪静态的转向规则的应用了噢。

经过我的测试，规则如下：

RewriteEngine On
RewriteBase /
RewriteRule ^(news)(.*) http://www.91zhaota.com$2 [L,R=301]

或者是

RewriteEngine On
RewriteBase /news
RewriteRule ^(.*) http://www.91zhaota.com/$1 [L,R=301]

然后你会发现，只要访问旧的网址就会自动转向了噢。简单吧。

上面的区别就是RewriteBase的使用，这个是设置基地址，就是告诉系统要从那个地方开始匹配规则。