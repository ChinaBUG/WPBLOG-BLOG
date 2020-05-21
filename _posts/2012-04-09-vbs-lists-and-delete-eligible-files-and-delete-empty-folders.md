---
ID: 2015
post_title: 'VBS-根据时间自动列出文件对比时间删除符合条件的文件及删除空文件夹(根据修改时间自动删除早期旧文件&#038;更新文件的修改时间属性)'
author: ChinaBUG
post_excerpt: |
  自从走上ShopEX商城二次开发这条路，一直在安装ShopEX与删除之间，每当接到新的项目时都是需要新建一个新的商城系统来全新开发，为什么呢？只是为了保证尽量小的收到干扰，便于程序的应用。
  最痛苦的还不是安装与删除这个过程，最痛苦的是项目最好，要把做过开发的文件挑出来，把不相干的文件删除，将代码及文件做精简才是最令人烦恼的，有些时候做开发，你没办法做到步步都做笔记，最好了才发觉自己不知道修改了那个文件，真是烦恼，如果忘记把修改后的文件发给客户的话，呵呵，好的客户不说什么，不好的客户还以为你是故意的。多伤感情噢。
  装SVN嘛~~有点难度，还是算了，用电简单的办法解决吧。
  这个脚本就是基于这个目的而开发的，只要指定网站目录，然后它会自动将在三十天内修改的文件保留，而三十天都没修改的文件直接删除，文件夹空了，则同时删除。
  ~~请将下面的代码保存为vbs文件~~
  ~~具体操作如下：复制下面代码，黏贴到记事本，然后保存，在文件名里面输入 文件名.vbs 然后格式里面选择全部文件，即可。直接双击执行，等出现OK时就搞定了噢~~
  ~~从我下面这行开始，不含我这行噢~~
  FolderPath = InputBox("请输入需要处理的文件夹","虫神的辅助工具-删除未二次开发的文件","G:\PHPnow\htdocs")
layout: post
permalink: >
  http://blog.ipodmp.com/2012/04/vbs-lists-and-delete-eligible-files-and-delete-empty-folders.html
published: true
post_date: 2012-04-09 17:02:57
---
自从走上ShopEX商城二次开发这条路，一直在安装ShopEX与删除之间，每当接到新的项目时都是需要新建一个新的商城系统来全新开发，为什么呢？只是为了保证尽量小的收到干扰，便于程序的应用。

最痛苦的还不是安装与删除这个过程，最痛苦的是项目最好，要把做过开发的文件挑出来，把不相干的文件删除，将代码及文件做精简才是最令人烦恼的，有些时候做开发，你没办法做到步步都做笔记，最好了才发觉自己不知道修改了那个文件，真是烦恼，如果忘记把修改后的文件发给客户的话，呵呵，好的客户不说什么，不好的客户还以为你是故意的。多伤感情噢。

装SVN嘛~~有点难度，还是算了，用电简单的办法解决吧。

这个脚本就是基于这个目的而开发的，只要指定网站目录，然后它会自动将在三十天内修改的文件保留，而三十天都没修改的文件直接删除，文件夹空了，则同时删除。

~~请将下面的代码保存为vbs文件~~

~~具体操作如下：复制下面代码，黏贴到记事本，然后保存，在文件名里面输入 文件名.vbs 然后格式里面选择全部文件，即可。直接双击执行，等出现OK时就搞定了噢~~

~~我的功能是：自动列出并删除符合条件的文件及删除空文件夹(根据修改时间自动删除早期文件)~~

~~从我下面这行开始，不含我这行噢~~

FolderPath = InputBox("请输入需要处理的文件夹","虫神的辅助工具-删除未二次开发的文件","G:\PHPnow\htdocs")

if FolderPath&lt;&gt;"" then
'删除文件
FilesTree FolderPath
'删除空文件夹
FolderisEmpty FolderPath
msgbox "OK~~"
end if

