---
ID: 4391
post_title: 'ASP.net-使用Access数据库经常性出现asp.net access Exception Details: System.Data.OleDb.OleDbException: 未指定的错误'
author: ChinaBUG
post_excerpt: '使用Access数据库经常性出现asp.net access Exception Details: System.Data.OleDb.OleDbException: 未指定的错误'
layout: post
permalink: >
  http://blog.ipodmp.com/2016/04/asp-net-access-details-system-data-oledb-oledbexception-unspecified-error-occurred-frequently-using-access-database.html
published: true
post_date: 2016-04-10 13:56:23
---
老客户的网站我都是使用ASP制作的，只有一个站点是使用ASP.NET的，结果，就是这个站每次都给我带来麻烦，还好，这个站的流量不是很大，要不就悲剧了。

使用翼讯的虚拟主机的时候就会经常性的出现，让我烦不胜烦，提交技术处理也没处理好，懒得测试，最近买了云主机，把几个站都给集中在一起了，然后发现这个问题照旧，怎么办？

提交工单技术也是处理半天还是没解决，建议转为MSSQL的，那么我就是那种随便就妥协的人嘛？

当然不是了。

网上找了很多文章，结果，你懂的，没屁用！！问题照旧。

那么怎么办呢？

终于在一篇文章中看到一点点的相关的东西了，具体文章地址给关了，也没找到到底是在哪里看到的。

只需要设置应用程序池的高级属性内的：

“请求限制”为1就可以了。

[caption id="attachment_4392" align="alignnone" width="449"]<a href="http://blog.ipodmp.com/wp-content/uploads/2016/04/asp-error.png" rel="attachment wp-att-4392"><img class="size-full wp-image-4392" src="http://blog.ipodmp.com/wp-content/uploads/2016/04/asp-error.png" alt="ASP.net+ACCESS未指定的错误" width="449" height="541" /></a> ASP.net+ACCESS未指定的错误[/caption]

很简单，粗暴，解决了，然后我用测试网速的程序测试一下，发现再也没有出现这个错误了，不知道是不是有什么后遗症哈。

总的来说是解决了问题。

&nbsp;

&nbsp;