---
ID: 674
post_title: 至少一个参数没有被指定值
author: ChinaBUG
post_excerpt: |
  　　异常详细信息: System.Data.OleDb.OleDbException: 至少一个参数没有被指定值。
  　　在ASP.NET开发的时候会出现如上错误:至少一个参数没有被指定值。
  　　出现这个错误的可能情况有两种，这边提供一下检查流程哈，防止自己忘记，~~人老了，经常会忽略一些什么东西哈~
  　　这个问题出现的主要原因是程序找不到你在SQL语句中提供的变量、或者在参数中提供的变量有错、或者你的数据库压根就没有这个字段存在。
  　　在ASP.NET中，SQL代码均能自动生成，所以，出现这个问题的时候要注意重点检查自己手动添加的字段名、变量，如果问题还在，那么一定就是数据库错误了，检查数据库内是否存在这个字段。
  　　为什么会出现数据库是否存在这个字段这个问题呢？有时候本地调试的好好地，弄到服务器，就出现问题，问题肯定就是数据库没更新呗，所以，要重新上传一下数据库，更新一下数据库即可。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/07/asp-net-one-parameter-value-is-not-specified.html
published: true
post_date: 2010-07-13 10:01:36
---
 
<h1>“/”应用程序中的服务器错误。

<hr size="1" />

</h1>
<h2><em>至少一个参数没有被指定值。</em></h2>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><strong>说明: </strong>执行当前 Web 请求期间，出现未处理的异常。请检查堆栈跟踪信息，以了解有关该错误以及代码中导致错误的出处的详细信息。</span></div>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><strong>异常详细信息: </strong>System.Data.OleDb.OleDbException: 至少一个参数没有被指定值。</span></div>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><strong>源错误:</strong></span></div>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"> </span></div>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"> 

</span>
<table width="100%" bgcolor="#ffffcc">
<tbody>
<tr>
<td><code>执行当前 Web 请求期间生成了未处理的异常。可以使用下面的异常堆栈跟踪信息确定有关异常原因和发生位置的信息。</code></td>
</tr>
</tbody>
</table>
 

　　异常详细信息: <strong>System.Data.OleDb.OleDbException: 至少一个参数没有被指定值</strong>。

　　在ASP.NET开发的时候会出现如上错误:至少一个参数没有被指定值。

　　出现这个错误的可能情况有两种，这边提供一下检查流程哈，防止自己忘记，~~人老了，经常会忽略一些什么东西哈~

　　这个问题出现的主要原因是程序找不到你在<span style="text-decoration: underline;">SQL语句中提供的变量</span>、或者在<span style="text-decoration: underline;">参数中提供的变量有错</span>、或者<span style="text-decoration: underline;">你的数据库压根就没有这个字段存在</span>。

　　在ASP.NET中，SQL代码均能自动生成，所以，出现这个问题的时候要注意<span style="text-decoration: underline;">重点检查自己手动添加的字段名、变量</span>，如果问题还在，那么一定就是数据库错误了，<span style="text-decoration: underline;">检查数据库内是否存在这个字段</span>。

　　为什么会出现数据库是否存在这个字段这个问题呢？有时候本地调试的好好地，弄到服务器，就出现问题，问题肯定就是数据库没更新呗，所以，要重新上传一下数据库，更新一下数据库即可。