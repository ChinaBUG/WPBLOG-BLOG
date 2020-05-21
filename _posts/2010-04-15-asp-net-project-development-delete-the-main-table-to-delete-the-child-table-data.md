---
ID: 447
post_title: >
  ASP.NET项目懒人开发-删除主表时删除子表数据
author: ChinaBUG
post_excerpt: |
  ASP.NET项目懒人开发-删除主表时删除子表数据
  　　在项目开发中，经常会出现这样的应用，主表数据删除后要求也要把相应的子表内的数据删除或者更正，那么怎么做到呢？
  　　恩，这个操作前提是，两个表之间的关系是用程序来维系的，而不是数据库本身的关系。
  　　本文就是分享这样的操作。
  　　例子中我们需要两个表格，一个表格称为A，为主表，另外一个称为B，为子表。现在用一个Gridview命名为GridView1载入A表，并设置删除功能，你也可以载入B表，这样在操作时能看得清楚数据的变化。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/asp-net-project-development-delete-the-main-table-to-delete-the-child-table-data.html
published: true
post_date: 2010-04-15 17:37:45
---
　　在项目开发中，经常会出现这样的应用，主表数据删除后要求也要把相应的子表内的数据删除或者更正，那么怎么做到呢？

　　恩，这个操作前提是，两个表之间的关系是用程序来维系的，而不是数据库本身的关系。

　　本文就是分享这样的操作。

　　例子中我们需要两个表格，一个表格称为A，为主表，另外一个称为B，为子表。现在用一个Gridview命名为GridView1载入A表，并设置删除功能，你也可以载入B表，这样在操作时能看得清楚数据的变化。

　　在用一个AccessDataSource命名为AccessDataSource1，这个可以有Gridview也可以没有，没什么关系。

　　现在需要进入代码编辑区，键入下面代码：

Protected Sub <span style="color: #ff0000;"><strong>GridView1</strong></span>_<span style="color: #ff0000;"><strong>RowDeleting</strong></span>(ByVal sender As Object, ByVal e As System.Web.UI.WebControls.GridViewDeleteEventArgs) Handles GridView1.RowDeleting
        <span style="color: #ff0000;"><strong>AccessDataSource2</strong></span>.<span style="color: #ff0000;">DeleteCommand</span> = "DELETE * FROM [<span style="color: #ff0000;"><span style="text-decoration: underline;">B</span></span>] WHERE [Parent] =" &amp; e.Keys(0)
        <span style="color: #ff0000;"><strong>AccessDataSource2</strong></span>.Delete()
End Sub

　　然后运行，OK~~~测试通过~~