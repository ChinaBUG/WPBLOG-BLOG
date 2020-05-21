---
ID: 2139
post_title: ASP.NET-边角料代码的集散
author: ChinaBUG
post_excerpt: |
  1、这样子调用getClass方法。
  2、获取数据库记录
  3、异常抛出调试
layout: post
permalink: >
  http://blog.ipodmp.com/2012/07/asp-net-user-code.html
published: true
post_date: 2012-07-28 20:19:19
---
1、这样子调用getClass方法。

[asp]&lt;asp:Label ID="Label1" runat="server" Text='&lt;%# <span style="color: #ff0000;">getClass(Eval("Class"))</span> %&gt;' &gt;&lt;/asp:Label&gt;[/asp]

2、获取数据库记录

[asp]Imports System.Data.OleDb

Function getClass(ByRef s As Integer) As String
Dim dbconn, sql, dbcomm, dbread
dbconn = New OleDbConnection("Provider=Microsoft.Jet.OLEDB.4.0;Data Source=" &amp; Server.MapPath("~/App_Data/ZAS_MDB.mdb") &amp; ";")
dbconn.Open()
sql = "SELECT title FROM [WS_Product_CLS] where ID=" &amp; s
dbcomm = New OleDbCommand(sql, dbconn)
dbread = dbcomm.ExecuteReader()
'customers.DataSource = dbread
'customers.DataBind()
If Not dbread.Read() Then
Return "分类不存在"
Else
Return dbread("title")
End If
'
dbread.Close()
dbconn.Close()
End Function[/asp]

3、异常抛出调试

在.cs或者.vb文件内如果不能输出的话只能用异常来调试了

[asp]throw new Exception(“异常信息”);[/asp]