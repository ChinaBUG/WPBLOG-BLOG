---
ID: 3224
post_title: >
  ASP-ASPCMS2.5系统中搜索页面出现800a0408
  Invalid character错误
author: ChinaBUG
post_excerpt: |
  最近在用新版的ASPCMS2.5 CMS系统制作网站，然后再一个问题上卡死了，输入关键词搜索的时候老是会出现下面的错误，但是其他页面都是正常的。
  错误信息如下：
  Microsoft VBScript compilation 错误 '800a0408'
  Invalid character
  /hdc/inc/AspCms_MainClass.asp，行 1803
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/asp-aspcms2-5-system-search-page-appears-800a0408-invalid-character-error.html
published: true
post_date: 2014-03-13 11:13:03
---
最近在用新版的ASPCMS2.5 CMS系统制作网站，然后再一个问题上卡死了，输入关键词搜索的时候老是会出现下面的错误，但是其他页面都是正常的。

错误信息如下：

<span style="font-family: 宋体; font-size: small;">Microsoft VBScript compilation </span> <span style="font-family: 宋体; font-size: small;">错误 '800a0408'
Invalid character
/hdc/inc/AspCms_MainClass.asp</span><span style="font-family: 宋体; font-size: small;">，行 1803</span>

到1803行找了一下，关于if语句的问题，于是一步步的调试过去，最终发现在我使用{aspcms:topsortid}变量时出现问题，可奇怪地是，这个语句包含在头部，而且是每一页都使用到的，为什么就这边出现错误呢？在ASPCMS2.5以前版本{aspcms:topsortid}这个变量是不受支持的，就是没值，难道？！

好吧，查看search.asp查看调用的整个过程发现，果然，程序没有对这个变量做设置！但是其他页面却有，好吧~增加上去吧

打开search.asp找到

.parseHtml()

在下面添加

.indexpath()

保存，问题解决了噢~~

PS：另外，我发现现在ASPCMS2.5新版本的问题有点多，哎，烦躁，难道又要重新改起~