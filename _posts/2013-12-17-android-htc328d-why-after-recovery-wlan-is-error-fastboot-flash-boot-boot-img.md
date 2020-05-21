---
ID: 3035
post_title: >
  Android-htc328d刷完机发现WLAN不能用，老是开启错误
author: ChinaBUG
post_excerpt: |
  最近手机安装的东西很多，好卡~就刷机了~结果下载的好几个版本都是刷完之后会发现，WLAN功能不能使用，我还以为是什么硬件问题，然后上网找了一下原来操作有误，姑且这么说吧，需要单独刷入boot.img文档才可以，然后按照教程做了一遍还真的可以了，在这边记录一下，省的以后还要找。
  这个操作需要用到adb命令行，所以要嘛自己安装要嘛就是装一下刷机的工具，我这边安装的是刷机精灵，虽然刷机的效果不怎样，但是这些小工具还是挺齐全的。
  第一步需要连接一下手机，然后用刷机精灵进入fastboot模式，然后你会发现屏幕上提示fasrboot usb虾米字样；
  第二步进入刷机精灵进入adb命令行
  第三步就是在命令行里面输入：fastboot flash boot c:\boot.img
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/android-htc328d-why-after-recovery-wlan-is-error-fastboot-flash-boot-boot-img.html
published: true
post_date: 2013-12-17 17:18:03
---
最近手机安装的东西很多，好卡~就刷机了~结果下载的好几个版本都是刷完之后会发现，WLAN功能不能使用，我还以为是什么硬件问题，然后上网找了一下原来操作有误，姑且这么说吧，需要单独刷入boot.img文档才可以，然后按照教程做了一遍还真的可以了，在这边记录一下，省的以后还要找。

这个操作需要用到adb命令行，所以要嘛自己安装要嘛就是装一下刷机的工具，我这边安装的是刷机精灵，虽然刷机的效果不怎样，但是这些小工具还是挺齐全的。

第一步需要连接一下手机，然后用刷机精灵进入fastboot模式，然后你会发现屏幕上提示fasrboot usb虾米字样；

第二步进入刷机精灵进入adb命令行

第三步就是在命令行里面输入：fastboot flash boot c:\boot.img

这边要注意的是boot.img这个文件需要先拷贝到c:盘噢，可以是其他盘，但是路径需要自己修改。

附录：

对于想要了解boot.img的结构的朋友，推荐阅读<a href="http://blog.csdn.net/zhenwenxian/article/details/6219431">android boot.img 结构 </a>