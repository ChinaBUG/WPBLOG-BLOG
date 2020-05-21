---
ID: 2270
post_title: Discuz-discuzX2论坛实现单版
author: ChinaBUG
post_excerpt: |
  原文来自：Discuz X2.5实现单版论坛的方法（适用于Discuz X2.0）[20120514]
  名词解释：
  问：什么是论坛单版？
  答：单版论坛就是论坛只有一个版块，并将这个版块的主题列表页设置为首页显示。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/discuz-forum-to-achieve-a-single-version.html
published: true
post_date: 2013-01-05 10:48:56
---
原文来自：<a href="http://www.discuz.net/thread-2308457-1-1.html">Discuz X2.5实现单版论坛的方法（适用于Discuz X2.0）[20120514]</a>

名词解释：

问：什么是论坛单版？

答：单版论坛就是论坛只有一个版块，并将这个版块的主题列表页设置为首页显示。

根据原文我在Discuz! X2.5 gbk版本上实验成功。代码如下：

//2013.01.05 实现论坛单版
$_GET +=array('mod'=&gt;'forumdisplay','fid'=&gt;<span style="color: #ff0000;">2</span>);

注意：上面的fid的值就是我们正常访问版块时，地址栏上的参数值，如http://bbs.ipodmp.com/forum.php?mod=<span style="color: #ff0000;">forumdisplay</span>&amp;fid=<span style="text-decoration: underline;"><strong><span style="color: #ff0000; text-decoration: underline;">2</span></strong></span>

然而，我们需要的可能不是这个效果，比如我一个版块叫虫子的乐趣，然后下面有很多的子版，我需要显示这个虫子的乐趣版块怎么办？上面的代码只能显示虫子的乐趣下面的版块。怎么办？请使用下面的代码吧：

//2013.01.05 实现论坛单版
$_GET +=array('mod'=&gt;'index','gid'=&gt;<span style="color: #ff0000;">1</span>);

注意：上面的gid的值就是我们正常访问版块时，地址栏上的参数值，http://bbs.ipodmp.com/forum.php?gid=<strong><span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">1</span></span></strong>

修改上传，效果显著呀。