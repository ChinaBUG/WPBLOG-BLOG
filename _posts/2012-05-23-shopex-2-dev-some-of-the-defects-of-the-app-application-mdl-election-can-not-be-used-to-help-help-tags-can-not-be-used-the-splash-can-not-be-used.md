---
ID: 2061
post_title: >
  ShopEX-二次开发之APP应用程序的一些缺陷：mdl多选不能用、help帮助标签不能用、splash不能使用
author: ChinaBUG
post_excerpt: |
  ShopEX-二次开发之APP应用程序的一些缺陷：mdl多选不能用、help帮助标签不能用、splash不能使用
  开发过ShopEX的APP应用程序的应用之后，会发现，mdl对象的多选功能不能使用、<help></help>帮助标签不能使用、splash提示方法不能使用的情况，这三者在ShopEX系统之中多好用，在这边就不多说了，就说说为什么不能使用吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/shopex-2-dev-some-of-the-defects-of-the-app-application-mdl-election-can-not-be-used-to-help-help-tags-can-not-be-used-the-splash-can-not-be-used.html
published: true
post_date: 2012-05-23 11:24:46
---
ShopEX-二次开发之APP应用程序的一些缺陷：mdl多选不能用、help帮助标签不能用、splash不能使用

开发过ShopEX的APP应用程序的应用之后，会发现，mdl对象的多选功能不能使用、&lt;help&gt;&lt;/help&gt;帮助标签不能使用、splash提示方法不能使用的情况，这三者在ShopEX系统之中多好用，在这边就不多说了，就说说为什么不能使用吧。

不能用太痛苦了，所以找了一天从头跟踪到尾，一个个调用找过去，才发现，原来是这么简单的~因为路径的问题！其实这些都是有调用，但是，都是空的，所以看起来是没效果的。

原因找到了，问题也就解决了，你还是不懂？那就没办法了，有做开发的应该能明白我说的意思，修改了就好了。

偶现在开发的APP应用程序是可以使用以上三者，再也不用自己编写提示方法了。

开心~~~