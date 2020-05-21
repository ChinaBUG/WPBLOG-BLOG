---
ID: 2121
post_title: >
  Godaddy-主机空间的cookie问题造成会员用户登录不了
author: ChinaBUG
post_excerpt: |
  最近，朋友的网站由国内的主机转移到GoDaddy上的主机，结果会员用户却登录不了~
  赶紧测试一下cookie看看，结果真的没值。
  经过检查代码使用cookie来保存登录信息的，检查了一下发现没什么问题，一开始怀疑是数据库的问题吧，排查后发现，不是。
  最后发现，如果在网址上增加子目录，就可以访问了哦。
  由此猜测可能是没有指定cookie的路径吧，上网去找了一圈，发现这个是Godaddy的正常现象，还真的是cookies的路径没有设置，造成读不到值。
  只要在需要设置的地方设置一下就可以：
  ......
  当然，你也可以指定路径的值，比如="/"或者是='/abc'了~~
  当然，最好这种重要的使用session比较好，要不很容易造成安全方面的隐患。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/07/godaddy-hosting-space-cookie-problems-caused-by-the-member-user-can-not-log.html
published: true
post_date: 2012-07-15 12:03:24
---
最近，朋友的网站由国内的主机转移到GoDaddy上的主机，结果会员用户却登录不了~

赶紧测试一下cookie看看，结果真的没值。

经过检查代码使用cookie来保存登录信息的，检查了一下发现没什么问题，一开始怀疑是数据库的问题吧，排查后发现，不是。

最后发现，如果在网址上增加子目录，就可以访问了哦。

由此猜测可能是没有指定cookie的路径吧，上网去找了一圈，发现这个是Godaddy的正常现象，还真的是cookies的路径没有设置，造成读不到值。

只要在需要设置的地方设置一下就可以：

'ChinaBUG@2012.07.15
Response.Cookies(webid).Path=""

当然，你也可以指定路径的值，比如="/"或者是='/abc'了~~

当然，最好这种重要的使用session比较好，要不很容易造成安全方面的隐患。