---
ID: 3274
post_title: >
  ShopEX-开启Discuz6第三方整合出现已注册却提示验证码错误的问题
author: ChinaBUG
post_excerpt: |
  最近在研究ShopEX的第三方整合的时候发现，开启整合之后注册会出现问题，明明已经注册成功了，但是却提示验证码错误，然后退回注册界面，结果新注册的账户却已经登录了！
  好神奇，这个问题一开始我想复杂了，一个个的跟踪过去发现时系统先调用第三方的支持文件来注册然后又调用本身的注册方法，然后就会发现注册成功，但是原版的注册方法却找不到验证码的值了，因为转了一圈回来值就没了！
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/shopex-turn-on-discuz6-third-party-integration-has-been-registered-but-suggest-verification-code-error-problems.html
published: true
post_date: 2014-03-17 17:45:40
---
最近在研究ShopEX的第三方整合的时候发现，开启整合之后注册会出现问题，明明已经注册成功了，但是却提示验证码错误，然后退回注册界面，结果新注册的账户却已经登录了！

好神奇，这个问题一开始我想复杂了，一个个的跟踪过去发现时系统先调用第三方的支持文件来注册然后又调用本身的注册方法，然后就会发现注册成功，但是原版的注册方法却找不到验证码的值了，因为转了一圈回来值就没了！

具体原因请自己查看ctl.passport.php文件，即可明白了噢

下面是解决方案，打开ctl,passport.php文件找到方法create()所在：

function create(){......

直接添加下面的代码，然后问题解决，这个是简单的做法噢
<pre>//2014.03.17 修正注册的问题
if(!empty($_COOKIE['MEMBER'])){
    header('Location: /?member.html');
}</pre>
完工~~害我浪费了一下午的时间~