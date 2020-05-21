---
ID: 431
post_title: .NET获取URL来路
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/net-request-url.html
published: true
post_date: 2010-04-12 09:35:25
---
如果测试的url地址是<a href="http://www.test.com/testweb/default.aspx">http://www.test.com/testweb/default.aspx</a>,   结果如下：

Request.ApplicationPath:                 /testweb
Request.CurrentExecutionFilePath:        /testweb/default.aspx
Request.FilePath:                        /testweb/default.aspx
Request.Path:                            /testweb/default.aspx
Request.PhysicalApplicationPath:         E:\WWW\testweb\
Request.PhysicalPath:                    E:\WWW\testweb\default.aspx
Request.RawUrl:                          /testweb/default.aspx
Request.Url.AbsolutePath:                /testweb/default.aspx
Request.Url.AbsoluteUrl:                 <a href="http://www.test.com/testweb/default.aspx">http://www.test.com/testweb/default.aspx</a>
Request.Url.Host:                        <a href="http://www.test.com/">www.test.com</a>
Request.Url.LocalPath:                   /testweb/default.aspx