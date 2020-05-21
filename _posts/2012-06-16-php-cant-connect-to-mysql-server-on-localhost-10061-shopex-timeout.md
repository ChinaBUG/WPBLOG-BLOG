---
ID: 2064
post_title: 'PHP-Can&#8217;t connect to MySQL server on &#8216;localhost&#8217; (10061)（ShopEX登陆超时）'
author: ChinaBUG
post_excerpt: |
  Can't connect to MySQL server on 'localhost' (10061)解决方法
  遇上这个提示是很悲剧的问题。因为网上没有特别有效的解决办法。偶也是遇上这个问题，经过层层的分析终于解决问题了。可是，也仅仅是靠猜测得出造成这个原因的缘故。
  这边说说偶的简单的思考吧。
  根据这个Can't connect to MySQL server on 'localhost'提醒，我们第一反应肯定是mysql服务器没有提供服务。确实！遇上这个提示一定是你的服务器暂停服务了。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/06/php-cant-connect-to-mysql-server-on-localhost-10061-shopex-timeout.html
published: true
post_date: 2012-06-16 16:23:53
---
Can't connect to MySQL server on 'localhost' (10061)解决方法

遇上这个提示是很悲剧的问题。因为网上没有特别有效的解决办法。偶也是遇上这个问题，经过层层的分析终于解决问题了。可是，也仅仅是靠猜测得出造成这个原因的缘故。

这边说说偶的简单的思考吧。

根据这个Can't connect to MySQL server on 'localhost'提醒，我们第一反应肯定是mysql服务器没有提供服务。确实！遇上这个提示一定是你的服务器暂停服务了。

但是在使用shopex的过程之后，出现的情况是，一会正常一会不正常，不正常必然会出现这个提示。

<span style="text-decoration: underline;">当然，出现这个提示的同时，shopex必然会出现一个情况，那就是登陆超时！！如果你的shopex一直提示登陆超时，需要重新登陆，而你修改了超时时间，却一直不能解决问题，那么请相信我，找找数据库问题吧。</span>

从正常的思维来说，服务器为什么一会能提供服务一会儿又不行呢？

这个说明什么呢？

说明服务器不稳定，这个是肯定的。

。。。

经过层层的分析。。。最后还是简单的一句话，服务器不稳定，而且还是软件环境的问题。怎么办？

软件的问题，自然就是重装呗。

所以我重装了服务器，问题解决！

后来，才知道这台服务器上之前装过一键PHP环境~~估计是没删除干净造成的软件的端口还是什么冲突吧。

&nbsp;