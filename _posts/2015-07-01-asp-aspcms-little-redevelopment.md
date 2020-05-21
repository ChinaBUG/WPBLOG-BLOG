---
ID: 4093
post_title: ASP-aspcms的一点点二次开发
author: ChinaBUG
post_excerpt: |
  定制目标：我们的目标是，只要定义了链接字段之后就按照定义的内容输出，而不去管它的类型是否是链接，避开设置为链接类型后无法管理的情况。
  定制目标：将分页导航的样式定制成数字类型的。
layout: post
permalink: >
  http://blog.ipodmp.com/2015/07/asp-aspcms-little-redevelopment.html
published: true
post_date: 2015-07-01 09:45:23
---
朋友的站我都是用aspcms这套系统来做网站的，所以难免会为了功能做一些定制的改动，但是，这套系统又经常性的有点缺陷漏洞，哎，免费的没办法。

记录一下我的定制修改吧，省的升级之后忘记到底修改了那些。

~.~.~.~.~.~
<ul>
	<li>问题说明：
系统定义菜单的第7种类型是链接，如果设定了就输出定义的链接，但是这个链接是需要两个条件才会输出我们定义的链接地址
<ol>
	<li>类型必须是链接</li>
	<li>定义的链接是以http开头的</li>
</ol>
</li>
	<li>定制目标：我们的目标是，只要定义了链接字段之后就按照定义的内容输出，而不去管它的类型是否是链接，避开设置为链接类型后无法管理的情况。</li>
	<li>目标版本：AspCms v2.5.7 20150120</li>
	<li>目标文件：/inc/AspCms_MainClass.asp</li>
</ul>
------------------------------------------
'获取导航栏链接
Function getSortLink(sortType, sortID, sortUrl, sortFolder, sortFileName, GroupID, Exclusive)
sortFolder=replace(repnull(sortFolder), "{sitepath}", sitePath)
sortFileName=replace(repnull(sortFileName), "{sortid}", sortID)
sortFileName=replace(sortFileName, "{page}", "1")
if sortType="7" then
if isurl(sortUrl) then
getSortLink=sortUrl
else
if isnul(pagemode) then
getSortLink=sitePath&amp;sortUrl
else
getSortLink=sitePath&amp;"/"&amp;pagemode&amp;sortUrl
end if
end if
else
if runMode=1 and viewNoRight(GroupID, Exclusive) then
getSortLink=sortFolder&amp;sortFileName&amp;fileExt
else
' 2015.06.29 ChinaBUG 如果有设置链接字段则直接输出
if len(sortUrl)&gt;0 then
if isurl(sortUrl) then
getSortLink=sortUrl
else
if isnul(pagemode) then
getSortLink=sitePath&amp;sortUrl
else
getSortLink=sitePath&amp;"/"&amp;pagemode&amp;sortUrl
end if
end if
else
Select  case sortType
case "1"
getSortLink=sitePath&amp;setting.languagePath&amp;""&amp;"about"&amp;"/?"&amp;sortID&amp;fileExt
case else
getSortLink=sitePath&amp;setting.languagePath&amp;""&amp;"list"&amp;"/?"&amp;sortID&amp;"_1"&amp;fileExt
End Select
end if
end if
end if
End Function
~.~.~.~.~.~
<ul>
	<li>问题说明：
系统带有的分页导航样式结构是不是你不喜欢呢？希望换成都是数字的？</li>
	<li>定制目标：将分页导航的样式定制成数字类型的。</li>
	<li>目标版本：AspCms v2.5.3 0619</li>
	<li>目标文件：
<ol>
	<li>/inc/AspCms_CommonFun.asp</li>
	<li>/inc/AspCms_Language.asp</li>
</ol>
</li>
</ul>
------------------------------------------
<ul>
	<li>/inc/AspCms_CommonFun.asp</li>
</ul>
'分页两侧
'2015.02.09 ChinaBUG
Function pageNumberLinkInfo(Byval currentPage,Byval pageListLen,Byval totalPages,Byval linkType,Byval sortid, Byval showType)
dim pageNumber,pagesStr,i,pageNumberInfo,firstPageLink,lastPagelink,nextPagelink,finalPageLink,p,jumppagelink,currentpagestr

