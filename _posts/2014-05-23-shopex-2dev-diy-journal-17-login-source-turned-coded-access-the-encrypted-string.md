---
ID: 3472
post_title: >
  ShopEX二次开发DIY日记17之会员登录的来源转向编码加密串的获取
author: ChinaBUG
post_excerpt: |
  今天，刚开发完Mootools版本的ShopEX4.85转盘游戏之水果机抽奖游戏插件之后，发现界面上面有一个提示，需要登录才能抽奖，那么到底是让客户自己点击顶部的登录或者注册链接还是提示之后直接转向到登录的界面？
  
  如果直接转向登录界面的话，怎么保证登录之后自动转向到游戏的界面继续抽奖游戏呢？
layout: post
permalink: >
  http://blog.ipodmp.com/2014/05/shopex-2dev-diy-journal-17-login-source-turned-coded-access-the-encrypted-string.html
published: true
post_date: 2014-05-23 00:11:25
---
今天，刚开发完Mootools版本的ShopEX4.85转盘游戏之水果机抽奖游戏插件之后，发现界面上面有一个提示，需要登录才能抽奖，那么到底是让客户自己点击顶部的登录或者注册链接还是提示之后直接转向到登录的界面？

如果直接转向登录界面的话，怎么保证登录之后自动转向到游戏的界面继续抽奖游戏呢？

经过试验，如果直接访问：

[html]http://www.xu.com/passport-login.html[/html]

登录之后将自动转向member.html也就是自动进入会员中心。

那么怎么才能定义我们的转向链接呢？查看源代码会发现这个是用base64方法做的加密，这个没研究，所以不懂~难道一定要懂吗？

本文将告诉你，怎么获取这个加密串，实现自定义转向噢。

下面是分析步骤，敬请进入分析角色噢~~

先打开需要转向的页面，我们这边的例子中，需要转向的链接为：

[html]http://www.xu.com/page-biaobairi.html[/html]

我们点击顶部的“请登录”链接，进入会员登录的界面，然后查看代码，你会发现代码中出现一个字段很可疑噢：

[html]&lt;input type=&quot;hidden&quot; value=&quot;http://www.xu.com/page-biaobairi.html&quot; name=&quot;ref_url&quot;&gt;[/html]

你看到的这个隐藏域的值正好是我们需要转向的地址！那么，这个变量时干嘛用的噢？如果你有查阅源代码你就知道这个是转向变量，也就是这个值参与后面的base64的运算。

随便输入一个账号，密码，然后提交，你就会发现提示错误，这个时候你会看到地址栏的网址变了！

[html]http://www.xu.com/passport-aHR0cDosLHd3dy54dS5jb20scGFnZS1iaWFvYmFpcmkuaHRtbA==-6aqM6K+B56CB5b2V5YWl6ZSZ6K+v77yM6K+36YeN5paw6L6T5YWl-login.html[/html]

然后，你会不会猜想一下，这两个加密字符串的作用呢？

根据MVC框架的规则，我们很容易将网址变成下面的：

[html]http://www.xu.com/passport-aHR0cDosLHd3dy54dS5jb20scGFnZS1jaGlodW9kYXp1b3poYW4uaHRtbA-login.html[/html]

然后尝试一下，你会就会发现，噢，原来第一个就是加密之后的字符串了，为什么呢？

其实就查看一下代码，你会发现隐藏域的值已经变成正确的转向地址了~