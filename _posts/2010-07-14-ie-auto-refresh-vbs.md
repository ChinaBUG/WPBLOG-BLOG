---
ID: 682
post_title: IE自动刷新脚本
author: ChinaBUG
post_excerpt: |
  　　现在很多的论坛提供在线时间转换论坛货币的功能噢，不过限制10分钟内要有活动的操作，汗，我总不能一直手动刷新吧，直接弄一个脚本来操作啦。
  　　这个脚本的功能就是自动在限定的时间内，自动刷新网站，其他的用法，恩，你可以自己想噢。。。应用范围还是够广的。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/07/ie-auto-refresh-vbs.html
published: true
post_date: 2010-07-14 08:46:16
---
现在很多的论坛提供在线时间转换论坛货币的功能噢，不过限制10分钟内要有活动的操作，汗，我总不能一直手动刷新吧，直接弄一个脚本来操作啦。

这个脚本的功能就是自动在限定的时间内，自动刷新网站，其他的用法，恩，你可以自己想噢。。。应用范围还是够广的。

'IErefresh.vbs

option explicit
dim ie, url, refreshSeconds, i

url = InputBox("Enter web site URL (http:// optional)",,"<a href="http://share.zcool.com.cn/">http://share.zcool.com.cn</a>")
if url = "" then WScript.Quit
refreshSeconds = InputBox("输入刷新时间,单位为分钟",,480)
if IsEmpty(refreshSeconds) then WScript.Quit
set ie = WScript.CreateObject("InternetExplorer.Application", "ie_") '这个对象的用法，请参考 <a href="http://www.devguru.com/Technologies/wsh/quickref/wscript_CreateObject.html" target="_blank">DevGuru</a>
ie.Navigate url
ie.Visible = false '这个取值有true|false，代表运行时是否显示IE，现在的设置为不显示，就是后台运行，不影响你其他的操作哈

do until ie.ReadyState = 4 : WScript.Sleep 100 : loop
while true
i = 0
while i &lt; CInt(refreshSeconds)
WScript.Sleep 1000
i = i + 1
wend
ie.refresh2 3 '3=REFRESH_COMPLETELY
wend

sub ie_onQuit
WScript.Quit
end sub

附录：2013.02.28

'IErefresh2.vbs
'打开一个本地网站，并且全屏
option explicit
dim ie, url, refreshSeconds, i
url = "c:\runonce\sys\index.html"

set ie = WScript.CreateObject("InternetExplorer.Application", "ie_")
ie.Navigate url

ie.Toolbar = false
ie.StatusBar = false
ie.Resizable = false
ie.AddressBar = false
ie.FullScreen = true
ie.Offline = false
ie.RegisterAsBrowser = true
ie.Visible = true

do until ie.ReadyState = 4 : WScript.Sleep 100 : loop

'因为某些原因不能使用上面的全屏参数，所以只能模拟操作了，正常手动按F11就可以全屏的
Dim WshShell
Set WshShell = Wscript.CreateObject("Wscript.Shell")
WshShell.SendKeys "{F11}"

sub ie_onQuit
WScript.Quit
end sub