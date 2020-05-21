---
ID: 145
post_title: >
  Flash点击激活与虚线框清除(怎么去掉
  flash点击的虚线)
author: ChinaBUG
post_excerpt: |
  MS有时候就是犯傻，搞个版权害的做FLASH页面的时候要多点一次激活FLASH文件，还会出现虚线框，实在是无语。
  ObjectSwap的实现只需要在<head>中引入一个脚本。其逻辑是在页面加载完成后，重写一次<object>标签来实现自动激活。</object></head>
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/activation-flash-and-clean-dashed-border.html
published: true
post_date: 2009-08-14 10:44:34
---
MS有时候就是犯傻，搞个版权害的做FLASH页面的时候要多点一次激活FLASH文件，还会出现虚线框，实在是无语。

目前解决办法多种多样，有些在操作上实在是麻烦，不是要修改代码就是增加的代码很多，人懒没办法，能偷懒就的偷懒，用了ObjectSwap之后天下太平啦，只需要增加一句代码，什么都搞定。

<span style="color: #0000ff;">ObjectWrap</span>

ObjectSwap的实现只需要在&lt;head&gt;中引入一个脚本。其逻辑是在页面加载完成后，重写一次&lt;object&gt;标签来实现自动激活。

特点是页面中的flash依然是标准的HTML。不需要通过document.write写入。而且对于禁用js的用户Flash依然可以正常显示。

这应该算是最unobtrusive的一种实现了。

使用步骤：

1、下载 <a title="objectSwap" href="http://seaone.yfyf.net/up-files/objectSwap.rar" target="_blank">ObjectWrap Js</a> | <a href="http://www.uudisc.com/user/seaone/file/1912715" target="_blank">UUShare</a>

2、在head头中添加下面代码
&lt;script type="text/javascript" src="objectSwap.js"&gt;&lt;/script&gt;

3、运行激活问题、虚线框问题就解决了。