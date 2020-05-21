---
ID: 301
post_title: >
  ASP/VBScript
  日期时间格式化函数(取代FormatDateTime)
author: ChinaBUG
post_excerpt: |
  函数：FormatDT
  作者：Abo(wupwu@qq.com)
  日期：2008.09.07
  功能：日期时间格式化
  参数：DateTime,日期时间
  　　　Template,格式化模板
  返回：格式化后的字串
  备注：模板标签注释
layout: post
permalink: >
  http://blog.ipodmp.com/2010/01/asp-vbscript-date-and-time-formatting-functions-to-replace-formatdatetime.html
published: true
post_date: 2010-01-14 15:47:32
---
&lt;<a href="mailto:%@LANGUAGE=&quot;VBSCRIPT">%@LANGUAGE="VBSCRIPT</a>" CODEPAGE="936"%&gt;      
&lt;%      
Option Explicit      
     
'函数调用示例：      
Response.Write FormatDT(Now(),"{Y}-{M}-{D} {H}:{N}:{S}") &amp; "&lt;br&gt;"     
Response.Write FormatDT(Now(),"{Y}年{M}月{D}日 {H}时{N}分{S}秒") &amp; "&lt;br&gt;"     
Response.Write FormatDT(Now(),"{w},{D} {Me} {Y}") &amp; "&lt;br&gt;"     
     
'==================================================================      
'函数：FormatDT      
'作者：Abo(<a href="mailto:wupwu@qq.com">wupwu@qq.com</a>)      
'日期：2008.09.07      
'功能：日期时间格式化      
'参数：DateTime,日期时间      
'　　　Template,格式化模板      
'返回：格式化后的字串      
'备注：模板标签注释      
'　　　{Y}:年      
'　　　{y}:2位年      
'　　　{M}:月      
'　　　{m}:补位月，例：01,02      
'　　　{ME}:英文月份      
'　　　{Me}:英文月份缩写      
'　　　{D}:日      
'　　　{d}:补位日      
'　　　{H}:时      
'　　　{h}:补位时      
'　　　{N}:分      
'　　　{n}:补位分      
'　　　{S}:秒      
'　　　{s}:补位秒      
'　　　{W}:星期几英文      
'　　　{w}:星期几英文缩写      
'修改记录：      
'==================================================================      
     
Function FormatDT(DateTime,Template)      
    If (Not IsDate(DateTime)) or Template = "" Then     
        FormatDT = Template      
        Exit Function     
    End If     
    Dim dtmY,dtmM,dtmD,dtmH ,dtmN,dtmS,dtmW      
    Dim arrFW,arrSW,arrFM,arrSM      
    dtmY = Year(DateTime)      
    dtmM = Month(DateTime)      
    dtmD = Day(DateTime)      
    dtmH  = Hour(DateTime)      
    dtmN = Minute(DateTime)      
    dtmS = Second(DateTime)      
    dtmW = WeekDay(DateTime)      
    arrFW = Array("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")      
    arrSW = Array("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")      
    arrFM = Array("January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December")      
    arrSM = Array("Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec")      
     
    Template = Replace(Template,"{Y}",dtmY)      
    Template = Replace(Template,"{y}",Right(dtmY,2))      
    Template = Replace(Template,"{M}",dtmM)      
    Template = Replace(Template,"{m}",Right("00"&amp;dtmM,2))      
    Template = Replace(Template,"{ME}",arrFM(dtmM-1))      
    Template = Replace(Template,"{Me}",arrSM(dtmM-1))      
    Template = Replace(Template,"{D}",dtmD)      
    Template = Replace(Template,"{d}",Right("00"&amp;dtmD,2))      
    Template = Replace(Template,"{H}",dtmH )      
    Template = Replace(Template,"{h}",Right("00"&amp;dtmH ,2))      
    Template = Replace(Template,"{N}",dtmN)      
    Template = Replace(Template,"{n}",Right("00"&amp;dtmN,2))      
    Template = Replace(Template,"{S}",dtmS)      
    Template = Replace(Template,"{s}",Right("00"&amp;dtmS,2))      
    Template = Replace(Template,"{W}",arrFW(dtmW-1))      
    Template = Replace(Template,"{w}",arrSW(dtmW-1))      
    FormatDT = Template      
End Function     
%&gt;  

摘自：<a href="http://www.59ku.net/blog/article.asp?id=2">http://www.59ku.net/blog/article.asp?id=2</a>