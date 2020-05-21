---
ID: 3359
post_title: 改变计算技术的伟大算法
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/change-the-algorithm-for-computing-greatest.html
published: true
post_date: 2014-03-28 09:47:02
---
本文由 <a href="http://blog.jobbole.com">伯乐在线</a> - <a href="http://blog.jobbole.com/author/linwenhui/">programmer_lin</a> 翻译自 <a href="http://en.docsity.com/news/interesting-facts/great-algorithms-revolutionized-computing/" target="_blank">docsity</a>。欢迎加入<a href="http://www.jobbole.com/groups/6/" target="_blank">技术翻译小组</a>。转载请参见文章末尾处的要求。

在过去，很多巧妙的计算机算法设计，改变了我们的计算技术。通过操作标准计算机中提供的中间运算符，可以产生很多的高效函数。这些函数导致了计算机 程序的复杂性和多样性，这也是今天计算机时代快速发展的重要原因。如下所示，我们列举了一些算法，它们改变了我们的计算机使用。
<h1>压缩技术</h1>
<h2>哈弗曼编码</h2>
<img alt="" src="http://ww1.sinaimg.cn/mw690/7cc829d3gw1eeb14igesag20a808unb4.gif" />
哈弗曼编码在无损数据压缩中广泛应用。为了找到一种最高效的二进制编码，哈弗曼在1951年提出了根据字符频率排序的二叉树这样的编码方法。这种方法被证 明，是最有效的编码方法。由于这种方法简单、高效，这种方法被用在很多的压缩方法中比如：DEFLATE（PKZIP压缩软件中的算法），以及很多的多媒 体编码包括JPEG和MP3中。
<h1>密码学</h1>
<h2>公共秘钥加密</h2>
<a title="改变计算技术的伟大算法" href="http://jbcdn2.b0.upaiyun.com/2014/03/1ec138d97f5d8af49e360ce6413f422f.png" rel="lightbox[61815]"><img alt="公共秘钥" src="http://jbcdn2.b0.upaiyun.com/2014/03/1ec138d97f5d8af49e360ce6413f422f-150x150.png" /></a>

对于加密算法而言，需要两种不同的秘钥，公共秘钥是用来作为加密的明文或者验证数字签名。私钥则用来解密密文，或生成数字签名。公共秘钥加密使得用 户可以在公共信道中安全传送数据。虽然这种方法于1997年发表，但是由英国政府通讯总部（GCHQ）的James H. Ellis, Clifford Cocks, Malcolm Williamson在1973年设计完成，并且投入使用。
<h1>搜索算法</h1>
<h2>Dijkstra 最短路径算法</h2>
<img alt="" src="http://ww3.sinaimg.cn/large/7cc829d3gw1eeb14kg74tg207v066ab4.gif" />
这一算法由Dijkstra在1956年完成，这是一个为图设计的搜索算法。它解决了单向图中的最短路径问题，因此，也可以用来生成最短路径树。很多基于 图的算法中，都应用了这样的算法来进行路径规划或是子路径选择。上图展示了在单向图中，利用这样的算法求最短路径的过程。
<h2>二分搜索算法</h2>
<img alt="" src="http://ww4.sinaimg.cn/mw690/7cc829d3gw1eeb14juhjvg208c06qgno.gif" />

二分搜索算法用来在已经有序的数组中找到关键字的位置。在说明词义的字典中，词的排列基本是有序的。电话本上，记录也都按照人名、地址或是电话号码排序。通过这样的算法，我们可以由人名，很快地在电话本中找到相应的电话以及地址。
<h1><a title="视觉直观感受 7 种常用的排序算法" href="http://blog.jobbole.com/11745/" target="_blank">排序算法</a></h1>
<h2>快速排序</h2>
<img alt="" src="http://ww1.sinaimg.cn/mw690/7cc829d3gw1eeb14godzxg208c050adl.gif" />

这种算法由Tony Hoare在1960年设计。这个算法本来用于调整待翻译单词的顺序，从而使它们与词典顺序更加一致，方便翻译。这种算法由于在Unix系统中被用作默认排序算法而声名大噪。同时，这种算法由于它在C语言标准库中的函数名“qsort”而得名。
<h1>数学方法</h1>
<h2>Karatsuba快速相乘算法</h2>
<img alt="" src="http://ww2.sinaimg.cn/large/7cc829d3gw1eeb14ixkuog207o08cmy4.gif" />
这种算法用来更快完成相乘的数学操作。由Anatolii Alexeevitch Karatsuba在1962年提出。它减少了乘法中需要操作的数字，并且提供了一个快速的相乘计算方法。这种算法的改进算法是Toom–Cook算法。 然而，对于大数相乘，Schönhage–Strassen 算法则是一种更快速的解决方案。
<h2>欧几里得算法（辗转相除）</h2>
<img alt="" src="http://ww4.sinaimg.cn/mw690/7cc829d3gw1eeb14fgch2g20dw08cwh4.gif" />

利用欧几里得算法，可以计算最大公约数。即两个正整数可以被整除的最大数。虽然这种算法只通过减法和比较来找到最大公约数，但是它被应用在了许多高 级算法中。欧几里得被认为是这个算法的发明者，欧几里得的这个算法被认为是欧几里得时期（公元前300年左右）最古老的算法之一。
<h1>图形学的发展</h1>
<h2>Bresenham直线算法</h2>
这种算法由Jack Elton Bresenham在1962年，他在IBM工作期间提出。这种算法本来用于在计算机屏幕上画出直线。算法用到的操作非常简单，整数的加法，减法和移位操 作。这在计算机图形学中是非常先进的方法。基于这样的方法，后来算法又有了一系列的拓展，比如：画圆算法等。由于这种算法的高效、快捷，至今在很多硬件中 （比如绘图仪和现代图形卡等）这种算法仍然十分重要并且仍在使用。.
<h2>平方根倒数速算法</h2>
这种算法提供了一种快速计算平方根的倒数的方法。这种方法在3D图像中广泛应用于确定光线和投影关系，这可能需要每秒上千万次的计算速度。在《雷神 之锤三：竞技场》的源代码中就有这样的算法，可是，直到2002年这种算法才被广泛应用。这个算法使用了一系列的简单操作来解决复杂问题。虽然很多人认 为，这种算法由John Carmack研发，但是，SGI和<a title="3dfx Interactive" href="http://zh.wikipedia.org/wiki/3dfx_Interactive">3dfx</a>早就曾在产品中应用此算法，当时应用的是Gary Tarolli实现的版本。