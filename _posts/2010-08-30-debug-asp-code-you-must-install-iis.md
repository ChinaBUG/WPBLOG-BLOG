---
ID: 770
post_title: 调试ASP代码一定要装IIS吗
author: ChinaBUG
post_excerpt: |
  　　网上经常下载一些ASP代码，如果是需要整页调试倒还好，那只需要安装IIS或者其他的服务器就能调试啦，可是，有时候只想要里面的一个函数，或者单单一些代码片段，只想测试它是否可以正常运行，那么特意安装一个IIS服务器或者其他的，就比较麻烦了。
  　　或者有时候心血来潮，忽然想起项目里面的一个小小应用，要编写一小段代码，结果要打开那个大大的VS或者DW或许就冷水浇头~~没劲啦~！
  　　恩，有没有什么办法，临时调试，效果一样，却不需要如此之麻烦呢？
  　　有！
  　　相信有经验的人都知道ASP其实内置的VB的脚本名字就叫做VBScript，而Windows2000之上的版本均内置Windows 脚本宿主程序，含有VBS、JS的脚本引擎，恩，其实内核都是一样的噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/08/debug-asp-code-you-must-install-iis.html
published: true
post_date: 2010-08-30 10:28:18
---
　　网上经常下载一些ASP代码，如果是需要整页调试倒还好，那只需要安装IIS或者其他的服务器就能调试啦，可是，有时候只想要里面的一个函数，或者单单一些代码片段，只想测试它是否可以正常运行，那么特意安装一个IIS服务器或者其他的，就比较麻烦了。

　　或者有时候心血来潮，忽然想起项目里面的一个小小应用，要编写一小段代码，结果要打开那个大大的VS或者DW或许就冷水浇头~~没劲啦~！

　　恩，有没有什么办法，临时调试，效果一样，却不需要如此之麻烦呢？

　　有！

　　相信有经验的人都知道ASP其实内置的VB的脚本名字就叫做VBScript，而Windows2000之上的版本均内置Windows 脚本宿主程序，含有VBS、JS的脚本引擎，恩，其实内核都是一样的噢。

　　现在知道咋办了吧？

　　只需要将ASP代码保存为扩展名.vbs的文件，双击之后，你就能马上看到效果啦~~~

备注：

　　一个是服务端，一个是客户端的区别，所以，自己编写的代码，倒没关系，如果是网上下载的就需要检查一下，并作相应的修改，否则会报错噢。

　　代码中凡是出现server字样，都要删除哈；凡是出现request、response的，均要修改为input、msgbox函数等等的细节根据自己的提示错误进行修改吧。

　　最重要的是保存时请选择对编码，保存错误，那么肯定是调试不过去的。

　　对于编码问题，具体请查阅本站
<ol>
	<li>&lt;&lt;<a title="永久链接到：网页设计时编码的选择" rel="bookmark" href="http://blog.ipodmp.com/archives/web-design-time-codes-choose/">网页设计时编码的选择</a>&gt;&gt;</li>
	<li>&lt;&lt;<a title="永久链接到：VBS-无效字符错误" rel="bookmark" href="http://blog.ipodmp.com/archives/vbs-invalid-character-error/">VBS-无效字符错误</a>&gt;&gt;</li>
</ol>