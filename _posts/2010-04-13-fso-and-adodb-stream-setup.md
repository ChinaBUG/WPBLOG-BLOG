---
ID: 443
post_title: 组件FSO与adodb.stream的安装
author: ChinaBUG
post_excerpt: |
  在ASP服务器中，很关键的FSO组件与adodb.stream的安装方法，应付有时重装的情景吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/fso-and-adodb-stream-setup.html
published: true
post_date: 2010-04-13 20:02:38
---
<span style="text-decoration: underline;"><strong>FSO组件安装</strong></span>

　　1、首先在系统目录中查找scrrun.dll，如果存在这个文件，请跳到第三步，如果没有，请执行第二步。

　　2、在系统安装盘i386目录中找到scrrun.dl_，用winrar解压缩，得scrrun.dll，然后复制到你的系统目录c:windowssystem32目录中。

　　3、运行regsvr32 scrrun.dll即可。

　　4、如果想关闭FSO组件，请运行 regsvr32 /u scrrun.dll即可。

<strong><span style="text-decoration: underline;">adodb.stream安装</span></strong>

　　regsvr32 "C:\Program Files\Common Files\System\ado\msado15.dll"

　　即可再次支持adodb.stream组件

　　注意：如果系统该位置没有msado15.dll，可能注册不成功，提示"找不到此文件等等"去<a href="http://www.zhaodll.com/">http://www.zhaodll.com/</a>按关键字“msado15”搜索下载该文件复制到如上路径再注册再进行regsvr32。