---
ID: 2438
post_title: >
  Android-assert failed:apply_patch_check
  for moto xt800
author: ChinaBUG
post_excerpt: |
  E:can't open /cache/recovery/command
  assert failed:apply_patch_check("/system/app/BCR.apk","41ed31....","27dadc....")
  E:Error in /sdcard/update.zip
layout: post
permalink: >
  http://blog.ipodmp.com/2013/05/android-assert-failedapply_patch_check-for-moto-xt800.html
published: true
post_date: 2013-05-16 13:19:10
---
最近也用上Android系统的手机啦，二手的噢，所以只好升级啦，结果升级包的时候老是发现提示这个问题：

目标手机：moto xt800

错误提示：

E:can't open /cache/recovery/command
assert failed:apply_patch_check("/system/app/BCR.apk","41ed31....","27dadc....")
E:Error in /sdcard/update.zip

重新还原出厂设置，就变成另外一个文件错误，这么瞎折腾了好久，网上找了，没啥解决办法呀。。。起码我是没看到解决方案，所以给官方写邮件，结果回复了噢。

回复如下：

针对您手机的问题，请确认手机之前<span style="text-decoration: underline;">是否有root或自行刷机过</span>，如果有的话就无法升级官方版本，<span style="text-decoration: underline;">建议联系维修点确认是否能恢复并升级。</span>如果没有的，建议重 新下载升级包再尝试一下，由于手机上没有设置可调节，如果不行的话建议联系维修点帮助升级。升级前备份好重要资料以免丢失。

哈~~正好我的机子是root过得，使用360破解过得，要去维修点呀，好吧，自己试试看吧~~找了一个文章来一步步的看，结果~刷机成功，然后下载最新的更新文件一更新，哈~~最新版的了噢。

附录：

<a href="http://download.pchome.net/home/mobile/motorola/down-189034-6.html">MOTO RSD Lite 刷机软件 5.0</a>   （电信的就不用试了，都是文件不存在，直接点击有线通下载吧）

具体刷机流程请看 <a href="http://bbs.189store.com/thread-5865-1-1.html">[ROM] MOTO xt800官方2.1刷至官方测试版2.2升级详细图文教程（更新2,2完整版官方ROM）</a>

XT800官方的刷机升级教程 <a href="http://www.motorola.com/Consumers/CN-ZH/_IndependentPages/XT800">XT800系统升级信息</a>

&nbsp;