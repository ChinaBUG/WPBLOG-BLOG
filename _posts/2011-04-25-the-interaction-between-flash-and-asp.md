---
ID: 925
post_title: FLASH与ASP之间的交互
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/04/the-interaction-between-flash-and-asp.html
published: true
post_date: 2011-04-25 16:36:50
---
　　 一、Flash与Asp之间的交互
　　Flash与Asp的通讯是用Http协议，其请求格式为<a href="http://ip/" target="_blank">http://ip</a>地址?参数1=值1&amp;参数2=值2
　　即是在目的地址后面加上问号，再跟上参数字符串，参数之间用“&amp;”号格开。如：
<a href="http://www.zcool.com.cn/test.asp?userid=guest&amp;pwd=123" target="_blank">http://www.zcool.com.cn/test.asp?userid=guest&amp;pwd=123</a>
　　在上面的请求中，请求的目的文件为：<a href="http://www.zcool.com.cn/test.asp" target="_blank">http://www.zcool.com.cn/test.asp</a>，第一个参数名为userid，值为guest，第二个参数名为pwd，值为123。
　　Flash与Asp之间的交互无非就是构造上面的请求字符串。
　　1、在Flash中，先构造好请求的字符串，然后利用函数LoadVariables()，就可以向服务器端发送请求和参数。我们来详细看看LoadVariables()这个函数。
　　函数的标准格式为loadVariables ("url" ,level/"target" [, variables])
　　在函数的各个参数中，url就是上面说的请求字符串。level/“target”是返回值的“层次”或者“目标”，这两个当中只能指定一个。variables是请求的方式，其值可以是“Get”或者是“Post”，一般Get用于参数值比较短的传送，Post用于参数值比较长的传送，这个参数是可选的。比如loadVariables ("<a href="http://www.zcool.com.cn/guest.asp?userid=guest&amp;pwd=123" target="_blank">http://www.zcool.com.cn/guest.asp?userid=guest&amp;pwd=123</a>" ,0, “GET”)就是一个完整的请求。
　　2、在Asp中，先要取得从Flash端传送过来的参数，这跟操作普通的HTML表单是一样的。都是利用Request对象，其语句为：
username = Request(“userid”)
password = Request(“pwd”)
　　userid和pwd就是从Flash端发送过来的参数名，如果是上一步中的请求字符串，username的值为guest，pwd的值为123。
3、在服务器端处理完请求，获得所需要的值后，Asp向Flash端发送结果，跟从Asp中操作Html语言一样，都是用Response对象，其语句为：
Response.Write(“login=true&amp;des=success”)
其返回值1的名为login，值为true，返回值2的名为des，值为success。
　　4、在Flash端取得从服务器端返回的值，与操作Flash中普通的变量没什么不同。如：
_root.gotoAndPlay(eval(login))表示的是跳转到login的值的那一帧。但要注意的是在发送请求一段时间之后，才能用返回值，不然取得的是尚未返回的值，错误就在所难免了，而且这一类的错误很难发现，用的时候要多加小心。
　　二、Asp与数据库之间的交互
　　在Asp与数据库的交互一般是用ADO控件。其读取数据库的语句为：
‘定义一个Connection对象
set conn=Server.CreateObject("ADODB.Connection")
‘用Connection对象打开数据库，这里打开的是SQL server，数据库的地址为192.168.1.32
‘数据库的用户名为zengyu，密码为123
conn.open application("Driver={SQL Server};SERVER=192.168.1.32;DATABASE=test;UID=zengyu;PASSWORD=123")
‘创建一个Recordset对象
set rstemp=Server.CreateObject("ADODB.Recordset")
‘构造一个SQL语句
SQLtemp1="select * from UserInfo where userid='"&amp;strname&amp;"' and password='"&amp;strpassword&amp;"'"
‘查询数据库
rstemp.open SQLtemp1,conn, 1, 1
if not(rstemp.bof and rstemp.eof) then
Response.Write (“login=true”)
end if
　　这里实现的只是简单地查询数据库，要想了解Asp操作数据库更详细的东西，可以找Asp与数据库方面的资料深入学习一下。
　　三、例子――登陆的实现
　　下面我们来制作一个简单的实例，在Flash端输入用户名和密码，通过Asp查询数据库，如果用户名和密码正确，就跳转到登陆成功界面，否则就跳转到登陆失败界面。
　　1、新建一个Flash，在场景中制作两个文本框和一个Button，如图2所示。其中用户名对应的文本框属性如图3所示，密码对应的文本框属性如图4所示。注意其中的文本类型和变量名。
2、创建另外两个关键帧，分别命名为“true”和“false”，并分别显示“登陆成功”和“登陆失败”字样。
　　3、在Button的ActionScript中增加下面的语句，注意更改其中的ip地址。
on (release) {
loadVariables("<a href="http://192.168.1.1/guest.asp?userid" target="_blank">http://192.168.1.1/guest.asp?userid</a>=" add eval(_root.userid) add "&amp;pwd=" add eval(_root.pwd),this, "GET");
now = new Date();
begintime = now.getSeconds();
while(true) {
endt = new Date();
endtime = endt.getSeconds();
if (endt - now &gt; 2)
{
_root.gotoAndPlay(eval(login));
}
}
}
　　4、在SQL Server数据库（数据库的类型不重要，改一改连接串就可以的）中，建立一张名为“userinfo”的表，其中有“Userid”和“Password”两个字段。
　　5、建立一个guest.asp文件，文件内容为
&lt;%
username = Request(“userid”)
password = Request(“pwd”)
set conn=Server.CreateObject("ADODB.Connection")
conn.open application("Driver={SQL Server};SERVER=192.168.1.32;DATABASE=test;UID=zengyu;PASSWORD=123")
set rstemp=Server.CreateObject("ADODB.Recordset")
SQLtemp1="select * from UserInfo where userid='"&amp;username&amp;"' and password='"&amp;strpassword&amp;"'"
rstemp.open SQLtemp1,conn, 1, 1
if not(rstemp.bof and rstemp.eof) then
Response.Write (“login=true”)
Else
Response.Write (“login=false”)
end if
%&gt;
　　6、将Flash文件和Asp文件部署到IIS服务器中，然后打开Flash文件，输入登陆信息就可以看到实例的效果了。
<div><strong>ActionScript3.0 页面给swf文件传值接受</strong></div>
<div>

以往as2.0通过web传递参数给swf
只需swf声明一个变量，如： var par:String;
WEB传递参数方式可以按如下方式： a.swf?par=aabbccc
这样子swf就能取得到web传递的参数值。

as3.0改变了些方式，要取得web传递的参数需要使用<strong>loaderInfo.parameters[]</strong>方法。

如：web传递参数为： a.swf?n=benblog.cn
swf 可先声明一个变量
var id:String;
id = loaderInfo.parameters["n"]; //parameters["n"] 中 n为web的参数名。

<strong>HTML与Flash传值的HTML方法</strong>

SWF地址后使用参数传递符“？”
FlashVars传递
JS控制

　　下面来具体介绍下这三种方式的传递是如何工作的：

<strong>　　 一、SWF地址后使用参数传递符“？”</strong>

　　我们知道，在ULR地址中使用参数传递符“？”可以以GET方式传递参数，例如 <a href="http://wenwen.soso.com/z/UrlAlertPage.e?sp=Shttp%3A%2F%2Fwww.v-sky.com%3Fuid%3D12%26uname%3Dvsky" target="_blank">http://www.v-sky.com?uid=12&amp;uname=vsky</a>，这里使用了参数传递符“？”，同时使用了连接符“&amp;”做为变量分隔标识，以这种规范的格式来传递两个参数：uid=12和uname=vsky，那么服务端可以使用GET方式获取这两个值。

　　在FLASH中我们同样可以采用类似的方式来传递参数，HTML页面中插入SWF文件最常用的就是使用Object标签和Embed标签结合的方式，这也是Adobe的推荐方式：

&lt;object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase=" <a href="http://wenwen.soso.com/z/UrlAlertPage.e?sp=Shttp%3A%2F%2Ffpdownload.macromedia.com%2Fpub%2Fshockwave%2Fcabs%2Fflash%2Fswflash.cab%23version%3D8%2C0%2C0%2C0%22" target="_blank">http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=8,0,0,0"</a> width="400" height="300" id="flashvars" align="middle"&gt;
&lt;param name="allowScriptAccess" value="sameDomain" /&gt;
&lt;param name="movie" value="demo.swf?uid=12&amp;uname=vsky" /&gt;
&lt;param name="quality" value="high" /&gt;&lt;param name="bgcolor" value="#ffffff" /&gt;
&lt;embed src="demo.swf?uid=12&amp;uname=vsky" quality="high" bgcolor="#ffffff" width="400" height="300" name="flashvars" align="middle" allowScriptAccess="sameDomain" type="application/x-shockwave-flash" pluginspage=" <a href="http://wenwen.soso.com/z/UrlAlertPage.e?sp=Shttp%3A%2F%2Fwww.macromedia.com%2Fgo%2Fgetflashplayer%22" target="_blank">http://www.macromedia.com/go/getflashplayer"</a> /&gt;
&lt;/object&gt;

其中粗体部分对应的就是SWF文件的地址，那么我们可以在这个地址后面通过类似于URL中GET方式传参的方法来个SWF传递参数，例如上面代码在页面完全加载完毕时，它已经给SWF文件写入了两个变量：uid=12和uname=vsky。

　　 <strong>二、FlashVars传递</strong>

　　你可以查阅FLASH帮助文档来看FlashVars的官方定义。其实在HTML语法中，这是一个被很多新手所忽视的属性，同样以上面的参数为例，下面用FlashVars来传递变量：

&lt;object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase=" <a href="http://wenwen.soso.com/z/UrlAlertPage.e?sp=Shttp%3A%2F%2Ffpdownload.macromedia.com%2Fpub%2Fshockwave%2Fcabs%2Fflash%2Fswflash.cab%23version%3D8%2C0%2C0%2C0%22" target="_blank">http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=8,0,0,0"</a> width="400" height="300" id="flashvars" align="middle"&gt;
&lt;param name="allowScriptAccess" value="sameDomain" /&gt;
&lt;param name="movie" value="demo.swf" /&gt;
&lt;param name="FlashVars" value="uid=12&amp;uname=vsky" /&gt;
&lt;param name="quality" value="high" /&gt;&lt;param name="bgcolor" value="#ffffff" /&gt;
&lt;embed src="demo.swf" FlashVars="uid=12&amp;uname=vsky" quality="high" bgcolor="#ffffff" width="400" height="300" name="flashvars" align="middle" allowScriptAccess="sameDomain" type="application/x-shockwave-flash" pluginspage=" <a href="http://wenwen.soso.com/z/UrlAlertPage.e?sp=Shttp%3A%2F%2Fwww.macromedia.com%2Fgo%2Fgetflashplayer%22" target="_blank">http://www.macromedia.com/go/getflashplayer"</a> /&gt;
&lt;/object&gt;

跟方式一相同，它也是直接给FLASH里添加了这两个变量。但我个人推荐使用此方式，结合SWFObject的使用，使用FlashVars来传递变量有很多好处，例如代码清晰，容易管理，浏览其兼容，符合标准。他们的结合使用在“为FLASH程序构造灵活的接口”一文中我已经做了介绍（PS:随后我会提供一个复杂点的、有说服力的实际应用来说明这种灵活接口的使用)。

　　 <strong>三、JS控制</strong>

　　对于客户端页面中的资源，JS通过DOM结构来控制它们可以说是随心所欲的，FLASH也不例外，下面是Flash Player的Javascript方法一览表:

Play() —————————————- 播放动画
StopPlay()————————————停止动画
IsPlaying()———————————– 动画是否正在播放
GotoFrame(frame_number)—————- 跳转到某帧
TotalFrames()——————————- 获取动画总帧数
CurrentFrame()——————————回传当前动画所在帧数-1
Rewind()————————————-使动画返回第一帧
SetZoomRect(left,top,right,buttom)——-放大指定区域
Zoom(percent)——————————改变动画大小
Pan(x_position,y_position,unit)————使动画在x,y方向上平移
PercentLoaded()—————————-返回动画被载入的百分比
LoadMovie(level_number,path)———– 加载动画
TGotoFrame(movie_clip,frame_number)- movie_clip跳转到指定帧数
TGotoLabel(movie_clip,label_name)—— movie_clip跳转到指定标签
TCurrentFrame(movie_clip)————— 回传movie_clip当前帧-1
TCurrentLabel(movie_clip)—————–回传movie_clip当前标签
TPlay(movie_clip)—————————播放movie_clip
TStopPlay(movie_clip)———————-停止movie_clip的播放
GetVariable(variable_name)—————–获取变量
SetVariable(variable_name,value)———–变量赋值
TCallFrame(movie_clip,frame_number)—call指定帧上的action
TCallLabel(movie_clip,label)—————-call指定标签上的action
TGetProperty(movie_clip,property)——–获取movie_clip的指定属性
TSetProperty(movie_clip,property,number)———-设置movie_clip的指定属性

　　在这里我们只需要使用的是粗体标识的SetVariable方法，JS通过调用此方法能够直接更改SWF中的变量值。首先我们需要定义插入的SWF的ID，例如id为VskyDemo，那么我们可以通过下面的JS语句来完成SWF内部变量的设置： window.document.VskyDemo.SetVariable("uid", 12);

　　很简单吧，就是这样的。除非是涉及到了HTML中SWF之外元素跟它交互，否则我一般不使用JS来控制SWF里的变量，因为我总觉得怪怪的，呵呵，个人习惯吧。多多实践，不要觉得这些小东西不起眼，小东西多了，聚结到一起了就是一个大的应用。条条大路通北京，选择你自己喜欢的，自己认为便捷的方式就可以了，目前我是没有发现这三个方式存在功能上的缺陷。

</div>
本文由 <a href="http://www.zcool.com.cn/" target="_blank">站酷网</a> - <a href="http://www.zcool.com.cn/u/543750/" target="_blank">pwdesign</a> 原创，转载请保留此信息，多谢合作。