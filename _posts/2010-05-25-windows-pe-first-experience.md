---
ID: 484
post_title: Windows PE初体验
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2010/05/windows-pe-first-experience.html
published: true
post_date: 2010-05-25 08:41:13
---
Windows PE 相关文章
<ol>
	<li><a href="http://technet.microsoft.com/zh-cn/library/cc766093(WS.10).aspx" target="_blank">什么是Windows PE?</a></li>
	<li><a href="http://technet.microsoft.com/zh-cn/library/cc721982(WS.10).aspx" target="_blank">什么是windows安装程序？</a></li>
	<li><a href="http://support.microsoft.com/kb/303891/zh-cn" target="_blank">如何在 Windows XP 中创建自定义启动 WinPE 光盘</a></li>
	<li><a href="http://technet.microsoft.com/zh-cn/library/cc766093(WS.10).aspx" target="_blank">将自定义脚本包括在 Windows PE 映像中</a></li>
	<li><a href="http://support.microsoft.com/kb/303891/zh-cn" target="_blank">如何在 Windows XP 中创建自定义启动 WinPE 光盘</a></li>
	<li><a href="http://technet.microsoft.com/zh-cn/library/cc766156(WS.10).aspx" target="_blank">Winpeshl.ini 文件</a></li>
</ol>
Windows PE资源下载
<ol>
	<li><a href="http://www.microsoft.com/downloads/details.aspx?displaylang=zh-cn&amp;FamilyID=c7d4bc6d-15f3-4284-9123-679830d629f2" target="_blank">Windows 自动安装工具包 (AIK) </a></li>
	<li><a href="http://www.microsoft.com/downloads/details.aspx?displaylang=zh-cn&amp;FamilyID=94bb6e34-d890-4932-81a5-5b50c657de08" target="_blank">Windows Vista SP1 和 Windows Server 2008 的自动安装工具包 (AIK)</a></li>
</ol>

<hr />

0xC0000017错误

如果在虚拟机中出现这个错误，我的解决办法是增大内存容量，默认是128MB的，我增加到512MB，因为WinPE未精简的默认数值就是512MB。