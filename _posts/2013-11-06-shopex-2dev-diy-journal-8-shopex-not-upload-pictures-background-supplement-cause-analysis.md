---
ID: 2743
post_title: >
  ShopEX二次开发DIY日记8之ShopEX后台不能上传图片的原因分析补充(为何我的图片上传后无法正常显示?)
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  在ShopEX-前台出现产品链接图片地址修改多了多级目录的链接乱码怎么解决(…/index.php/…)？一文中我们推荐了清风的《ShopEx后台不能上传图片的原因分析》一文，文中分析了几种ShopEX系统后台不能上传图片的原因，本文主要就是对这个原因做点补充哈~
  原因分析一文中分析了如下原因：
  1、 网站目录权限设置不当，没有写入、修改权限。
  2、GD库没有配置。
  3、二次开发或修改影响了后台图片上传功能。
  4、网站的磁盘空间不足！
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-8-shopex-not-upload-pictures-background-supplement-cause-analysis.html
published: true
post_date: 2013-11-06 13:12:45
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

在<a title="ShopEX-前台出现产品链接图片地址修改多了多级目录的链接乱码怎么解决(…/index.php/…)？" href="http://blog.ipodmp.com/archives/shopex-front-address-product-images-appear-more-multi-level-directory-of-links-garbled-how-to-solve/">ShopEX-前台出现产品链接图片地址修改多了多级目录的链接乱码怎么解决(…/index.php/…)？</a>一文中我们推荐了清风的《<a href="http://www.hnqss.cn/help/help-122.html">ShopEx后台不能上传图片的原因分析</a>》一文，文中分析了几种ShopEX系统后台不能上传图片的原因，本文主要就是对这个原因做点补充哈~

原因分析一文中分析了如下原因：

1、 网站目录权限设置不当，没有写入、修改权限。 涉及到远程传输存储文件的目录（home、images、plugins、themes），是必须有写入、修改权限的，一般就是将它的权限设置 为:777(即可读、可写、可运行)。 如果home目录(涉及到的子目录为upload)、images目录没有写入权限，就不能上传图片，会提示上传失败；如果单是images目录没有写入 权限，后台上传图片能成功，但不能正常生成列表页缩略图、将会出现没有缩略图或者直接显示原始大图的情况。

2、GD库没有配置。 因为shopex依赖于使用GD库生成产品列表页缩略图、详细页和相册图（即每个产品至少生成3张前台使用的图片），如果php中没有配置GD库，将导致图片无法生成，前台显示不了图片。

3、二次开发或修改影响了后台图片上传功能。一个资深的ShopEx二次开发技术，一般是不会犯这样低级的错误的；但一些业余的、兼职的、菜鸟级别的php学习者，往往顾此失彼，开发A功能影响了B 功能。 所以，如果你想让网店保障运营，那尽量找官方或者资深的开发者来做二次开发会比较稳当一些。

那么我需要补充的是什么原因呢？！

4、网站的磁盘空间不足！

如果你发现你程序都没有修改，环境没有变动，以前可以上传，突然之间发生了图片上传不了的情况，那么请不要着急，请检查一下自己的网站磁盘，看看容量是不是不足了！请清空一些垃圾文件再次尝试上传。

在ShopEX系统里面，最耗费磁盘容量的是home\logs（日志）、home\backup（使用系统后台备份的备份文件存放点）、home\upload（上传的商品图片原图）、images（商品图片规格图片）等目录看，所以偶尔有空的话，可以进入这些文件夹里面逛逛，看看有什么异常吧。

附录：其他相关的问题
<ol>
	<li><a href="bbs.shopex.cn/read.php?tid-313138.html">[分销王]为何我的图片上传后无法正常显示?（2013.11.19）</a></li>
</ol>
&nbsp;