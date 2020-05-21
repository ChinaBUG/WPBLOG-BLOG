---
ID: 3660
post_title: >
  SQL-MySQL的时间日期操作函数方法
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/10/sql-mysql-method-for-date-and-time-manipulation-functions.html
published: true
post_date: 2014-10-09 14:57:08
---
序

在开发的时候有时候需要涉及到时间日期，每次要生成测试数据时都是要写一个简单脚本，保存一下运行后才能得到当前或者目标的时间字符串，感觉好麻烦噢~比如：

$curDateTime = strtotime(date('Y-m-d H:i:s',time()));

获取当前的时间的Unix 时间戳日期。

文

phpmyadmin是咱做开发时经常性需要打开的软件之一，所以，其实学会一点点基础的话，对我们的工作是有很大的帮助的，今天就来学习一下MySQL的时间日期函数，方便解决平时需要的一些小需求噢。

我们先进入phpmyadmin然后打开SQL查询器，就是那个标志上有SQL字样的按钮噢。输入如下代码：

SELECT now();

点击执行，然后你会看到如下结果噢
<table id="table_results" class="data">
<thead>
<tr>
<th>now()</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class=" nowrap">2014-10-09 14:49:55</td>
</tr>
</tbody>
</table>
上面日期就是我当前写文章输入SQL语句的时间噢。很简单吧~

我们如果需要获取Unix 时间戳呢？其实就直接输入：

SELECT unix_timestamp();

就会得到了：
<table id="table_results" class="data">
<thead>
<tr>
<th>unix_timestamp()</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class=" nowrap" align="right">1412837488</td>
</tr>
</tbody>
</table>
哈~简单吧，再也不用写代码保存文件了~~当然你也可以指定你需要的时间，然后生成时间戳呗：

select unix_timestamp('2014-10-09 14:53:00');

结果就是：
<table id="table_results" class="data">
<thead>
<tr>
<th>unix_timestamp('2014-10-09 14:53:00')</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td class=" nowrap" align="right">1412837580</td>
</tr>
</tbody>
</table>
&nbsp;

更多的应用及例子，请查看下面的文章或者其他相关的文档噢：<a href="http://www.jb51.net/article/23966.htm">MySQL日期数据类型、时间类型使用总结</a>