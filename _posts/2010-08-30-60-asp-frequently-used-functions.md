---
ID: 765
post_title: 60个ASP常用的函数全集
author: ChinaBUG
post_excerpt: |
  检测是否有效的E-mail地址
  过滤超链接
  过滤文件名字
  过滤特殊字符
  恢复特殊字符
  转换HTML代码
  反转换HTML代码
  恢复&字符
  过滤textarea
  过滤HTML代码
  防止外部提交
  IP过滤
  获得注册码
  限制上传文件类型
  检测是否只包含英文和数字
  检测是否只包含英文和数字
  检测是否有效的数字
  用户名检测
  分页函数
  切割内容 - 按行分割
  切割内容 - 按字符分割
  删除引用标签
  获取客户端IP
  获取客户端浏览器信息
  计算随机数
  自动闭合UBB
  自动闭合HTML
  读取文件
  保存文件
  数据库添加修改操作
  检测系统组件是否安装
  判断服务器Microsoft.XMLDOM
  判断服务器MSXML2.ServerXMLHTTP
  检查正则式
  获取在线人数
  2014.04.01
  检查组件是否已经安装
layout: post
permalink: >
  http://blog.ipodmp.com/2010/08/60-asp-frequently-used-functions.html
published: true
post_date: 2010-08-30 10:05:11
---
虫神提示您：
本文，虫子就检查前半部分的逻辑问题，顺便调整一下缩进格式，至于后半部分看得累了，就不检查了。
如果，你有需要用到这里面的函数，请一定，一定要自己检查一下并测试一下，当然，有问题可以联系我就是了~
对于程序调试问题，具体请查阅本站 &lt;&lt;<a title="永久链接到：调试ASP代码一定要装IIS吗" href="http://blog.ipodmp.com/archives/debug-asp-code-you-must-install-iis/" rel="bookmark">调试ASP代码一定要装IIS吗</a>&gt;&gt;

’*************************************
’检测是否有效的E-mail地址
’*************************************
Function IsValidEmail(Email)
Dim names, name, i, c
IsValidEmail = True
Names = Split(email, "@")
If UBound(names) &lt;&gt; 1 Then
IsValidEmail = False
Exit Function
End If
For Each name IN names
If Len(name) &lt;= 0 Then
IsValidEmail = False
Exit Function
End If
For i = 1 to Len(name)
c = Lcase(Mid(name, i, 1))
If InStr("abcdefghijklmnopqrstuvwxyz_-.", c) &lt;= 0 And Not IsNumeric(c) Then
IsValidEmail = false
Exit Function
End If
Next
If Left(name, 1) = "." or Right(name, 1) = "." Then
IsValidEmail = false
Exit Function
End If
Next
If InStr(names(1), ".") &lt;= 0 Then
IsValidEmail = False
Exit Function
End If
i = Len(names(1)) - InStrRev(names(1), ".")
If i &lt;&gt; 2 And i &lt;&gt; 3 Then
IsValidEmail = False
Exit Function
End If
If InStr(email, "..") &gt; 0 Then
IsValidEmail = False
End If
End Function

’*************************************
’过滤超链接
’*************************************
Function checkURL(ByVal ChkStr)
Dim str:str=ChkStr
str=Trim(str)
If IsNull(str) Then
checkURL = ""
Exit Function
End If
Dim re
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
re.Pattern="(d)(ocument.cookie)"
Str = re.replace(Str,"$1ocument cookie")
re.Pattern="(d)(ocument.write)"
Str = re.replace(Str,"$1ocument write")
re.Pattern="(s)(cript:)"
Str = re.replace(Str,"$1cript ")
re.Pattern="(s)(cript)"
Str = re.replace(Str,"$1cript")
re.Pattern="(o)(bject)"
Str = re.replace(Str,"$1bject")
re.Pattern="(a)(pplet)"
Str = re.replace(Str,"$1pplet")
re.Pattern="(e)(mbed)"
Str = re.replace(Str,"$1mbed")
Set re=Nothing
Str = Replace(Str, "&gt; ", "&gt; ")
Str = Replace(Str, "&lt;", "&lt;")
checkURL=Str
end function

’*************************************
’过滤文件名字
’*************************************
Function FixName(UpFileExt)
If IsEmpty(UpFileExt) Then Exit Function
FixName = Ucase(UpFileExt)
FixName = Replace(FixName,Chr(0),"")
FixName = Replace(FixName,".","")
FixName = Replace(FixName,"ASP","TXT")
FixName = Replace(FixName,"ASA","")
FixName = Replace(FixName,"ASPX","")
FixName = Replace(FixName,"CER","")
FixName = Replace(FixName,"CDX","")
FixName = Replace(FixName,"HTR","")
End Function

