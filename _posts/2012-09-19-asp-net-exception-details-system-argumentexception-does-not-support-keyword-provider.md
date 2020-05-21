---
ID: 2178
post_title: 'ASP.net-异常详细信息: System.ArgumentException: 不支持关键字: provider'
author: ChinaBUG
post_excerpt: |
  “/”应用程序中的服务器错误。
  不支持关键字: “provider”。
  说明: 执行当前 Web 请求期间，出现未处理的异常。请检查堆栈跟踪信息，以了解有关该错误以及代码中导致错误的出处的详细信息。
  异常详细信息: System.ArgumentException: 不支持关键字: “provider”。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/09/asp-net-exception-details-system-argumentexception-does-not-support-keyword-provider.html
published: true
post_date: 2012-09-19 22:46:43
---
<h1>“/”应用程序中的服务器错误。</h1>

<hr size="1" width="100%" />

<h2><em>不支持关键字: “provider”。</em></h2>
<span style="font-family: Arial,Helvetica,Geneva,SunSans-Regular,sans-serif;"><strong>说明: </strong>执行当前 Web 请求期间，出现未处理的异常。请检查堆栈跟踪信息，以了解有关该错误以及代码中导致错误的出处的详细信息。</span>

<strong>异常详细信息: </strong>System.ArgumentException: 不支持关键字: “provider”。

&nbsp;

话说，最近在一个项目中需要用到连接Access数据库，然后又想要使用SqlDataSource控件，所以就按照正常的在web.config里面做了如下配置：

&lt;connectionStrings&gt;
&lt;add name="ConnString" providerName="System.Data.OleDb" connectionString="Provider=Microsoft.Jet.OLEDB.4.0;Data Source=|DataDirectory|\MDB.mdb"/&gt;
&lt;/connectionStrings&gt;

然后再调用：

&lt;asp:SqlDataSource ID="ads_class2_list" runat="server"
ConnectionString="&lt;%$ ConnectionStrings:ConnString%&gt;"
SelectCommand="SELECT [ID], [Title], [parent] FROM [WS_Product_CLS] WHERE ([parent] = ?) ORDER BY [ID] DESC"&gt;
&lt;SelectParameters&gt;
&lt;asp:QueryStringParameter DefaultValue="1" Name="parent" QueryStringField="cls"
Type="Int32" /&gt;
&lt;/SelectParameters&gt;
&lt;/asp:SqlDataSource&gt;

然后就会出现如上的错误，找了一下这个错误信息，按照找到的来操作，结果，可能运气不好吧，没有一个能解决的，靠~~真是需要的时候才发现自己的基础不行~

回MS老家看看，还真的有~

其实就增加一个参数而已而难题解决~~

白费劲~献给同样遇上问题且找很久没解决的朋友吧~

&lt;asp:SqlDataSource ID="ads_class2_list" runat="server"
<span style="color: #ff0000;">ProviderName="System.Data.OleDb"</span>
ConnectionString="&lt;%$ ConnectionStrings:ConnString%&gt;"
SelectCommand="SELECT [ID], [Title], [parent] FROM [WS_Product_CLS] WHERE ([parent] = ?) ORDER BY [ID] DESC"&gt;
&lt;SelectParameters&gt;
&lt;asp:QueryStringParameter DefaultValue="1" Name="parent" QueryStringField="cls"
Type="Int32" /&gt;
&lt;/SelectParameters&gt;
&lt;/asp:SqlDataSource&gt;

说白点，就增加ProviderName="System.Data.OleDb"这个参数值哈~~

另外一种语法是：

ConnectionString="&lt;%$ ConnectionStrings:ConnString %&gt;"
ProviderName="&lt;%$ ConnectionStrings:ConnString.ProviderName %&gt;"

这个是用控件向导自动生成的哈~

PS...其实在使用控件向导时，你可以选择SqlDataSource然后就会出现指定参数的选项，然后还会自动在web.config里面新建连接字段保存连接的字符串，自动生成的就是上面的代码了。

参考：<a href="http://msdn.microsoft.com/zh-cn/library/system.web.ui.webcontrols.sqldatasource.connectionstring%28v=VS.80%29.aspx">SqlDataSource.ConnectionString 属性</a>