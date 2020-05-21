---
ID: 844
post_title: >
  未能加载文件或程序集(system
  design version 4.0.0.0 culture neutral
  publickeytoken)
author: ChinaBUG
post_excerpt: |
  分析器错误信息: 未能加载文件或程序集“System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35”或它的某一个依赖项。找到的程序集清单定义与程序集引用不匹配。 (异常来自 HRESULT:0x80131040)
  
  解决方案：
  直接把web.config文件内的那一串关于System.Web.Extensions的内容删除即可正常了，为什么会出现这样的情况，我也不知道，我只是删除自动添加的这些代码，程序就能运行了。
  要时刻监控web.config文件内的内容呀~~VS个自作聪明的家伙噢~~
layout: post
permalink: >
  http://blog.ipodmp.com/2010/10/system-web-extensions.html
published: true
post_date: 2010-10-25 17:31:43
---
“/”应用程序中的服务器错误。
--------------------------------------------------------------------------------
配置错误
说明: 在处理向该请求提供服务所需的配置文件时出错。请检查下面的特定错误详细信息并适当地修改配置文件。
分析器错误信息: 未能加载文件或程序集“System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35”或它的某一个依赖项。找到的程序集清单定义与程序集引用不匹配。 (异常来自 HRESULT:0x80131040)
源错误: 行 24: 行 25: 行 26: 行 27: 行 28:
源文件: agent\web.config
行: 26
程序集加载跟踪:
下列信息有助于确定程序集“System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35”无法加载的原因。
警告: 程序集绑定日志记录被关闭。要启用程序集绑定失败日志记录，请将注册表值 [HKLM\Software\Microsoft\Fusion!EnableLog] (DWORD)设置为 1。注意: 会有一些与程序集绑定失败日志记录关联的性能损失。要关闭此功能，请移除注册表值 [HKLM\Software\Microsoft\Fusion!EnableLog]。
--------------------------------------------------------------------------------
版本信息: Microsoft .NET Framework 版本:2.0.50727.3603; ASP.NET 版本:2.0.50727.3082

============================================

解决方案：

直接把<strong>web.config</strong>文件内的那一串关于System.Web.Extensions的内容删除即可正常了，为什么会出现这样的情况，我也不知道，我只是删除自动添加的这些代码，程序就能运行了。

要时刻监控<strong>web.config</strong>文件内的内容呀~~VS个自作聪明的家伙噢~~

附注：

我系统上，每当转换成模板属性的话，就会自动添加这段代码，直接删除即可。请根据实际情况删除噢。仅供参考。

&lt;assemblies&gt;
&lt;add assembly="System.Design, Version=2.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.Web.Extensions, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System.ServiceModel.Activation, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System.Runtime.Serialization, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.ServiceModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Xml, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Web.Services, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Core, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Data.Linq, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Drawing, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.ServiceModel.Web, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System.Data.Services.Client, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Data.Services.Design, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Data.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Design, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B03F5F7F11D50A3A"/&gt;
&lt;add assembly="System.Web.DynamicData, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System.ComponentModel.DataAnnotations, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31BF3856AD364E35"/&gt;
&lt;add assembly="System.Web.Entity, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;
&lt;add assembly="System.Xml.Linq, Version=4.0.0.0, Culture=neutral, PublicKeyToken=B77A5C561934E089"/&gt;&lt;/assemblies&gt;