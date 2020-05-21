---
ID: 2689
post_title: 'ASP-ASPCMS功能增强之{aspcms:cimages}标签怎么从图片组中去除过滤文章缩略图'
author: ChinaBUG
post_excerpt: |
  ASPcms在文章管理时可以上传设置“文章缩略图”，还能上传图片，细心的小朋友肯定可以看出来，这些图片是保存在一起的！
  如果你有分析数据库的话那么你会发现图片组保存在一个字段里，而文章缩略图也保存在另外一个字段里，并且文章缩略图的值还在图片组的字段里面有一个记录。
  也就是我们在ASPcms系统里面如果调用{aspcms:cimages}标签的话，显示出来的图片组里面包含文章缩略图！问题在于文章缩略图我们大部分情况下是不需要显示的，怎么办？
  DIY解决吧~
  打开inc\AspCms_templateFun.asp文件查找如下代码
  Function makeContentImages(sContent)
  在查找到的位置下方增加下面的变量定义
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/asp-aspcms-enhancements-of-aspcms-cimages-tags-how-to-remove-the-group-from-the-picture-filters-article-thumbnail.html
published: true
post_date: 2013-10-31 15:44:00
---
ASPcms在文章管理时可以上传设置“文章缩略图”，还能上传图片，细心的小朋友肯定可以看出来，这些图片是保存在一起的！

如果你有分析数据库的话那么你会发现图片组保存在一个字段里，而文章缩略图也保存在另外一个字段里，并且文章缩略图的值还在图片组的字段里面有一个记录。

也就是我们在ASPcms系统里面如果调用{aspcms:cimages}标签的话，显示出来的图片组里面包含文章缩略图！问题在于文章缩略图我们大部分情况下是不需要显示的，怎么办？

DIY解决吧~

打开inc\AspCms_templateFun.asp文件查找如下代码

Function makeContentImages(sContent)

在查找到的位置下方增加下面的变量定义
<pre>dim exc_IndexImage,img_index<strong></strong></pre>
往下找下面的代码：

m_maxcount = parseArr(match.SubMatches(0))("count")

增加下面的代码

exc_IndexImage = parseArr(match.SubMatches(0))("indeximage")

这个代码的含义就是我们前台模板调用的时候需要增加一个参数，参数名字就叫做indeximage。

<strong>PS：</strong>前台的调用代码{aspcms:cimages count=5 contentid=1 <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">indeximage=true</span></span>}......{/aspcms:cimages}

接着再找到

sql = "select imagepath from {prefix}Content where ContentID=" &amp; m_contentid

我们把这段代码做一下修改：

sql = "select imagepath,IndexImage from {prefix}Content where ContentID=" &amp; m_contentid

就是在读写参数时将缩略图的字段也一起读出来，用于下面程序的比对。

继续往下找

img=rs(0)

然后在这个代码下面增加

img_index = rs(1)

再往下找：

for each img in imgs
if iCount &lt; cint(m_maxcount) then

将这个if逻辑判断修改一下，这个if判断就是我们要处理的地方噢。

if iCount &lt; cint(m_maxcount) then
if exc_IndexImage="true" and img=img_index then
iCount = iCount - 1
else
soutput = soutput &amp; match.SubMatches(1)
soutput =  replace(soutput,"[cimages:src]",img)
soutput = replace(soutput,"[cimages:i]",iCount+1)
end if
end if

然后，保存，更新，你就会发现{aspcms:cimages}标签多了一个参数了噢，具体使用方法如下：

{aspcms:cimages count=5 contentid=1 <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">indeximage=true</span></span>}
[cimages:src]
[cimages:i]
{/aspcms:cimages}

附录：makeContentImages完整的代码
<pre>'2013.03.20 增加一个参数IndexImage，如果为true则输出的图片列表中不包含缩略图文件
Function makeContentImages(sContent)
'{aspcms:cimages count=5 contentid=1}
'[cimages:src]
'{/aspcms:cimages}
'if instr(sConetnt,"{aspcms:cimages") then
dim rs,sql,img,imgs
dim m_labelRule,m_labelRuleField
dim regExpObj
dim match,matches
dim m_contentid
dim m_maxcount,iCount
dim soutput
soutput = ""
m_contentid = empty
iCount = 0
sql = "select * from {prefix}Content where contentid="
'set rs = conn.exec(sql,"r1")
set regExpObj= new RegExp
m_labelRule="{aspcms:cimages([\s\S]*?)}([\s\S]*?){/aspcms:cimages}"
dim exc_IndexImage,img_index
regExpObj.Pattern=m_labelRule
set matches=regExpObj.Execute(sContent)
for each match in matches
m_contentid = parseArr(match.SubMatches(0))("contentid")
m_maxcount = parseArr(match.SubMatches(0))("count")
exc_IndexImage = parseArr(match.SubMatches(0))("indeximage")
if isnul(exc_IndexImage) then exc_IndexImage=false
if isnul(m_maxcount) or not isnumeric(m_maxcount) then m_maxcount = 9999
if not isnul(m_contentid) then
if not isnumeric(m_contentid) then m_contentid=-1
'2013.03.20 增加获取缩略图字段，用于对参数的控制处理
sql = "select imagepath,IndexImage from {prefix}Content where ContentID=" &amp; m_contentid
set rs = conn.exec(sql,"r1")
if not rs.eof then
img=rs(0)
img_index = rs(1)  '2013.03.20
end if
rs.close
set rs = nothing
imgs = split(img,"|")
for each img in imgs
if iCount &lt; cint(m_maxcount) then
if exc_IndexImage="true" and img=img_index then
iCount = iCount - 1
else
soutput = soutput &amp; match.SubMatches(1)
'soutput = replace(soutput,"[aspcms:cimagesitem]","")
'soutput = replace(soutput,"[/aspcms:cimagesitem]","")
soutput =  replace(soutput,"[cimages:src]",img)
soutput = replace(soutput,"[cimages:i]",iCount+1)
end if
'echo img &amp; "&lt;br&gt;"
end if
iCount = iCount + 1
next
end if
'die soutput
sContent=replaceStr(sContent,match.value,soutput)
'die sConetnt
next
if instr(sContent,"{aspcms:cimages") &gt; 0 then
'die "停止"
makeContentImages sContent
end if
'end if
makeContentImages = sContent
End Function</pre>