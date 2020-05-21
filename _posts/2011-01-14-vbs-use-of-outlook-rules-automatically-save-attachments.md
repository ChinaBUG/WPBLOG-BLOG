---
ID: 882
post_title: >
  VBS-使用Outlook的规则自动保存附件
author: ChinaBUG
post_excerpt: |
  　　公司的加盟商图片回传使用邮箱形式，每次保存累死我了，一直想要编个脚本，自动操作一下，结果一直懒的动手，昨天，偶遇张先生的文章，提供好代码，偶只好笑纳了，在此基础上修改了一下，符合自己的需要，贴出来与需要者共享之。
  　　我平常工作时，保存格式是 E:\加盟商图片回传\MMDD.加盟店名称[邮箱地址] ，所以，在脚本中我也是这样设计的。其中MMDD就是当天的日期，如今天2011.01.14则为0114。其他的直接看代码吧。
  ===============
  附录
  本文主要功能参考 张志强先生的< 自动保存Outlook邮件的附件 >
layout: post
permalink: >
  http://blog.ipodmp.com/2011/01/vbs-use-of-outlook-rules-automatically-save-attachments.html
published: true
post_date: 2011-01-14 15:35:11
---
　　公司的加盟商图片回传使用邮箱形式，每次保存累死我了，一直想要编个脚本，自动操作一下，结果一直懒的动手，昨天，偶遇张先生的文章，提供好代码，偶只好笑纳了，在此基础上修改了一下，符合自己的需要，贴出来与需要者共享之。

　　我平常工作时，保存格式是 E:\加盟商图片回传\MMDD.加盟店名称[邮箱地址] ，所以，在脚本中我也是这样设计的。其中MMDD就是当天的日期，如今天2011.01.14则为0114。其他的直接看代码吧。

Public Sub SaveAttach(Item As Outlook.MailItem)
    '定义返回值变量，这个变量主要是确认是否要保存附件
    Dim isRun
    isRun = MsgBox(Item.Subject, 33, "保存确认 -- ChinaBUG")
    '定义保存的主目录,需新建的文件夹名,新的路径
    Dim RootPath, NewF, NewPath
    RootPath = "E:\邮件附件\"
    NewF = RLeft(Month(Date)) &amp; RLeft(Day(Date)) &amp; "." &amp; Item.Subject &amp; "[" &amp; Item.SenderEmailAddress &amp; "]"
    NewPath = RootPath &amp; NewF &amp; "\"
    '判断是否要保存
    If isRun = vbOK Then
        SaveAttachment Item, NewPath, "*.*"
        MsgBox "附件已保存."
    Else
        MsgBox "您已经取消关于 " &amp; Item.Subject &amp; " 的保存操作."
    End If
End Sub

' 保存附件函数
' path为保存路径，condition为附件名匹配条件
Private Sub SaveAttachment(ByVal Item As Object, ByVal path As String, Optional condition = "*")
    Dim olAtt As Attachment
    Dim i As Integer
    '检查文件夹是否存在，不存在就新建
    Dim fso, f
    Set fso = CreateObject("Scripting.FileSystemObject")
    If fso.FolderExists(path) &lt;&gt; True Then
        fso.CreateFolder (path)
    End If
    '循环保存附件
    If Item.Attachments.Count &gt; 0 Then
        For i = 1 To Item.Attachments.Count
            Set olAtt = Item.Attachments(i)
            If olAtt.FileName Like condition Then
                olAtt.SaveAsFile path &amp; olAtt.FileName
            End If
        Next
    End If
    MsgBox "此次保存 " &amp; Item.Attachments.Count &amp; " 个对象."
    Set olAtt = Nothing
End Sub

'该函数自动补充不足的"0"
Function RLeft(sval)
    RLeft = Right("00" &amp; CStr(sval), 2)
End Function

　　如何运行这个脚本呢？张先生初略的说一下，对我这种不熟悉OE的人来说实在是痛苦之事，还好，偶找到了。

　　请打开 <span style="text-decoration: underline;">工具、规则和通知</span> 在窗口中，点击 <span style="text-decoration: underline;">新建规则</span>，然后点击 <span style="text-decoration: underline;">由空白规则开始</span>，按照提示做吧，里面都有噢~~记得运行项里面的脚本要选择你自己编写的函数名噢。

===============

附录

本文主要功能参考 张志强先生的&lt; <a href="http://zhiqiang.org/blog/it/auto-save-attachment-in-outlook.html">自动保存Outlook邮件的附件</a> &gt;