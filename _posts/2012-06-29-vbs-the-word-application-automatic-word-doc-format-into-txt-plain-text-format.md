---
ID: 2109
post_title: >
  VBS-用Word.Application自动批量将WORD的doc格式转为txt纯文本格式
author: ChinaBUG
post_excerpt: |
  最近姐姐要用mp4听课件，可见配套的doc文档mp4打不开，只能打开txt纯文本的文件，只能一个个的手动转换了，好多，累死我了~~
  怎么办？
  就整个转换的脚本，用vbs写了一个，只要指定一个目录就会自动将这个目录下面的全部的word文档都给自动转换为txt纯文本文件。
  脚本存在的问题：
  目前如果word文档很大的话，会很影响转换的速度噢，所以在转换之前请将没必要的程序都关闭掉吧，生的影响效率。默认是后台运行，就是你看不到有打开word文档，如果你觉得这样不好，请修改下面代码之中的
  objWord.Visible = false
  将false改为true即可。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/06/vbs-the-word-application-automatic-word-doc-format-into-txt-plain-text-format.html
published: true
post_date: 2012-06-29 20:22:22
---
VBS-怎么自动将Word文档转换成纯文本文件(doc2text)

最近姐姐要用mp4听课件，可见配套的doc文档mp4打不开，只能打开txt纯文本的文件，只能一个个的手动转换了，好多，累死我了~~

怎么办？

就整个转换的脚本，用vbs写了一个，只要指定一个目录就会自动将这个目录下面的全部的word文档都给自动转换为txt纯文本文件。

脚本存在的问题：

目前如果word文档很大的话，会很影响转换的速度噢，所以在转换之前请将没必要的程序都关闭掉吧，生的影响效率。默认是后台运行，就是你看不到有打开word文档，如果你觉得这样不好，请修改下面代码之中的

objWord.Visible = false

将false改为true即可。

下面是脚本的代码，请拷贝下面的代码并保存为*.vbs文件，*代表你自己命名的，随便，反正到时候你要能找到这个文件即可。

FolderPath = InputBox("请输入需要处理的文件夹","虫神的辅助工具","")
if FolderPath&lt;&gt;"" then
genTree FolderPath
msgbox "文档生成完毕"
end if
'---------------------------------
'列出文件
Function genTree(sPath)
Set oFso = CreateObject("Scripting.FileSystemObject")
Set oFolder = oFso.GetFolder(sPath)
Set oSubFolders = oFolder.SubFolders

Set oFiles = oFolder.Files
For Each oFile In oFiles
if oFile.Type="Microsoft Word 文档" then
tr( oFile.Path )
end if
Next
For Each oSubFolder In oSubFolders
genTree(oSubFolder.Path)'递归
Next
Set oFolder = Nothing
Set oSubFolders = Nothing
Set oFso = Nothing
End Function
sub tr(fname)
Const wdFormatText = 2
Set objWord = CreateObject("Word.Application")
objWord.Visible = false
Set objDoc = objWord.Documents.Open(fname)
objDoc.SaveAs fname&amp;".txt",wdFormatText
objWord.Quit
end sub