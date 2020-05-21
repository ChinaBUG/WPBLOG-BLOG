---
ID: 827
post_title: >
  如何在IE右键菜单中添加菜单项
author: ChinaBUG
post_excerpt: >
  如果使用过Netants的朋友可能都知道，NetAnts在IE中添加了右键菜单功能，只要在页面的一个链接或者图片上点击右键后在菜单中选择
  Down By Netants
  就可以调用Netants下载该链接指向的文件。在本文中作者将介绍如何通过VB来实现这样的功能。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/10/how-in-the-ie-context-menu-add-a-menu-item.html
published: true
post_date: 2010-10-13 15:04:40
---
一、如何在IE右键菜单中添加菜单项

　　如果使用过Netants的朋友可能都知道，NetAnts在IE中添加了右键菜单功能，只要在页面的一个链接或者图片上点击右键后在菜单中选择 Down By Netants 就可以调用Netants下载该链接指向的文件。在本文中作者将介绍如何通过VB来实现这样的功能。

　　要实现在IE右键菜单中添加菜单项的功能，要依次实现以下步骤：

　　1、在注册表HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\MenuExt项下建立一个新项，项的名称既出现在菜单中的标题，例如你想建立的菜单项标题为Add URL，则新建项的名称为
HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\MenuExt\Add URL

　　2、将新建项的默认值设定为一个URL地址，当用户点击菜单项后，IE就会调用URL指向的页面中的脚本，在目标页面的脚本中通过访问IE提供的external对象的menuArguments属性就可以访问IE中的页面中的各种对象，例如链接、图片、表单域、被选中的文本等。详细的帮助请参考MSDN中关于InternetExplore object的帮助，熟悉了Window对象才可以比较好的了解下面的脚本。

　　对于如何实现自身的程序访问menuArguments的问题，我们可以仿效Netants的做法，首先建立一个OLE Automation对象，然后在脚本中调用该对象，并将页面信息传递对象处理。下面我们需要首先通过VB建立一个对象：

　　打开VB，点击菜单： File | New ，在新建工程窗口中选择 ActiveX Dll 后按确定键建立一个ActiveX DLL工程。然后在工程列表窗口中将Class1的Name属性更改为NetAPI，然后在NetAPI的代码窗口中添加如下代码：

Public Sub AddURL(URL As String, Info As String)
　　MsgBox Info, vbOKOnly, URL
End Sub

　　保存文件，将工程文件保存成NetSamp.vbp。然后在菜单中选择 File | Make NetSamp.dll建立对象动态连接库。

　　接下来是注册库，在Windows目录下找到Regsvr32.exe，然后将其拷贝到netsamp.dll所在目录下，将netsamp.dll的的图标拖到Regsvr32.exe上放开，这时Regsvr32.exe就会弹出对话框提示对象注册成功。

　　打开UltraEdit(或者其它文本编辑器)将下面的脚本代码输入编辑器中：

&lt;script language="VBScript"&gt;

Sub OnContextMenu()
 On Error Resume Next
 set srcEvent = external.menuArguments.event
 set EventElement = external.menuArguments.document.elementFromPoint(srcEvent.clientX, srcEvent.clientY)
 set objNetSamp=CreateObject("NetSamp.NetAPI")
　　　　if srcEvent.type = "MenuExtAnchor" then
  set srcAnchor = EventElement
  do until TypeName(srcAnchor)="HTMLAnchorElement"
   set srcAnchor=srcAnchor.parentElement
  Loop
  Call objNetSamp.AddUrl(srcAnchor.href,srcAnchor.innerText)
 elseif srcEvent.type="MenuExtImage" then
  if TypeName(EventElement)="HTMLAreaElement" then
   Call objNetSamp.AddUrl(EventElement.href,EventElement.Alt)
  else
   set srcImage = EventElement
   set srcAnchor = srcImage.parentElement
   do until TypeName(srcAnchor)="HTMLAnchorElement"
　　set srcAnchor=srcAnchor.parentElement
　　if TypeName(srcAnchor)="Nothing" then
　　 call objNetSamp.AddUrl(srcImage.href,srcImage.Alt)
　　 exit sub
　　end if
   Loop
   Call objNetSamp.AddUrl(srcAnchor.href, srcImage.Alt)
  end if
 elseif srcEvent.type="MenuExtUnknown" then
  set srcAnchor = EventElement
  do until TypeName(srcAnchor)="HTMLAnchorElement"
   set srcAnchor=srcAnchor.parentElement
   if TypeName(srcAnchor)="Nothing" then
　　Call objNetSamp.AddUrl(EventElement.href,EventElement.innerText)
　　exit sub
   end if
  Loop
  Call objNetSamp.AddUrl(srcAnchor.href,srcAnchor.innerText)
 end if
end Sub

call OnContextMenu()

&lt;/script&gt;

　　将文件保存到c:\program files 下，文件名为 geturl.htm

　　从上面的脚本可以看到，首先访问external.menuArguments属性，获得用户单击鼠标右键位置的对象，然后根据对象的不同获得它的URL，然后建立IEContextMenu.IEMenu1对象并调用该对象的AddURL方法。

　　接下来是为右键菜单建立注册项，打开UltraEdit(或者其它文本编辑器)将下面的注册数据输入编辑器中

Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Microsoft\Internet Explorer\MenuExt\&amp;Get URL]
@="c:\\program files\\geturl.htm"
"Contexts"=dword:00000022

　　将文件以reg为后缀保存，然后在Windows资源管理器中双击该文件将注册项添加到注册表中，然后打开IE，右键点击一个连接或者图片，在弹出菜单中会出现一个Get URL项，点击该项，就会出现一个消息框显示点击的连接或者图片的URL地址

   下面再介绍一下上面注册项中Contexts项的作用，通过该项可以制定菜单项在右键点击IE中的什么对象时出现，它可以为以下值的“或”组合：

   对象  值
   缺省   0x1
   图片   0x2
   控件   0x4
   表单域   0x8
   选择文本  0x10
   锚点   0x20

