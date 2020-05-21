---
ID: 264
post_title: ASP.NET常用的功能代码
author: ChinaBUG
post_excerpt: |
  1.在ASP里面直接使用的Left功能
  2.字段名不要以“_”开头
  3.只要是模板库里面的元件
layout: post
permalink: >
  http://blog.ipodmp.com/2009/11/asp-net-function-code-used.html
published: true
post_date: 2009-11-12 16:45:18
---
1.字段的字数太多，需要截取一些？在ASP里面直接使用的Left功能，现在在ASP.NET中就比较麻烦了，例子如下：

&lt;%# Microsoft.VisualBasic.Left(Eval("Pro_Title"), 11) %&gt;或者&lt;%# (Eval("info_Content")).ToString().SubString(0,380) %&gt;

当然，你也可以直接使用&lt;%# Left(Eval("Pro_Title"), 11) %&gt;也是能得到结果的，不过，会有什么不好，我也不知道，汗~！

要记住的是绑定的数据一定是用Eval而不是Bind要不会报错的。

参考网址：<a href="http://msdn.microsoft.com/en-us/library/microsoft.visualbasic.strings.left.aspx" target="_blank">微软MSDN</a>

2.字段名不要以“_”开头，要不，很多地方的SQL语句回执行不过去。

3.只要是模板库里面的元件，都能绑定，如选择功能就能绑定字段名。