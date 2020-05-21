---
ID: 350
post_title: >
  ASP.NET项目懒人开发-在同一个文件中按分类读入数据
author: ChinaBUG
post_excerpt: |
  1、分成两个文件，一个可以读入全部的数据，一个就是来判断分类号并读入分类数据。
  2、一个文件，就实现1中的功能。
  关键代码:
  ADS_Show_Content.SelectCommand = "SELECT * FROM [WS_News] where class ="&Request("Cid") & " ORDER BY [ID] DESC"
  ADS_Show_Content.DataBind()
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/asp-project-development-in-the-same-file-read-into-the-data-by-category.html
published: true
post_date: 2010-03-16 17:26:33
---
　　在网站开发中，我们经常需要按照分类读入数据，这个功能是最基本的，形式如下：

　　1、分成两个文件，一个可以读入全部的数据，一个就是来判断分类号并读入分类数据。

　　2、一个文件，就实现1中的功能。

　　在ASP.NET中真的很不习惯的，很多ASP中能轻松解决的，在ASP.NET却要费很大的劲，当然，你不使用里面的封装好的组件除外噢。

　　不罗嗦，1的方法不介绍，这边介绍一下2的方法。

　　首先，我们新建一个文件取名叫news.aspx吧，用的语言请自己决定，我是习惯VB啦。

　　然后添加GridView控件，设置好参数噢，要注意的是，现在的选择语句应该是选择全部的数据，毕竟我们在运用时，需要默认读入全部数据。

　　GridView的参数没什么特别就不说了，这边把AccessDataSource贴出来把，毕竟后面的操作需要用到噢，瞧一下才不会莫名其妙噢。

        &lt;asp:AccessDataSource ID="<span style="color: #993300;">ADS_Show_Content</span>" runat="server"
            DataFile="~/App_Data/NewSunPaper2010.mdb"
            SelectCommand="SELECT * FROM [WS_News] ORDER BY [ID] DESC"&gt;
        &lt;/asp:AccessDataSource&gt;

　　这是显示全部的数据，那么怎么在这个页面内用现有的GV来显示分类的数据呢？

　　其实很简单，两行代码就能搞定噢。

            <span style="color: #993300;">ADS_Show_Content</span>.SelectCommand = "SELECT * FROM [WS_News] where class ="&amp;Request("Cid") &amp; " ORDER BY [ID] DESC"

            <span style="color: #993300;">ADS_Show_Content</span>.DataBind()

　　当然，你需要自己加上判断的语句，要不出错别怪我噢。

　　然后你只需要用news.aspx?Cid=XX就能读入分类数据啦。

　　简单吧？！