pageNumber=makePageNumber(currentPage,pageListLen,totalPages,linkType,sortid,showType)
dim searchtype,keys,tag
searchtype=filterPara(getForm("searchtype","get"))
keys=filterPara(getForm("keys","get"))
tag=filterPara(getForm("tag","get"))
if currentPage=1 then
firstPageLink="&lt;span class='nolink string-01'&gt;"&amp;str_01&amp;"&lt;/span&gt;" : lastPagelink="&lt;span class='nolink string-03'&gt;"&amp;str_03&amp;"&lt;/span&gt;"
else
if linkType="gbooklist" then
firstPageLink="&lt;a class='string-01' href='?"&amp;sortid&amp;"_1"&amp;FileExt&amp;"'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : lastPagelink="&lt;a class='string-03' href='?"&amp;sortid&amp;"_"&amp;currentPage-1&amp;FileExt&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
elseif linkType="userbuylist" then
firstPageLink="&lt;a class='string-01' href='?page=1'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : lastPagelink="&lt;a class='string-03' href='?page="&amp;currentPage-1&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
elseif linkType="searchlist" then
firstPageLink="&lt;a class='string-01' href='?page=1&amp;keys="&amp;keys&amp;"&amp;searchtype="&amp;searchtype&amp;"'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : lastPagelink="&lt;a class='string-03' href='?page="&amp;currentPage-1&amp;"&amp;keys="&amp;keys&amp;"&amp;searchtype="&amp;searchtype&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
elseif showType="tags" then
firstPageLink="&lt;a class='string-01' href='?page=1'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : lastPagelink="&lt;a href='?page="&amp;currentPage-1&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
elseif showType="taglist" then
firstPageLink="&lt;a class='string-01' href='?page=1&amp;tag="&amp;tag&amp;"'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : lastPagelink="&lt;a href='?page="&amp;currentPage-1&amp;"&amp;tag="&amp;tag&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
else
if runMode=0 then
firstPageLink="&lt;a href='?"&amp;sortid&amp;"_1"&amp;FileExt&amp;"'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : if currentPage&gt;2 then lastPagelink="&lt;a href='?"&amp;sortid&amp;"_"&amp;currentPage-1&amp;FileExt&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;" : else lastPagelink="&lt;a href='?"&amp;sortid&amp;"_1"&amp;FileExt&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
else
firstPageLink="&lt;a href='"&amp;replace(showType, "{page}", 1)&amp;"'&gt;"&amp;str_01&amp;"&lt;/a&gt;" : if currentPage&gt;2 then lastPagelink="&lt;a href='"&amp;replace(showType, "{page}", currentPage-1)&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;" : else lastPagelink="&lt;a href='"&amp;replace(showType, "{page}", 1)&amp;"'&gt;"&amp;str_03&amp;"&lt;/a&gt;"
end if
end if
end if
if linkType&lt;&gt;"gbooklist" and linkType&lt;&gt;"userbuylist" and linkType&lt;&gt;"searchlist" and showType&lt;&gt;"tags" and showType&lt;&gt;"taglist" then
jumppagelink=str_17&amp;" &lt;SELECT NAME=""select"" ONCHANGE=""var jmpURL=this.options[this.selectedIndex].value ; if(jmpURL!='') {window.location=jmpURL;} else {this.selectedIndex=0 ;}"" &gt;"

for p=1 to totalPages
currentpagestr=""
if currentPage=p then currentpagestr=" selected=selected"
if runMode=0 then
jumppagelink=jumppagelink&amp;"&lt;option value=?"&amp;sortid&amp;"_"&amp;p&amp;FileExt&amp;"  "&amp;currentpagestr&amp;"&gt;"&amp;p&amp;"&lt;/option&gt;"

else
jumppagelink=jumppagelink&amp;"&lt;option value="&amp;replace(showType, "{page}", p)&amp;" "&amp;currentpagestr&amp;"&gt;"&amp;p&amp;"&lt;/option&gt;"

end if
next
jumppagelink=jumppagelink&amp;"&lt;/SELECT&gt;"

