---
ID: 297
post_title: VBS-800A0408无效字符错误
author: ChinaBUG
post_excerpt: |
  脚本：XXXXX\XXXX.vbs
  行：1
  字符：1
  错误：无效字符
  代码：800A0408
  源：Microsoft VBscript编译器错误
layout: post
permalink: >
  http://blog.ipodmp.com/2010/01/vbs-invalid-character-error.html
published: true
post_date: 2010-01-14 15:33:56
---
诡异的错误，描述如下：有时候用记事本写完VBS代码，想测试一下，直接保存为*.vbs文件，然后双击运行，一般情况，都能马上运行。但偶尔会出现错误，令人苦恼，不过还好，问题被我找到了，记录一下。

错误提示如下：

脚本：XXXXX\XXXX.vbs
行：1
字符：1
错误：无效字符
代码：800A0408
源：Microsoft VBscript编译器错误

这个时候，你只要打开记事本，重新保存一下，记得保存编码要选择一下！正确的是Unicode或者是ANSI(前面的Unicode编码集合比后面的ANSI还要大，所以建议前面的Unicode编码肯定是没问题的)，不能是其他的，如果是其他的，那么明显，错误就是编码造成的。

修改之后问题照旧那就没办法了，本文仅仅是其中一种情况，不过就我遇上的问题来说，就这个而已，我经常使用WSH来自动化任务的。