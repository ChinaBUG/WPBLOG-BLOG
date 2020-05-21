---
ID: 433
post_title: >
  类型BoundField不具有HtmlEncodeFormatString属性
author: ChinaBUG
post_excerpt: |
  　　说明: 在分析向此请求提供服务所需资源时出错。请检查下列特定分析错误详细信息并适当地修改源文件。
  　　分析器错误信息: 类型“System.Web.UI.WebControls.BoundField”不具有名为“HtmlEncodeFormatString”的公共属性。
  
  　　上周我在新网互联的服务器出现上面莫名其妙的错误，之前用的都还是好好的，代码绝对没修改，自从上周服务器不能使用开始，一直会出现上面的错误。
  　　找客服，回复说是代码问题，服务器没问题，扯了老半天，结果问题还是没解决，实在火大~~！
  　　小样，实习的吧？！
  　　我实在不行，只好上MSDN去查，结果发现：我的代码是正确的！出现这个问题，还真的是服务器问题，TNN的服务器没安装.NET Framework SP1造成的。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/boundfield-does-not-have-htmlencodeformatstring-property.html
published: true
post_date: 2010-04-12 14:42:47
---
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">“/”应用程序中的服务器错误。
--------------------------------------------------------------------------------
分析器错误
说明: 在分析向此请求提供服务所需资源时出错。请检查下列特定分析错误详细信息并适当地修改源文件。
分析器错误信息: 类型“System.Web.UI.WebControls.BoundField”不具有名为“HtmlEncodeFormatString”的公共属性。
源错误:
行 182:                DataSourceID="ADS_Show_Copyright" EnableModelValidation="True" GridLines="None"&gt;
行 183:                &lt;Fields&gt;
行 184:                    &lt;asp:BoundField DataField="Config_Content" HeaderText="Config_Content" ShowHeader="False"
行 185:                        SortExpression="Config_Content" HtmlEncode="False" HtmlEncodeFormatString="False" /&gt;
行 186:                &lt;/Fields&gt;
源文件: /index.aspx    行: 184
--------------------------------------------------------------------------------
版本信息: Microsoft .NET Framework 版本:2.0.50727.42; ASP.NET 版本:2.0.50727.42
</span></span><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">----------------------------------------------</span></span>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"> </span><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">　　上周我在新网互联的服务器出现上面莫名其妙的错误，之前用的都还是好好的，代码绝对没修改，自从上周服务器不能使用开始，一直会出现上面的错误。 </span><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">　　找客服，回复说是代码问题，服务器没问题，扯了老半天，结果问题还是没解决，实在火大~~！ </span></span></div>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">　　小样，实习的吧？！ 

　　我实在不行，只好上MSDN去查，结果发现：我的代码是正确的！出现这个问题，还真的是服务器问题，TNN的服务器没安装.NET Framework SP1造成的。 

　　只好又跟客服说这个情况 

　　干~！ 

　　竟然还是说技术说得，是我代码问题，让我自个解决，后面就不鸟我了！ 

　　得~我的错，我干嘛要买你们家的服务器噢，我记住了！ 

　　这个问题的解决办法就是不要出现.net SP1里面的特性就可以了，好吧，都转为模板再说吧，结果如何还不知道，因为我改完上传，TNN的服务器就彻底歇菜了，打不开了~~~ 

　　PS：
　　1、你问我为什么知道是服务器歇菜？原因自然是我刚才没说的啦，没用到HtmlEncodeFormatString这个属性的是管理员界面，所以，管理员界面是能打开的，现在都打不开了，还是我代码的问题吗？！
　　2、恩，在我写好这篇文章之后再次测试时，服务器恢复正常了。 

<span style="text-decoration: underline;"><strong>结论区：</strong></span> 

　　以后要是出现这个问题，因为本地机的功能肯定是最全面的，也是最新的，所以有时候会出现这个，那个的错误，不要紧，像我上面这个问题吧，只要到MSDN上查找这个属性就知道是不是正确的了~ 

　　<a href="http://msdn.microsoft.com/zh-tw/library/system.web.ui.webcontrols.boundfield.htmlencodeformatstring.aspx" target="_blank">MSDN BoundField.HtmlEncodeFormatString 屬性</a>
　　版本資訊
　　.NET Framework
　　支援版本：3.5 SP1、3.0 SP1、2.0 SP1 

　　上面网址里，我们能知道，恩，这个类型是支持这个属性的，但是支持的属性版本是2.0SP1的，也就是说恩，最低要求是2.0SP1才能运行，现在出现问题证明，服务器上.NET Framework的版本低于2.0SP1，从上面我们可以看到版本号是2.0.50727.42从<a href="http://support.microsoft.com/kb/318785/zh-tw" target="_blank">如何判斷所安裝的 .NET Framework 版本</a>这篇文章里面，我们能查到2.0.50727.42的信息是<span style="text-decoration: underline;"><em> 2.0 原始 RTM 2.0.50727.42</em></span> 。 

<span style="text-decoration: underline;"><strong>附录：</strong></span> 

　　<span style="text-decoration: underline;">新网互联的服务器体验一览</span> 

　　本人买的主机空间是ASP/ASP.NET类型的，200MB，用起来速度一般，不会太快，也不见得多慢。自从去年底开始，服务器会不时出现一些小小问题，会打不开的情况出现，恩，还是能解决的，整体服务一般。 

　　缺点有如下： 

　　1、也许是我这个代理比较差吧，恩，到现在还不知道他们家的网上客服号，就是说他家除了电话联系，或者代理专员联系之外，压根就没有网上客服QQ。这个问题从上周开始一直到周五还是不能解决，接着周末，专员休息，找不到人，恩，我的服务器，继续等等吧！ 

　　恩，你要是注重效率，希望出现问题他们能迅速解决的话，一定要找有网上客服的，而且，网上客服还不会有假期的噢。 

　　2、这个主机目前我只拿来做测试，或者有客户需要的时候临时客串使用。你要问为什么只当作临时的？恩，请记住，你家的网站要是流量大的话，千万别买新网互联的主机，因为他家的流量用的太快了~~ 
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">　　我之前挂着一个阿里巴巴的商城，压根就没宣传，哇~~结果发现连续几个月流量全暴~~~~用的不知不觉，查看流量系统，你会发现，都是那些垃圾站或者莫名其妙的来源，真正的客户流量，恩，没有！</span></div>
 

</span></span>