end if
if currentPage=totalPages then
nextPagelink="&lt;span class='nolink'&gt;"&amp;str_04&amp;"&lt;/span&gt;" : finalPageLink="&lt;span class='nolink'&gt;"&amp;str_02&amp;"&lt;/span&gt;"
else
if linkType="gbooklist" then
nextPagelink="&lt;a href='?"&amp;sortid&amp;"_"&amp;currentPage+1&amp;FileExt&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?"&amp;sortid&amp;"_"&amp;totalPages&amp;FileExt&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
elseif linkType="searchlist" then
nextPagelink="&lt;a href='?page="&amp;currentPage+1&amp;"&amp;keys="&amp;keys&amp;"&amp;searchtype="&amp;searchtype&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?page="&amp;totalPages&amp;"&amp;keys="&amp;keys&amp;"&amp;searchtype="&amp;searchtype&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
elseif linkType="userbuylist" then
nextPagelink="&lt;a href='?page="&amp;currentPage+1&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?page="&amp;totalPages&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
elseif showType="tags" then
nextPagelink="&lt;a href='?page="&amp;currentPage+1&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?page="&amp;totalPages&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
elseif showType="taglist" then
nextPagelink="&lt;a href='?page="&amp;currentPage+1&amp;"&amp;tag="&amp;tag&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?page="&amp;totalPages&amp;"&amp;tag="&amp;tag&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
else
if runMode=0 then
nextPagelink="&lt;a href='?"&amp;sortid&amp;"_"&amp;currentPage+1&amp;FileExt&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='?"&amp;sortid&amp;"_"&amp;totalPages&amp;FileExt&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
else
nextPagelink="&lt;a href='"&amp;replace(showType, "{page}", currentPage+1)&amp;"'&gt;"&amp;str_04&amp;"&lt;/a&gt;" : finalPageLink="&lt;a href='"&amp;replace(showType, "{page}", totalPages)&amp;"'&gt;"&amp;str_02&amp;"&lt;/a&gt;"
end if
end if
end if

'pageNumberInfo="&lt;span&gt; "&amp;str_06&amp;""&amp;totalPages&amp;" "&amp;str_07&amp;" "&amp;str_05&amp;":"&amp;currentPage&amp;"/"&amp;totalPages&amp;" "&amp;str_07&amp;"&lt;/span&gt;"&amp;firstPageLink&amp;lastPagelink&amp;pageNumber&amp;""&amp;nextPagelink&amp;""&amp;finalPagelink&amp;" "&amp;jumppagelink
pageNumberInfo =firstPageLink&amp;lastPagelink&amp;pageNumber&amp;""&amp;nextPagelink&amp;""&amp;finalPagelink
pageNumberLinkInfo=pageNumberInfo
End Function
<ul>
	<li>/inc/AspCms_Language.asp</li>
</ul>
err_01="数据库连接错误"
err_02="语言别名设置错误"
err_03="执行SQL语句错误"
err_04="st"&amp;"ream对象实例创建失败"
err_05="F"&amp;"SO对象实例创建失败"
err_06="加载文件失败"
err_07="数据列表未指定主键"
err_08="数据列表未指定表"
err_09="写入文件失败"
err_10="创建文件夹失败"
err_11="删除文件夹失败"
err_12="删除文件失败"
err_13="文件夹不存在"
err_14="移动文件夹失败"
err_15="请设置默认语言"
err_16="模板文件不存在"
err_17="您当前所在用户组无查看权限！"

str_01="&lt;&lt;"
str_02="&gt;&gt;"
str_03="&lt;"
str_04="&gt;"
str_05="页次"
str_06="共"
str_07="页"
str_08="对不起，该分类无任何记录"
str_09="对不起，关键字"
str_10=" 无任何记录"
str_11="您当前所在用户组无查看权限！"
str_12=""
str_13=""
str_14=""
str_15=""
str_16=""
str_17="转到"

newspageInfo(0)="<span style="color: red;"> 对不起，无任何内容 </span>"
channellistInfo(0)="<span style="color: red;"> 对不起，该分类无记录任何记录 </span>":channellistInfo(1)="指定分类错误"
searchlistInfo(0)="对不起，没有找到任何记录"
pageRunStr(0)="页面执行时间: ":pageRunStr(1)="秒 ":pageRunStr(2)="次数据查询"
~.~.~.~.~.~