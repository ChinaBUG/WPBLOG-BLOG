---
ID: 2366
post_title: ASP-类型不匹配:clng
author: ChinaBUG
post_excerpt: |
  技术信息（用于支持人员）
  错误类型：
  Microsoft VBScript 运行时错误 (0x800A000D)
  类型不匹配: 'clng'
  /aspcms/admin_aspcms/editor/fckeditor.asp, 第 187 行
  浏览器类型：
  Mozilla/5.0 (Windows NT 5.1; rv:17.0) Gecko/17.0 Firefox/17.0
layout: post
permalink: >
  http://blog.ipodmp.com/2013/02/asp-aspcms-type-mismatch-function-clng.html
published: true
post_date: 2013-02-21 16:30:55
---
在使用ASPcms系统的时候有时候会出现如下错误，怎么办噢。。看了一下代码，原来是代码的问题，就简单处理一下吧。

错误提示如下：

技术信息（用于支持人员）
<ul>
	<li>错误类型：
Microsoft VBScript 运行时错误 (0x800A000D)
类型不匹配: 'clng'
<b>/aspcms/admin_aspcms/editor/fckeditor.asp, 第 187 行</b></li>
</ul>
&nbsp;
<ul>
	<li>浏览器类型：
Mozilla/5.0 (Windows NT 5.1; rv:17.0) Gecko/17.0 Firefox/17.0</li>
</ul>
解决办法：

找到fckeditor.asp 187行，将代码修改为如下形式：

'2013.02.20 针对部分浏览器会提示Clng错误
if IsNumeric(Mid( sAgent, InStr( sAgent, "Gecko/" ) + 6, 8 ))=true then
iVersion = CLng( Mid( sAgent, InStr( sAgent, "Gecko/" ) + 6, 8 ) )
else
iVersion = 17.0 '这个是实际的浏览器的参数值，就是上面错误信息里卖弄的浏览器类型Gecko/17.0中的数字
end if
'iVersion = CLng( Mid( sAgent, InStr( sAgent, "Gecko/" ) + 6, 8 ) )

然后保存，问题解决了~