’*************************************
’过滤特殊字符
’*************************************
Function CheckStr(byVal ChkStr)
Dim Str:Str=ChkStr
If IsNull(Str) Then
CheckStr = ""
Exit Function
End If
Str = Replace(Str, "&amp;", "&amp;")
Str = Replace(Str,"’","’")
Str = Replace(Str,"""",""")
Dim re
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
re.Pattern="(w)(here)"
Str = re.replace(Str,"$1here")
re.Pattern="(s)(elect)"
Str = re.replace(Str,"$1elect")
re.Pattern="(i)(nsert)"
Str = re.replace(Str,"$1nsert")
re.Pattern="(c)(reate)"
Str = re.replace(Str,"$1reate")
re.Pattern="(d)(rop)"
Str = re.replace(Str,"$1rop")
re.Pattern="(a)(lter)"
Str = re.replace(Str,"$1lter")
re.Pattern="(d)(elete)"
Str = re.replace(Str,"$1elete")
re.Pattern="(u)(pdate)"
Str = re.replace(Str,"$1pdate")
re.Pattern="(s)(or)"
Str = re.replace(Str,"$1or")
Set re=Nothing
CheckStr=Str
End Function

’*************************************
’恢复特殊字符
’*************************************
Function UnCheckStr(ByVal Str)
If IsNull(Str) Then
UnCheckStr = ""
Exit Function
End If
Str = Replace(Str,"’","’")
Str = Replace(Str,""","""")
Dim re
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
re.Pattern="(w)(here)"
str = re.replace(str,"$1here")
re.Pattern="(s)(elect)"
str = re.replace(str,"$1elect")
re.Pattern="(i)(nsert)"
str = re.replace(str,"$1nsert")
re.Pattern="(c)(reate)"
str = re.replace(str,"$1reate")
re.Pattern="(d)(rop)"
str = re.replace(str,"$1rop")
re.Pattern="(a)(lter)"
str = re.replace(str,"$1lter")
re.Pattern="(d)(elete)"
str = re.replace(str,"$1elete")
re.Pattern="(u)(pdate)"
str = re.replace(str,"$1pdate")
re.Pattern="(s)(or)"
Str = re.replace(Str,"$1or")
Set re=Nothing
Str = Replace(Str, "&amp;", "&amp;")
UnCheckStr=Str
End Function

’*************************************
’转换HTML代码
’*************************************
Function HTMLEncode(ByVal reString)
Dim Str:Str=reString
If Not IsNull(Str) Then
Str = Replace(Str, "&gt; ", "&gt; ")
Str = Replace(Str, "&lt;", "&lt;")
Str = Replace(Str, CHR(9), " ")
Str = Replace(Str, CHR(32), " ")
Str = Replace(Str, CHR(39), "’")
Str = Replace(Str, CHR(34), """)
Str = Replace(Str, CHR(13), "")
Str = Replace(Str, CHR(10), "&lt;br/&gt; ")
HTMLEncode = Str
End If
End Function

’*************************************
’反转换HTML代码
’*************************************
Function HTMLDecode(ByVal reString)
Dim Str:Str=reString
If Not IsNull(Str) Then
Str = Replace(Str, "&gt; ", "&gt; ")
Str = Replace(Str, "&lt;", "&lt;")
Str = Replace(Str, " ", CHR(9))
Str = Replace(Str, " ", CHR(32))
Str = Replace(Str, "’", CHR(39))
Str = Replace(Str, """, CHR(34))
Str = Replace(Str, "", CHR(13))
Str = Replace(Str, "&lt;br/&gt; ", CHR(10))
HTMLDecode = Str
End If
End Function

’*************************************
’恢复&amp;字符
’*************************************
function ClearHTML(ByVal reString)
Dim Str:Str=reString
If Not IsNull(Str) Then
Str = Replace(Str, "&amp;", "&amp;")
ClearHTML = Str
End If
End Function

’*************************************
’过滤textarea
’*************************************
Function UBBFilter(ByVal reString)
Dim Str:Str=reString
If Not IsNull(Str) Then
Str = Replace(Str, "&lt;/textarea&gt;", "&lt;/textarea&gt;") '---不懂这个函数什么意思，汗~
UBBFilter = Str
End If
End Function

’*************************************
’过滤HTML代码
’*************************************
Function EditDeHTML(byVal Content)
EditDeHTML=Content
IF Not IsNull(EditDeHTML) Then
EditDeHTML=UnCheckStr(EditDeHTML)
EditDeHTML=Replace(EditDeHTML,"&amp;","&amp;")
EditDeHTML=Replace(EditDeHTML,"&lt;","&lt;")
EditDeHTML=Replace(EditDeHTML,"&gt; ","&gt; ")
EditDeHTML=Replace(EditDeHTML,chr(34),""")
EditDeHTML=Replace(EditDeHTML,chr(39),"’")
End IF
End Function

’*************************************
’防止外部提交
’*************************************
function ChkPost()
dim server_v1,server_v2
chkpost=false
server_v1=Cstr(Request.ServerVariables("HTTP_REFERER"))
server_v2=Cstr(Request.ServerVariables("SERVER_NAME"))
If Mid(server_v1,8,Len(server_v2))&lt;&gt; server_v2 then
chkpost=False
else
chkpost=True
end If
end function

’*************************************
’IP过滤
’*************************************
function MatchIP(IP)
on error resume next
MatchIP=false
Dim SIp,SplitIP
for each SIp in FilterIP
SIp=replace(SIp,"*","d*")
SplitIP=split(SIp,".")
Dim re, strMatchs,strIP
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
re.Pattern="("&amp;SplitIP(0)"|).""("&amp;SplitIP(1)"|).""("&amp;SplitIP(2)"|).""("&amp;SplitIP(3)"|)"
Set strMatchs=re.Execute(IP)
strIP=strMatchs(0).SubMatches(0) &amp; "." &amp; strMatchs(0).SubMatches(1)&amp; "." &amp; strMatchs(0).SubMatches(2)&amp; "." &amp; strMatchs(0).SubMatches(3)
if strIP=IP then MatchIP=true:exit function
Set strMatchs=Nothing
Set re=Nothing
next
end function

’*************************************
’获得注册码
’*************************************
Function getcode()
getcode= "&lt;img src=""getcode.asp"" alt="""" style=""margin-right:40px;""/&gt; "
End Function

’*************************************
’限制上传文件类型
’*************************************
Function IsvalidFile(File_Type)
IsvalidFile = False
Dim GName
For Each GName in UP_FileType
If File_Type = GName Then
IsvalidFile = True
Exit For
End If
Next
End Function

’*************************************
’检测是否只包含英文和数字
’*************************************
Function IsValidChars(str)
Dim re,chkstr
Set re=new RegExp
re.IgnoreCase =true
re.Global=True
re.Pattern="[^_.a-zA-Zd]"
IsValidChars=True
chkstr=re.Replace(str,"<a href="http://www.code-123.com/">http://www.code.com</a>") '不懂这个什么含义？！
if chkstr&lt;&gt; str then IsValidChars=False
set re=nothing
End Function

’*************************************
’检测是否只包含英文和数字
’*************************************
Function IsvalidValue(ArrayN,Str)
IsvalidValue = false
Dim GName
For Each GName in ArrayN
If Str = GName Then
IsvalidValue = true
Exit For
End If
Next
End Function

’*************************************
’检测是否有效的数字
’*************************************
Function IsInteger(Para)
IsInteger=False
If Not (IsNull(Para) Or Trim(Para)="" Or Not IsNumeric(Para)) Then
IsInteger=True
End If
End Function

’*************************************
’用户名检测
’*************************************
Function IsValidUserName(byVal UserName)
on error resume next
Dim i,c
Dim VUserName
IsValidUserName = True
For i = 1 To Len(UserName)
c = Lcase(Mid(UserName, i, 1))
If InStr("$!&lt;&gt; <a href="mailto:?#^%@~`&amp;*();:+=’">?#^%@~`&amp;*();:+=’</a>"" ", c) &gt; 0 Then
IsValidUserName = False
Exit Function
End IF
Next
For Each VUserName in Register_UserName
If UserName = VUserName Then
IsValidUserName = False
Exit For
End If
Next
End Function

’*************************************
’分页函数
’--CB~乱，不知道是否可行~
’*************************************
dim FirstShortCut,ShortCut
FirstShortCut=false
Function MultiPage(Numbers,Perpage,Curpage,Url_Add,aname,Style)
CurPage=Int(Curpage)
Numbers=Int(Numbers)
Dim URL
URL=Request.ServerVariables("Script_Name")&amp;Url_Add
MultiPage=""
Dim Page,Offset,PageI
If Int(Numbers)&gt; Int(PerPage) Then
Page=9
Offset=4
Dim Pages,FromPage,ToPage
If Numbers Mod Cint(Perpage)=0 Then
Pages=Int(Numbers/Perpage)
Else
Pages=Int(Numbers/Perpage)+1
End If
FromPage=Curpage-Offset
ToPage=Curpage+Page-Offset-1
If Page&gt; Pages Then
FromPage=1
ToPage=Pages
Else
If FromPage&lt;1 Then
Topage=Curpage+1-FromPage
FromPage=1
If (ToPage-FromPage)&lt;Page And (ToPage-FromPage)&lt;Pages Then ToPage=Page
ElseIF Topage&gt; Pages Then
FromPage =Curpage-Pages +ToPage
ToPage=Pages
If (ToPage-FromPage)&lt;Page And (ToPage-FromPage)&lt;Pages Then FromPage=Pages-Page+1
End If
End If
MultiPage="&lt;divpage"" style="""&amp;Style"""&gt; &lt;ul&gt; "
if Curpage&lt;&gt; 1 then MultiPage=MultiPage&amp;"&lt;liPageL""&gt; &lt;a href="""&amp;Url&amp;"page=1""PageLbutton"" title=""第一页""&gt; &lt;/a&gt; &lt;/li&gt; "
MultiPage=MultiPage"&lt;lipageNumber""&gt; "
if Curpage&lt;&gt; 1 then MultiPage=MultiPage"&lt;a href="""&amp;Url"page=1"" title=""第一页"" style=""text-decoration:none""&gt; &lt;&lt;/a&gt; | "
if not FirstShortCut then ShortCut=" accesskey="",""" else ShortCut=""
if Curpage&lt;&gt; 1 then MultiPage=MultiPage"&lt;a href="""&amp;Url"page="&amp;CurPage-1""" title=""上一页"" style=""text-decoration:none;"""&amp;ShortCut"&gt; &lt;/a&gt; "
For PageI=FromPage TO ToPage
If PageI&lt;&gt; CurPage Then
MultiPage=MultiPage"&lt;a href="""&amp;Url"page="&amp;PageI&amp;aname"""&gt; "&amp;PageI"&lt;/a&gt; | "
Else
MultiPage=MultiPage"&lt;strong&gt; "&amp;PageI"&lt;/strong&gt; "
if PageI&lt;&gt; Pages then MultiPage=MultiPage" | "
End If
Next
if not FirstShortCut then ShortCut=" accesskey="".""" else ShortCut=""
if Curpage&lt;&gt; pages then MultiPage=MultiPage"&lt;a href="""&amp;Url"page="&amp;CurPage+1""" title=""下一页"" style=""text-decoration:none"""&amp;ShortCut"&gt; &lt;/a&gt; "
if Curpage&lt;&gt; pages then MultiPage=MultiPage"&lt;a href="""&amp;Url"page="&amp;Pages&amp;aname""" title=""最后一页"" style=""text-decoration:none""&gt; &gt; &lt;/a&gt; "
MultiPage=MultiPage"&lt;/li&gt; "
’If Int(Pages)&gt; Int(Page) Then
’ MultiPage=MultiPage&amp;"&lt;li&gt; ...&lt;/li&gt; &lt;li&gt; &lt;a href="""&amp;Url&amp;"page="&amp;Pages&amp;aname&amp;"""&gt; "&amp;pages&amp;"&lt;/a&gt; &lt;/li&gt; "
’End If
’if Curpage&lt;&gt; pages then MultiPage=MultiPage&amp;"&lt;liPageR""&gt; &lt;a href="""&amp;Url&amp;"page="&amp;Pages&amp;aname&amp;"""PageRbutton"" title=""最后一页""&gt; &lt;/a&gt; &lt;/li&gt; "
MultiPage=MultiPage"&lt;/ul&gt; &lt;/div&gt; "
’ End If
FirstShortCut=true
End Function

’*************************************
’切割内容 - 按行分割
’--CB~乱，不知道是否可行~
’*************************************
Function SplitLines(byVal Content,byVal ContentNums)
Dim ts,i,l
ContentNums=int(ContentNums)
If IsNull(Content) Then Exit Function
i=1
ts = 0
For i=1 to Len(Content)
l=Lcase(Mid(Content,i,5))
If l="&lt;br/&gt; " Then
ts=ts+1
End If
l=Lcase(Mid(Content,i,4))
If l="
" Then
ts=ts+1
End If
l=Lcase(Mid(Content,i,3))
If l="&lt;p&gt; " Then
ts=ts+1
End If
If ts&gt; ContentNums Then Exit For
Next
If ts&gt; ContentNums Then
Content=Left(Content,i-1)
End If
SplitLines=Content
End Function

’*************************************
’切割内容 - 按字符分割
’*************************************
Function CutStr(byVal Str,byVal StrLen)
Dim l,t,c,i
If IsNull(Str) Then CutStr="":Exit Function
l=Len(str)
StrLen=int(StrLen)
t=0
For i=1 To l
c=Asc(Mid(str,i,1))
If c&lt;0 Or c&gt; 255 Then t=t+2 Else t=t+1
IF t&gt; =StrLen Then
CutStr=left(Str,i)"..."
Exit For
Else
CutStr=Str
End If
Next
End Function

’*************************************
’删除引用标签
’*************************************
Function DelQuote(strContent)
If IsNull(strContent) Then Exit Function
Dim re
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
re.Pattern="[quote](.[^]]*?)[/quote]"
strContent= re.Replace(strContent,"")
re.Pattern="[quote=(.[^]]*)](.[^]]*?)[/quote]"
strContent= re.Replace(strContent,"")
Set re=Nothing
DelQuote=strContent
End Function

’*************************************
’获取客户端IP
’*************************************
function getIP()
dim strIP,IP_Ary,strIP_list
strIP_list=Replace(Request.ServerVariables("HTTP_X_FORWARDED_FOR"),"’","")

If InStr(strIP_list,",")&lt;&gt; 0 Then
IP_Ary = Split(strIP_list,",")
strIP = IP_Ary(0)
Else
strIP = strIP_list
End IF

If strIP=Empty Then strIP=Replace(Request.ServerVariables("REMOTE_ADDR"),"’","")
getIP=strIP
End Function

’*************************************
’获取客户端浏览器信息
’*************************************
function getBrowser(strUA)
dim arrInfo,strType,temp1,temp2
strType=""
strUA=LCase(strUA)
arrInfo=Array("Unkown","Unkown")
’浏览器判断
if Instr(strUA,"mozilla")&gt; 0 then arrInfo(0)="Mozilla"
if Instr(strUA,"icab")&gt; 0 then arrInfo(0)="iCab"
if Instr(strUA,"lynx")&gt; 0 then arrInfo(0)="Lynx"
if Instr(strUA,"links")&gt; 0 then arrInfo(0)="Links"
if Instr(strUA,"elinks")&gt; 0 then arrInfo(0)="ELinks"
if Instr(strUA,"jbrowser")&gt; 0 then arrInfo(0)="JBrowser"
if Instr(strUA,"konqueror")&gt; 0 then arrInfo(0)="konqueror"
if Instr(strUA,"wget")&gt; 0 then arrInfo(0)="wget"
if Instr(strUA,"ask jeeves")&gt; 0 or Instr(strUA,"teoma")&gt; 0 then arrInfo(0)="Ask Jeeves/Teoma"
if Instr(strUA,"wget")&gt; 0 then arrInfo(0)="wget"
if Instr(strUA,"opera")&gt; 0 then arrInfo(0)="opera"

if Instr(strUA,"gecko")&gt; 0 then
strType="[Gecko]"
arrInfo(0)="Mozilla"
if Instr(strUA,"aol")&gt; 0 then arrInfo(0)="AOL"
if Instr(strUA,"netscape")&gt; 0 then arrInfo(0)="Netscape"
if Instr(strUA,"firefox")&gt; 0 then arrInfo(0)="FireFox"
if Instr(strUA,"chimera")&gt; 0 then arrInfo(0)="Chimera"
if Instr(strUA,"camino")&gt; 0 then arrInfo(0)="Camino"
if Instr(strUA,"galeon")&gt; 0 then arrInfo(0)="Galeon"
if Instr(strUA,"k-meleon")&gt; 0 then arrInfo(0)="K-Meleon"
arrInfo(0)=arrInfo(0)+strType
end if

if Instr(strUA,"bot")&gt; 0 or Instr(strUA,"crawl")&gt; 0 then
strType="[Bot/Crawler]"
arrInfo(0)=""
if Instr(strUA,"grub")&gt; 0 then arrInfo(0)="Grub"
if Instr(strUA,"googlebot")&gt; 0 then arrInfo(0)="GoogleBot"
if Instr(strUA,"msnbot")&gt; 0 then arrInfo(0)="MSN Bot"
if Instr(strUA,"slurp")&gt; 0 then arrInfo(0)="Yahoo! Slurp"
arrInfo(0)=arrInfo(0)+strType
end if

if Instr(strUA,"applewebkit")&gt; 0 then
strType="[AppleWebKit]"
arrInfo(0)=""
if Instr(strUA,"omniweb")&gt; 0 then arrInfo(0)="OmniWeb"
if Instr(strUA,"safari")&gt; 0 then arrInfo(0)="Safari"
arrInfo(0)=arrInfo(0)+strType
end if

if Instr(strUA,"msie")&gt; 0 then
strType="[MSIE"
temp1=mid(strUA,(Instr(strUA,"msie")+4),6)
temp2=Instr(temp1,";")
temp1=left(temp1,temp2-1)
strType=strType &amp; temp1 "]"
arrInfo(0)="Internet Explorer"
if Instr(strUA,"msn")&gt; 0 then arrInfo(0)="MSN"
if Instr(strUA,"aol")&gt; 0 then arrInfo(0)="AOL"
if Instr(strUA,"webtv")&gt; 0 then arrInfo(0)="WebTV"
if Instr(strUA,"myie2")&gt; 0 then arrInfo(0)="MyIE2"
if Instr(strUA,"maxthon")&gt; 0 then arrInfo(0)="Maxthon"
if Instr(strUA,"gosurf")&gt; 0 then arrInfo(0)="GoSurf"
if Instr(strUA,"netcaptor")&gt; 0 then arrInfo(0)="NetCaptor"
if Instr(strUA,"sleipnir")&gt; 0 then arrInfo(0)="Sleipnir"
if Instr(strUA,"avant browser")&gt; 0 then arrInfo(0)="AvantBrowser"
if Instr(strUA,"greenbrowser")&gt; 0 then arrInfo(0)="GreenBrowser"
if Instr(strUA,"slimbrowser")&gt; 0 then arrInfo(0)="SlimBrowser"
arrInfo(0)=arrInfo(0)+strType
end if

’操作系统判断
if Instr(strUA,"windows")&gt; 0 then arrInfo(1)="Windows"
if Instr(strUA,"windows ce")&gt; 0 then arrInfo(1)="Windows CE"
if Instr(strUA,"windows 95")&gt; 0 then arrInfo(1)="Windows 95"
if Instr(strUA,"win98")&gt; 0 then arrInfo(1)="Windows 98"
if Instr(strUA,"windows 98")&gt; 0 then arrInfo(1)="Windows 98"
if Instr(strUA,"windows 2000")&gt; 0 then arrInfo(1)="Windows 2000"
if Instr(strUA,"windows xp")&gt; 0 then arrInfo(1)="Windows XP"

if Instr(strUA,"windows nt")&gt; 0 then
arrInfo(1)="Windows NT"
if Instr(strUA,"windows nt 5.0")&gt; 0 then arrInfo(1)="Windows 2000"
if Instr(strUA,"windows nt 5.1")&gt; 0 then arrInfo(1)="Windows XP"
if Instr(strUA,"windows nt 5.2")&gt; 0 then arrInfo(1)="Windows 2003"
end if
if Instr(strUA,"x11")&gt; 0 or Instr(strUA,"unix")&gt; 0 then arrInfo(1)="Unix"
if Instr(strUA,"sunos")&gt; 0 or Instr(strUA,"sun os")&gt; 0 then arrInfo(1)="SUN OS"
if Instr(strUA,"powerpc")&gt; 0 or Instr(strUA,"ppc")&gt; 0 then arrInfo(1)="PowerPC"
if Instr(strUA,"macintosh")&gt; 0 then arrInfo(1)="Mac"
if Instr(strUA,"mac osx")&gt; 0 then arrInfo(1)="MacOSX"
if Instr(strUA,"freebsd")&gt; 0 then arrInfo(1)="FreeBSD"
if Instr(strUA,"linux")&gt; 0 then arrInfo(1)="Linux"
if Instr(strUA,"palmsource")&gt; 0 or Instr(strUA,"palmos")&gt; 0 then arrInfo(1)="PalmOS"
if Instr(strUA,"wap ")&gt; 0 then arrInfo(1)="WAP"

’arrInfo(0)=strUA
getBrowser=arrInfo
end function

’*************************************
’计算随机数
’*************************************
function randomStr(intLength)
dim strSeed,seedLength,pos,str,i
strSeed = "abcdefghijklmnopqrstuvwxyz1234567890"
seedLength=len(strSeed)
str=""
Randomize
for i=1 to intLength
str=str+mid(strSeed,int(seedLength*rnd)+1,1)
next
randomStr=str
end function

’*************************************
’自动闭合UBB
’*************************************
function closeUBB(strContent)
dim arrTags,i,OpenPos,ClosePos,re,strMatchs,j,Match
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
arrTags=array("code","quote","list","color","align","font","size","b","i","u","html")
for i=0 to ubound(arrTags)
OpenPos=0
ClosePos=0

re.Pattern="["+arrTags(i)+"(=[^[]]+|)]"
Set strMatchs=re.Execute(strContent)
For Each Match in strMatchs
OpenPos=OpenPos+1
next
re.Pattern="[/"+arrTags(i)+"]"
Set strMatchs=re.Execute(strContent)
For Each Match in strMatchs
ClosePos=ClosePos+1
next
for j=1 to OpenPos-ClosePos
strContent=strContent+"[/"+arrTags(i)+"]"
next
next
closeUBB=strContent
end function

’*************************************
’自动闭合HTML
’*************************************
function closeHTML(strContent)
dim arrTags,i,OpenPos,ClosePos,re,strMatchs,j,Match
Set re=new RegExp
re.IgnoreCase =True
re.Global=True
arrTags=array("p","div","span","table","ul","font","b","u","i","h1","h2","h3","h4","h5","h6")
for i=0 to ubound(arrTags)
OpenPos=0
ClosePos=0

re.Pattern="&lt;"+arrTags(i)+"( [^&lt;&gt; ]+|)&gt; "
Set strMatchs=re.Execute(strContent)
For Each Match in strMatchs
OpenPos=OpenPos+1
next
re.Pattern="&lt;/"+arrTags(i)+"&gt; "
Set strMatchs=re.Execute(strContent)
For Each Match in strMatchs
ClosePos=ClosePos+1
next
for j=1 to OpenPos-ClosePos
strContent=strContent+"&lt;/"+arrTags(i)+"&gt; "
next
next
closeHTML=strContent
end function

’*************************************
’读取文件
’*************************************
Function LoadFromFile(ByVal File)
Dim objStream
Dim RText
RText=array(0,"")
On Error Resume Next
Set objStream = Server.CreateObject("ADODB.Stream")
If Err Then
RText=array(Err.Number,Err.Description)
LoadFromFile=RText
Err.Clear
exit function
End If
With objStream
.Type = 2
.Mode = 3
.Open
.Charset = "utf-8"
.Position = objStream.Size
.LoadFromFile Server.MapPath(File)
If Err.Number&lt;&gt; 0 Then
RText=array(Err.Number,Err.Description)
LoadFromFile=RText
Err.Clear
exit function
End If
RText=array(0,.ReadText)
.Close
End With
LoadFromFile=RText
Set objStream = Nothing
End Function

’*************************************
’保存文件
’*************************************
Function SaveToFile(ByVal strBody,ByVal File)
Dim objStream
Dim RText
RText=array(0,"")
On Error Resume Next
Set objStream = Server.CreateObject("ADODB.Stream")
If Err Then
RText=array(Err.Number,Err.Description)
Err.Clear
exit function
End If
With objStream
.Type = 2
.Open
.Charset = "utf-8"
.Position = objStream.Size
.WriteText = strBody
.SaveToFile Server.MapPath(File),2
.Close
End With
RText=array(0,"保存文件成功!")
SaveToFile=RText
Set objStream = Nothing
End Function

’*************************************
’数据库添加修改操作
’*************************************
function DBQuest(table,DBArray,Action)
dim AddCount,TempDB,i,v
if Action&lt;&gt; "insert" or Action&lt;&gt; "update" then Action="insert"
if Action="insert" then v=2 else v=3
if not IsArray(DBArray) then
DBQuest=-1
exit function
else
Set TempDB=Server.CreateObject("ADODB.RecordSet")
On Error Resume Next
TempDB.Open table,Conn,1,v
if err then
DBQuest=-2
exit function
end if
if Action="insert" then TempDB.addNew
AddCount=UBound(DBArray,1)
for i=0 to AddCount
TempDB(DBArray(i)(0))=DBArray(i)(1)
next
TempDB.update
TempDB.close
set TempDB=nothing
DBQuest=0
end if
end Function

’*************************************
’检测系统组件是否安装
’*************************************
Function CheckObjInstalled(strClassString)
On Error Resume Next
Dim Temp
Err = 0
Dim TmpObj
Set TmpObj = Server.CreateObject(strClassString)
Temp = Err
IF Temp = 0 OR Temp = -2147221477 Then
CheckObjInstalled=true
ElseIF Temp = 1 OR Temp = -2147221005 Then
CheckObjInstalled=false
End IF
Err.Clear
Set TmpObj = Nothing
Err = 0
End Function

’*************************************
’判断服务器Microsoft.XMLDOM
’*************************************
Function getXMLDOM
On Error Resume Next
Dim Temp
getXMLDOM="Microsoft.XMLDOM"
Err = 0
Dim TmpObj
Set TmpObj = Server.CreateObject(getXMLDOM)
Temp = Err
IF Temp = 1 OR Temp = -2147221005 Then
getXMLDOM="Msxml2.DOMDocument.5.0"
End IF
Err.Clear
Set TmpObj = Nothing
Err = 0
end function

’*************************************
’判断服务器MSXML2.ServerXMLHTTP
’*************************************
Function getXMLHTTP
On Error Resume Next
Dim Temp
getXMLHTTP="MSXML2.ServerXMLHTTP"
Err = 0
Dim TmpObj
Set TmpObj = Server.CreateObject(getXMLHTTP)
Temp = Err
IF Temp = 1 OR Temp = -2147221005 Then
getXMLHTTP="Msxml2.ServerXMLHTTP.5.0"
End IF
Err.Clear
Set TmpObj = Nothing
Err = 0
end function

’*********************************************************
’ 目的： 检查正则式
’ 输入： id
’ 返回： 成功为True
’*********************************************************
Function CheckRegExp(source,para)

If para="[username]" Then
para="^[.A-Za-z0-9u4e00-u9fa5]+$"
End If
If para="[password]" Then
para="^[a-z0-9]+$"
End If
If para="[email]" Then
para="^([0-9a-zA-Z]([-.w]*[0-9a-zA-Z])*@([0-9a-zA-Z][-w]*.)+[a-zA-Z]*)$"
End If
If para="[homepage]" Then
para="^[a-zA-Z]+://[a-zA-z0-9-./]+?/*$"
End If
If para="[nojapan]" Then
para="[u3040-u30ff]+"
End If
If para="[guid]" Then
para="^w{8}-w{4}-w{4}-w{4}-w{12}$"
End If

Dim re
Set re = New RegExp
re.Global = True
re.Pattern = para
re.IgnoreCase = False
CheckRegExp = re.Test(source)

End Function

’**********************************************
’获取在线人数
’**********************************************
function getOnline
getOnline=1
if len(Application(space_CookieName"_onlineCount"))&gt; 0 then
if DateDiff("s",Application(space_CookieName"_userOnlineCountTime"),now())&gt; 60 then
Application.Lock()
Application(space_CookieName"_online")=Application(space_CookieName"_onlineCount")
Application(space_CookieName"_onlineCount")=1
Application(space_CookieName"_onlineCountKey")=randStr(2)
Application(space_CookieName"_userOnlineCountTime")=now()
Application.Unlock()
else
if Session(space_CookieName"userOnlineKey")&lt;&gt; Application(space_CookieName"_onlineCountKey") then
Application.Lock()
Application(space_CookieName"_onlineCount")=Application(space_CookieName"_onlineCount")+1
Application.Unlock()
Session(space_CookieName"userOnlineKey")=Application(space_CookieName"_onlineCountKey")
end if
end if
else
Application.Lock
Application(space_CookieName"_online")=1
Application(space_CookieName"_onlineCount")=1
Application(space_CookieName"_onlineCountKey")=randStr(2)
Application(space_CookieName"_userOnlineCountTime")=now()
Application.Unlock
end if
getOnline=Application(space_CookieName"_online")
end Function

2014.04.01

'函数名：IsObjInstalled
'作  用：检查组件是否已经安装
'参  数：strClassString ----组件名
'返回值：True  ----已经安装
'       False ----没有安装
'***************************************************
Function IsObjInstalled(strClassString)
On Error Resume Next
IsObjInstalled = False
Err = 0
Dim xTestObj
Set xTestObj = Server.CreateObject(strClassString)
If 0 = Err Then IsObjInstalled = True
Set xTestObj = Nothing
Err = 0
End Function

response.write "组件的存在："
response.write IsObjInstalled("adodb.recordset")
response.end