'列出文件
'如果符合修改时间的则保留，未修改的则删除
Function FilesTree(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders

Set oFiles = oFolder.Files
For Each oFile In oFiles
if CLng(DateDiff("d", oFile.DateLastModified,Now)) &gt; 30 then
oFile.Delete
end if
Next
For Each oSubFolder In oSubFolders
FilesTree(oSubFolder.Path)'递归
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function
'空文件夹处理
'如果为空则删除
Function FolderisEmpty(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders
For Each oSubFolder In oSubFolders
If oSubFolder.Size=0 Then
oSubFolder.Delete
else
FolderisEmpty(oSubFolder.Path)'递归
End If
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function

~~不要复制我，到上面一行为止~~

~~我的功能是：更新html及.php文件的修改时间~~

~~从我下面开始复制~~

FolderPath = InputBox("请输入需要处理的文件夹","虫神的辅助工具-统一文件的修改日期","G:\core2_rcomp2p")

if FolderPath&lt;&gt;"" then
'删除文件
FilesTree FolderPath
'删除空文件夹
FolderisEmpty FolderPath
msgbox "OK~~"
end if

'列出文件
'如果符合修改时间的则保留，未修改的则删除
Function FilesTree(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders
Set oFiles = oFolder.Files
For Each oFile In oFiles
ReadFiles(oFile.path)
'if CLng(DateDiff("d", oFile.DateLastModified,Now)) &gt; 30 then
'    oFile.Delete
'end if
Next
For Each oSubFolder In oSubFolders
FilesTree(oSubFolder.Path)'递归
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function
'空文件夹处理
'如果为空则删除
Function FolderisEmpty(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders
For Each oSubFolder In oSubFolders
If oSubFolder.Size=0 Then
oSubFolder.Delete
else
FolderisEmpty(oSubFolder.Path)'递归
End If
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function
'写空白行，更新那个最后修改日期
Function ReadFiles(sFile)
Dim fso, f1, ts, s
Const ForAppending = 8
Set fso = CreateObject("Scripting.FileSystemObject")
ext = LCase(right(sFile,4))
if ext="html" or ext=".php" then
Set ts = fso.OpenTextFile( sFile , ForAppending)
ts.WriteBlankLines(1)
ts.Close
end if
End Function

~~不要复制我，到上面一行为止~~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

2012.04.12 修正

实践出真知呀，经过试用才发觉，上面的脚本却是可以得到一定的发挥作用的，但是不够符合二次开发时的流程呀~怎么办呢？重新写呗，下面是修改之后的两个文件，分别是生成参照的原始文档，用于删除参照，一个是根据参照文档执行删除操作跟最后的空文件夹的删除。

使用的流程是：在开发之前，先执行一下step1的脚本，生成参照文档，然后发布的时候执行step2的脚本删除商城原先的没有做过修改的文件，剩下的文件就是自己开发的全部文档了。

~~复制我下面的代码保存成 Step1-生成参照档.vbs 文件~~

'列出全部的文件及修改的时间
'将信息记录到文件
'打开文件，按照记录一条条的对，如果修改时间已经修改则保留，否则删除
'''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''''
FolderPath = InputBox("请输入需要处理的文件夹","虫神的辅助工具-Step1-生成参照档","G:\PHPnow\htdocs")

if FolderPath&lt;&gt;"" then
CreateAfile()
genTree FolderPath
msgbox "文档生成完毕"
end if
'---------------------------------
'列出文件
'如果符合修改时间的则保留，未修改的则删除
Function genTree(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders

Set oFiles = oFolder.Files
For Each oFile In oFiles
writetxt(oFile.Path &amp; "|" &amp; oFile.DateLastModified)
Next
For Each oSubFolder In oSubFolders
genTree(oSubFolder.Path)'递归
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function
'写
Function writetxt(sPath)
Dim fso, f1, ts, s
Const ForAppending = 8
Set fso = CreateObject("Scripting.FileSystemObject")
Set ts = fso.OpenTextFile( Cstr(Date)+".txt" , ForAppending)
ts.Writeline(sPath)
ts.Close
End Function
Sub CreateAfile()
Dim fso, MyFile
Set fso = CreateObject("Scripting.FileSystemObject")
Set MyFile = fso.CreateTextFile( Cstr(Date)+".txt", True)
MyFile.Close
End Sub

~~到上面结束~~

~~复制我下面的代码保存成 Step2-根据文档清除文件.vbs 文件~~

openfilename = InputBox("请输入需要处理的文件名","虫神的辅助工具-Step2-根据文档清除文件",Cstr(Date)+".txt")
folderPath = InputBox("请输入需要处理空文件夹的路径","虫神的辅助工具-Step2-根据文档清除文件","G:\core2_rcomp2p")

if openfilename&lt;&gt;"" then
'载入文件内容
Const ForReading = 1
Dim fso, theFile, retstring
Dim fileline(1000000),flnum
flnum = 0
Set fso = CreateObject("Scripting.FileSystemObject")
Set theFile = fso.OpenTextFile( openfilename, ForReading, False)

Do While theFile.AtEndOfStream &lt;&gt; True
fileline(flnum) = theFile.ReadLine
flnum=flnum+1
Loop
theFile.Close
'开始处理文件
For Each f in fileline
if f&lt;&gt;"" then
ff = Split(f,"|")
rundelete ff(0),ff(1)
end if
Next
if folderPath&lt;&gt;"" then
FolderisEmpty(folderPath)
end if
msgbox "文件已经整理完毕"
end if
'--------
Function rundelete(filespec,DateLModified)
Dim fso,f
Set fso = CreateObject("Scripting.FileSystemObject")
if fso.FileExists(filespec) then
Set f = fso.GetFile(filespec)
if DateDiff("d",f.DateLastModified, DateLModified)=0 then
'msgbox "相同"
f.Delete
end if
end if
End Function
'空文件夹处理
'如果为空则删除
Function FolderisEmpty(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders
For Each oSubFolder In oSubFolders
If oSubFolder.Size=0 Then
oSubFolder.Delete
else
FolderisEmpty(oSubFolder.Path)'递归
End If
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function

~~到上面结束~~