---
ID: 3683
post_title: >
  OA-Notepad++的正则表达式运用例子收藏
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/oa-notepad-regular-expression-examples-using-collections.html
published: true
post_date: 2014-11-05 16:57:00
---
1、开发时输出数组之后要将数组整合成代码时替换

实例：

[0] =&amp;gt; %ID%
[1] =&amp;gt; %PID%
[2] =&amp;gt; %CID%
[3] =&amp;gt; %CREATETIME%
[4] =&amp;gt; %UPDATETIME%
[5] =&amp;gt; %PUBLISHTIME%
[6] =&amp;gt; %TITLE%
[7] =&amp;gt; %TITLE_EN%
[8] =&amp;gt; %AUTHOR%
[9] =&amp;gt; %SEO_TITLE%
[10] =&amp;gt; %SEO_KEYWORD%
[11] =&amp;gt; %SEO_DESC%
[12] =&amp;gt; %SEO_URL%
[13] =&amp;gt; %ABSTRACT%
[14] =&amp;gt; %THUMBNAIL_PIC%
[15] =&amp;gt; %BIG_PIC%
[16] =&amp;gt; %CONTENT%
[17] =&amp;gt; %TAGS%
[18] =&amp;gt; %ADDONE%
[19] =&amp;gt; %STATUS%
[20] =&amp;gt; %TOP%
[21] =&amp;gt; %HOT%
[22] =&amp;gt; %REC%

期望结果：

"%ID%",
"%PID%",
......

使用搜索，替换，查找模式为“正则表达式”，然后后

查找目标为：(.*)[(\d*)](.*)=&amp;gt;(.*)%(.*)%

替换为："%$4%",

点击全部替换，就得出结果了噢。

2、复制代码时有行号的时候

实例：

1.var SimpleSlideShowDemo = new Class({
2.        Implements: [Options, Events],
3.        options: {
4.                slides: [],
5.                startIndex: 0,
6.                wrap: true
7.                //onShow: $empty
8.        },
......

这是一个加行号的代码，我们要复制别人的代码时有时就会带上行号了，怎么办？用手删？不不~用Notepad++

使用搜索，替换，查找模式为“正则表达式”，然后后

查找目标为：^([0-9\.]{2,3})

替换为：请放空

点击全部替换，就得出结果了噢。

3、使用openssl生成RSA的公钥私钥PEM文件的时候，怎么按照64位一行组合？

今天在使用ECMobile的时候，里面需要用到支付宝的公钥私钥文件，然后按照教程去操作了，结果其中一步将文件全部的弄成一行之后又要按照每行64个字符分割，那么多的字符，输错了咋办呢？

有简单的办法没？

有，没有的话就不分享了呗。

打开notepad之后找找目标填写：.{64}

然后，点击查找下一个，就会发现，每64个字符就会高亮，这个时候你还怕分割错吗？！

4、