　　例如上面我们希望菜单项在用户点击图片或者超链接时出现，那么我们就将值设置为dword:00000022，既在点击图片 或者 锚点时出现菜单。一个锚点是页面中描述一个超链接的对象。如果不设置Contexts项，则菜单项会在点击任何对象时出现在右键菜单中。

　　通过上面的程序介绍我们可以看到IE鼠标右键菜单的工作过程。前面讲了，Netants就是使用这样的方法通过在脚本中建立对象来实现调用NetAnts的，那么我们如果安装了NetAnts，就可以在程序中通过调用NetAnts对象来调用NetAnts。

　　建立一个新工程，点击菜单 Projects | References 项，选择其中的 AntAPI 1.0 Type Library 项，如果没有点击Browser按钮，在文件列表框中选择网络蚂蚁目录下的NetAPI.dll后按打开键。在Form1中添加一个CommandButton按钮，在Command1_Click事件中添加如下代码：

　　Dim ant As New ANTAPILib.AntAPIObj

　　ant.AddUrl "<a href="http://www.applevb.com/z.zip">http://www.applevb.com/z.zip</a>", "", "<a href="http://www.applevb.com/">http://www.applevb.com/</a>"

　　点击command1，然后NetAnts就会运行并且将<a href="http://www.applevb.com/z.zip">http://www.applevb.com/z.zip</a>添加到任务中。

二、如何添加任务栏按钮

　　基本上来说，添加任务栏按钮只需要修改注册表就可以实现。通过修改注册表来实现添加按钮的步骤如下：

1、建立一个GUID。

2、打开注册表编辑器，转到HKEY_LOCAL_MACHINE\Software\Microsoft\Internet Explorer\Extensions部分，在其下添加一个新的项，名称为 &lt;Your GUID&gt; ，Your GUID为你刚建立的GUID。

3、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为HotIcon，该值定义当按钮具有热点时的图标，它的一般类型为：
包含图标的文件全路径名,图标索引，例如：
C:\PROGRA~1\KINGSOFT\XDICT\ieplugin.DLL,208

4、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为Icon，该值定义当按钮的图标，它的一般类型为：
图标文件全路径名,图标索引

5、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为ButtonText，该值定义按钮的ToolTip文本。

6、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为Default Visible，该值定义按钮是否可见，如果是，则该值设定为"Yes"，否则设定为"No"。

7、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为Clsid，将该值设定为{1FBA04EE-3024-11D2-8F1F-0000F87ABD16}

8、在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称为Exec，该值定义点击按钮后运行的文件的全路径名称，例如:
c:\program files\samples\net.exe

例如NetAnts的按钮注册表项的内容就是象下面这样：

Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\{57E91B47-F40A-11D1-B792-444553540000}]
"CLSID"="{1FBA04EE-3024-11D2-8F1F-0000F87ABD16}"
"Default Visible"="Yes"
"HotIcon"="C:\\PROGRA~1\\NETANTS\\NetAnts.exe,1001"
"Icon"="C:\\PROGRA~1\\NETANTS\\NetAnts.exe,1000"
"Exec"="C:\\PROGRA~1\\NETANTS\\NetAnts.exe"
"ButtonText"="NetAnts"
"MenuText"="&amp;NetAnts"
"MenuStatusBar"="Launch NetAnts"

　　当点击NetAnts按钮时就会运行Netants。上面的注册表项中下面的两项：MenuText项添加一个菜单项到菜单的“工具”栏中，MenuStatusBar项定义当光标移动到添加的菜单栏上后显示在状态栏中提示文本。此外在注册表的HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下还可以添加一个名称为MenuCustomize的字符串类型值，将该值设定为"Help"将使菜单项出现在“帮助”菜单栏中，否则出现在“工具”栏中。

　　当然，我们不会满足于只是添加一个按钮，执行一个程序，我们希望能够获得当用户点击按钮时能够操控当前页面，在注册表的 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;下建立一个新的String类型的值，名称设定为一个htm文件的全路径名，同前面介绍的添加鼠标右键菜单一样，在点击按钮后IE会调用该文件，在文件中通过设定VBScript访问external对象的menuArguments属性就可以获得浏览器中的页面。例如我们将HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Extensions\&lt;Your GUID&gt;\VBScript的值设定为c:\program files\samp.htm，然后在c:\program files下建立一个名为Samp.htm的文件，在文件中输入以下脚本内容：
　　&lt;script language="VBScript"&gt;

　　Set objNetSamp=CreateObject("IEContextMenu.IEMenu1")
　　userURL=external.menuArguments.location.href
　　Call objNetSamp.AddUrl(userURL,"")

　　&lt;/script&gt;
　　打开IE浏览器，点击新建按钮，就会弹出对话框显示当前页面的URL。注意该项同前面设定的Exec项不能够同时使用。

　　最后，对于按钮图标，IE需要两种尺寸的图标：20x20和16x16的，前者用于正常状态下的显示，后者用于在全屏幕下的显示，所以上面HotIcon和Icon指向的图标资源应该是三个图标的组合，这三个图标的规格如下：

　　16x16 16-色 icon (必须)
　　20x20 16-色 icon (可选)
　　20x20 256-色 icon (必须)

　　在设计图标时，256色图标应该使用Windows半色调调色板，而16色图标使用Windows 16色调色板。

<a href="http://www.applevb.com/art/ie_menu.txt">http://www.applevb.com/art/ie_menu.txt</a>