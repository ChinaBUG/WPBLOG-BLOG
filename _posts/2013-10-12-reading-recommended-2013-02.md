---
ID: 2373
post_title: 悦读推荐
author: ChinaBUG
post_excerpt: |
  2015.08
  Effective JavaScript Item 12 理解Variable Hoisting
  2015.07
  PHP中unserialize返回false的解决方法
  2015.06
  JavaScript客户端检测技术详细解析
  2015.04
  SQL注入，你想知道的那些事
  实战：上亿数据如何秒查
  某中介无线组网及VPN接入应用案
  2015.03
  ThinkPHP框架安全实现分析
  2015.02
  微信公众平台的八大法则
  来自实例的经典分析--HTTP协议
  2015.01
  常见HTTPS攻击方法解析
  2014.12
  解读大型网站系统架构的演化
  2014.09
  有用的PHP代码段（useful php snippets）
  11个实用的Apache .htaccess配置
  程序员需要知道的字符知识总结
  2014.08
  不用jQuery写JS的10条技巧：10 Tips for Writing JavaScript without jQuery
  手游设计如何给玩家带来愉悦的交互体验 原文链接：http://colachan.com/post/3380
  分析了一下作为一个吸引人的游戏应该有哪些东西哈~阅读一下
  2014.06
  走进科学：揭秘如何入侵电视机
  265行代码实现第一人称游戏引擎
  2014.05
  零售行业的数据挖掘七步走
  2014.04
  ASP.NET的HTTP模块和处理程序之处理程序的执行
  关于Android配色 自适应颜色的实现
  2014.03
  下载youtube上视频的姿势大全
  Android设计中的.9.png
  最锋利的Visual Studio Web开发工具扩展：Web Essentials详解
  PHP CodeBase: 判断用户是否手机访问
  亚文化是产品经理必修课
  再谈javascript图片预加载技术-比onload更快获取图片尺寸
  大型网站负载均衡架构
  成为一个PHP专家：缺失的环节
  程序员接触不到大项目，该如何提高自己？
  没读过设计院校，如何成为设计师
  张小龙神话已破灭 马化腾该接管微信了
  颤抖了吗?九步逆向破解银行安全令牌
  Android开发者必备的42个链接
  2014.02
  怎么一步步编写简单的PHP的Framework
  只有20行Javascript代码！手把手教你写一个页面模板引擎 原文Javascript template engine in just 20 lines
  我是如何反编译D-Link路由器固件程序并发现它的后门的
  中国的黑客究竟有多张狂？
  实战演示黑客如何利用SQL注入漏洞攻破一个WordPress网站
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/reading-recommended-2013-02.html
published: true
post_date: 2013-10-12 10:30:47
---
<p>
	2015.08
</p>

<p>
	<a href="http://blog.csdn.net/dm_vincent/article/details/39153783">Effective JavaScript Item 12 理解Variable Hoisting</a><br />
	今天阅读《<a href="http://developer.51cto.com/art/201508/489704.htm">7个去伪存真的JavaScript面试题</a>》内发现一个&ldquo;变量提升（Variable Hoisting）&rdquo;的话题，结果发现，哎还真的没印象，赶紧找一下才发现原来如此，还好平时写的还算规范，要不就麻烦了，恩，还是提前定义再使用这个习惯好，推荐阅读。
</p>

<p>
	2015.07
</p>

<p>
	<a href="http://www.jb51.net/article/55476.htm">PHP中unserialize返回false的解决方法</a><br />
	最近在做ECStore二次开发的时候需要针对字段内序列化的内容做反序列操作，然后就发现，转出来的都是错误的！一开始没怎么注意，今天重新检查的时候就发现这个问题，果断查找一下，找到这篇文章，正好解决问题，推荐一下。
</p>

<div class="container">
	<blockquote>
		<div class="line number1 index0 alt2">
			<em><code class="php comments">// utf8 </code></em>
		</div>

		<div class="line number2 index1 alt1">
			<em><code class="php keyword">function</code> <code class="php plain">mb_unserialize(</code><code class="php variable">$serial_str</code><code class="php plain">) { </code></em>
		</div>

		<div class="line number3 index2 alt2">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php variable">$serial_str</code><code class="php plain">= preg_replace(</code><code class="php string">&#39;!s:(d+):&quot;(.*?)&quot;;!se&#39;</code><code class="php plain">, </code><code class="php string">&quot;&#39;s:&#39;.strlen(&#39;$2&#39;).&#39;:&quot;$2&quot;;&#39;&quot;</code><code class="php plain">, </code><code class="php variable">$serial_str</code> <code class="php plain">); </code></em>
		</div>

		<div class="line number4 index3 alt1">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php variable">$serial_str</code><code class="php plain">= </code><code class="php functions">str_replace</code><code class="php plain">(</code><code class="php string">&quot;r&quot;</code><code class="php plain">, </code><code class="php string">&quot;&quot;</code><code class="php plain">, </code><code class="php variable">$serial_str</code><code class="php plain">); </code></em>
		</div>

		<div class="line number5 index4 alt2">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php keyword">return</code> <code class="php plain">unserialize(</code><code class="php variable">$serial_str</code><code class="php plain">); </code></em>
		</div>

		<div class="line number6 index5 alt1">
			<em><code class="php plain">} </code></em>
		</div>

		<div class="line number8 index7 alt1">
			<em><code class="php comments">// ascii </code></em>
		</div>

		<div class="line number9 index8 alt2">
			<em><code class="php keyword">function</code> <code class="php plain">asc_unserialize(</code><code class="php variable">$serial_str</code><code class="php plain">) { </code></em>
		</div>

		<div class="line number10 index9 alt1">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php variable">$serial_str</code> <code class="php plain">= preg_replace(</code><code class="php string">&#39;!s:(d+):&quot;(.*?)&quot;;!se&#39;</code><code class="php plain">, </code><code class="php string">&#39;&quot;s:&quot;.strlen(&quot;$2&quot;).&quot;:&quot;$2&quot;;&quot;&#39;</code><code class="php plain">, </code><code class="php variable">$serial_str</code> <code class="php plain">); </code></em>
		</div>

		<div class="line number11 index10 alt2">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php variable">$serial_str</code><code class="php plain">= </code><code class="php functions">str_replace</code><code class="php plain">(</code><code class="php string">&quot;r&quot;</code><code class="php plain">, </code><code class="php string">&quot;&quot;</code><code class="php plain">, </code><code class="php variable">$serial_str</code><code class="php plain">); </code></em>
		</div>

		<div class="line number12 index11 alt1">
			<em><code class="php spaces">&nbsp;&nbsp;</code><code class="php keyword">return</code> <code class="php plain">unserialize(</code><code class="php variable">$serial_str</code><code class="php plain">); </code></em>
		</div>

		<div class="line number13 index12 alt2">
			<code class="php plain"><em>}</em> </code>
		</div>
	</blockquote>
</div>

<p>
	2015.06
</p>

<p>
	<a href="http://developer.51cto.com/art/201506/478614_all.htm">JavaScript客户端检测技术详细解析</a><br />
	不错，基本上几个主流的浏览器都涉及到了，方便需要了解客户端浏览器类型跟版本的人学习。
</p>

<p>
	2015.04
</p>

<p>
	<a href="http://database.51cto.com/art/201504/472026_all.htm">SQL注入，你想知道的那些事</a><br />
	很不错的SQL注入的分析文章，值得推荐学习
</p>

<p>
	<a href="http://database.51cto.com/art/201504/471614_all.htm">实战：上亿数据如何秒查</a><br />
	里面有很不错的SQL参考实例，也有解决问题的分析思路。
</p>

<p>
	<a href="http://wangchunhai.blog.51cto.com/225186/1626375">&nbsp;某中介无线组网及VPN接入应用案</a><br />
	分享怎么新建一个VPN的方案，看了像天书，保留一下吧，这个VPN挺有用的说。
</p>

<p>
	2015.03
</p>

<p>
	<a href="http://netsecurity.51cto.com/art/201503/467020_all.htm">ThinkPHP框架安全实现分析</a><br />
	分析了TP的安全处理机制代码，对于安全处理机制了解有一定的帮助噢，推荐阅读。
</p>

<p>
	2015.02
</p>

<p>
	<a href="http://mobile.51cto.com/news-459825.htm">微信公众平台的八大法则</a><br />
	1）鼓励有价值服务；2）消除地理限制；3）消除中介；4）去中心化；5）搭建生态系统；6）公众平台是一个动态系统；7）社交流量；8）所有考虑都会基于一个前提：用户价值第一
</p>

<p>
	<a href="http://network.51cto.com/art/201501/464054_all.htm">来自实例的经典分析--HTTP协议</a><br />
	虽然这部分的文章很多，不过这个还是有点价值的，文中借助Fiddler来分析，也可以当做是Fiddler的入门教程噢。
</p>

<p>
	2015.01
</p>

<p>
	<a href="http://netsecurity.51cto.com/art/201412/462877_all.htm">常见HTTPS攻击方法解析</a><br />
	还不错，说了一些攻击的思路，学习一下还是可以预防攻击噢
</p>

<p>
	2014.12
</p>

<p>
	<a href="http://developer.51cto.com/art/201409/452331.htm">解读大型网站系统架构的演化</a><br />
	很不错的一篇文章，入门可以看~
</p>

<p>
	2014.09
</p>

<p>
	<a href="http://developer.51cto.com/art/201409/452331.htm">解读大型网站系统架构的演化</a><br />
	网站结构的入门文章噢。
</p>

<p>
	<a href="http://www.script-tutorials.com/useful-php-snippets/">有用的PHP代码段（useful php snippets）</a><br />
	1.计算两个左边之间的距离(Calculate the distance between two coordinates);2.PHP调试错误信息发送到邮箱里(Email debug PHP errors);3.PDF转换成JPG图片(Convert PDF to JPG);4.从出生日期获取年龄(Get age by birth date);5.从ZIP文档内解压文件(Extract files from ZIP archive)。
</p>

<p>
	<a href="http://www.open-open.com/bbs/view/1319588511749">11个实用的Apache .htaccess配置</a><br />
	1. 强制后缀反斜杠;2. 防盗链;3. 重定向移动设备;4. 强制浏览器下载指定的文件类型;5. 火狐的跨域名字体嵌入;6. 使用.htaccess缓存 给网站提速;7. 阻止WordPress博客的垃圾评论;8.重定向不同的feed格式到统一的格式;9. 配置网站的HTML5视频;10. 记录PHP错误;11. 在JavaScript代码中运行PHP
</p>

<p>
	<a href="http://mobile.51cto.com/news-450299_all.htm">程序员需要知道的字符知识总结</a><br />
	这篇文章还是不错的，总的讲了编码的来源去脉，对于一些不了解乱码的人来说是不错的文章，看完之后就明白怎么处理乱码了，值得推荐：<span style="text-decoration: underline;">造成乱码的原因就是因为使用了错误的字符编码去解码字节流，因此当我们在思考任何跟文本显示有关的问题时，请时刻保持清醒：当前使用的字符编码是什么。</span>
</p>

<p>
	2014.08
</p>

<p>
	<a href="http://tutorialzine.com/2014/06/10-tips-for-writing-javascript-without-jquery/">不用jQuery写JS的</a><a href="http://tutorialzine.com/2014/06/10-tips-for-writing-javascript-without-jquery/">10条</a><a href="http://tutorialzine.com/2014/06/10-tips-for-writing-javascript-without-jquery/">技巧：10 Tips for Writing JavaScript without jQuery</a><br />
	10条原生JS操作。
</p>

<p>
	<a href="http://mobile.51cto.com/design-447596.htm">手游设计如何给玩家带来愉悦的交互体验</a> 原文链接：<a href="http://colachan.com/post/3380" target="_blank">http://colachan.com/post/3380</a><br />
	分析了一下作为一个吸引人的游戏应该有哪些东西哈~阅读一下
</p>

<p>
	2014.06
</p>

<p>
	<a href="http://netsecurity.51cto.com/art/201406/442694.htm">走进科学：揭秘如何入侵电视机</a><br />
	非常不错的一个智能电视机入侵与破解的教程哈，纯理论看看，具体没机会尝试哈~有机会的朋友记得回访哈~
</p>

<p>
	<a href="http://developer.51cto.com/art/201406/442328.htm">265行代码实现第一人称游戏引擎</a><br />
	文章内阐述了光线等算法，还不错噢。
</p>

<p>
	2014.05
</p>

<p>
	<a class="meta-title" href="http://blog.jobbole.com/67914/" target="_blank" title="Android开发贴士集合（1）">Android开发贴士集合（1）</a> <a class="meta-title" href="http://blog.jobbole.com/67920/" target="_blank" title="Android开发贴士集合（2）">Android开发贴士集合（2）</a> <a class="meta-title" href="http://blog.jobbole.com/67928/" target="_blank" title="Android开发贴士集合（3）">Android开发贴士集合（3）</a><br />
	这个系列非常不错，原因在于作者将在项目中使用的对象都罗列出来了~然后你可以根据喜欢的进去学习，然后运用了~
</p>

<p>
	<a href="http://act.life.alipay.com/shopping/before/help/index.html">支付宝帮助:怎么注册担保交易及及时到账服务</a><br />
	这个是官方帮助，告诉你怎么申请一个商家工具，怎么集成，什么申请条件等等。
</p>

<p>
	<a href="http://database.51cto.com/art/201404/437288.htm">零售行业的数据挖掘七步走</a><br />
	数据挖掘的一点点思路呀，看一下触类旁通吧~
</p>

<p>
	2014.04
</p>

<p>
	<a href="http://developer.51cto.com/art/201104/255208.htm">ASP.NET的HTTP模块和处理程序之处理程序的执行</a><br />
	这个干货还是可以的啦~学习学习
</p>

<p>
	<a href="http://mobile.51cto.com/abased-435463.htm">关于Android配色 自适应颜色的实现</a><br />
	文章里面述说着怎么使用<a href="http://lokeshdhakar.com/projects/color-thief/">Color Thief</a>获取图片调色板，你可以指定你喜欢的图片噢。其他的没什么随便看看吧~
</p>

<p>
	2014.03
</p>

<p>
	<a href="http://www.geekfan.net/7509/">下载youtube上视频的姿势大全</a><br />
	昨天为了下载YouTUBE上的一个wordpress模板主题的设置教程，真的是哭天山地，找了好多的网站，都是没用，下载了好几个软件都是乱七八糟的，就是没有可以用的，最后才找到一个YTD可以稍微能用一下，然后不死心重新找了一下才看到这篇文章，很好，很强大！里面的寄居蟹真的需要排队，其他的基本上就不用看了，不能用，而<a href="https://addons.mozilla.org/zh-cn/firefox/addon/greasemonkey" target="_blank">Greasemonkey</a>（<a href="http://userscripts.org/scripts/show/25105" target="_blank">Download YouTube Videos as MP4</a>）插件还是真的可以用，超值的推荐。
</p>

<p>
	<a href="http://isux.tencent.com/android-ui-9-png.html" title="Permanent Link to Android设计中的.9.png">Android设计中的.9.png</a><br />
	很好的介绍了.9.png这种文件格式的设计与使用......就是没有实际的android的应用教程噢
</p>

<p>
	<a href="http://mobile.51cto.com/news-433035.htm">9招教你如何设计一款电商app,并实现使用量170%增长</a><br />
	虽然这篇文章看似软文，不过里面提到的9个点还是不错的：1. 对于要显示的信息考虑好清晰的优先级；2. 交易按钮要总在屏幕显示之上；3. 内容容易接触，步骤简单；4. 在移动端要使用聊天的方式沟通；5. 注册页面要短；6. 内容为先，描述次之；7.对于iOS应用来说，左边是导航，右边是交易；8. 心要比星好；9. 为设计流程写文档
</p>

<p>
	<a href="http://www.cnblogs.com/TomXu/archive/2011/11/22/2258849.html">最锋利的Visual Studio Web开发工具扩展：Web Essentials详解</a><br />
	Visual Studio 2013&nbsp;的扩展工具的使用噢，强大的工具就是为了提高开发效率
</p>

<p>
	<a href="http://www.nowamagic.net/librarys/veda/detail/2499">PHP CodeBase: 判断用户是否手机访问</a><br />
	收藏一下哈~这个手机判断的现在开发什么系统都是需要用到的，手机版的系统现在很流行哈~PS：站长的PHP CodeBase每一个都是为了解决一个问题而存在噢，推荐看看
</p>

<p>
	<a href="http://mobile.51cto.com/news-429774.htm">亚文化是产品经理必修课</a><br />
	摘抄：要做出让年轻人热爱的产品，关键是要到年轻人第一现场去。今天，亚文化群体年轻人很重要的现场，年轻人现在消费的不是简单的功能，不是简单的品牌，他消费的是参与感。
</p>

<p>
	<a href="http://www.planeart.cn/?p=1121">再谈javascript图片预加载技术-比onload更快获取图片尺寸</a><br />
	很棒的分析，很棒的思路~
</p>

<p>
	<a href="http://developer.51cto.com/art/201310/413113.htm">大型网站负载均衡架构</a><br />
	介绍文~长见识
</p>

<p>
	<a href="http://developer.51cto.com/art/201402/430429.htm">成为一个PHP专家：缺失的环节</a><br />
	不错的介绍噢。令我感觉有价值的在于：<em>如果没有什么事可以做，试着创造一些</em>。<br />
	总是有事可做。永远不要说&ldquo;我没有项目可做&rdquo;，或者更糟的&ldquo;我很无聊&rdquo;。如果你没有一个正在进行的项目可以做&mdash;那就创造一个。你每天使用的工具是否让你感到受挫因为它不完善的功能？自己做出一个更好的！对新产品没有想法？那就复制一个现有的&mdash;试着重建一个基本的FaceBook，重建一些你已经知道了的，为了能够实践一下。
</p>

<p>
	<a href="http://www.woshipm.com/zhichang/63198.html">程序员接触不到大项目，该如何提高自己？</a><br />
	以前我也这么想，后来我醒悟了，大项目也是有小项目开始的，这篇文章说的不错~推荐
</p>

<p>
	<a href="http://www.woshipm.com/pmd/70331.html">没读过设计院校，如何成为设计师</a><br />
	这个文章不错，很多人怨天怨地，看看就不会了~
</p>

<p>
	<a href="http://www.woshipm.com/it/60678.html">张小龙神话已破灭 马化腾该接管微信了</a><br />
	里面有说到一些APP的开发灵感噢~~
</p>

<p>
	<a href="http://netsecurity.51cto.com/art/201402/430610.htm">颤抖了吗?九步逆向破解银行安全令牌</a><br />
	又是一篇好文，想要破解或者反编译Android APP的同学可以好好的看一下。
</p>

<p>
	<a href="http://mobile.51cto.com/ahot-426035.htm">Android开发者必备的42个链接</a><br />
	很多Android开发资源的链接
</p>

<p>
	2014.02
</p>

<p>
	<a href="http://my.oschina.net/mingtingling/blog?catalog=263852">怎么一步步编写简单的PHP的Framework</a><br />
	想写一个自己的PHP框架？那么从这边入门吧~作者一步步的教大家怎么开发一个PHP框架~
</p>

<p>
	<a href="http://developer.51cto.com/art/201401/428321.htm">只有20行Javascript代码！手把手教你写一个页面模板引擎</a> 原文<a href="http://tech.pro/tutorial/1743/javascript-template-engine-in-just-20-lines">Javascript template engine in just 20 lines</a><br />
	很不错的讲解了怎么去设计一个模板引擎
</p>

<p>
	<a href="http://www.vaikan.com/reverse-engineering-a-d-link-backdoor/">我是如何反编译D-Link路由器固件程序并发现它的后门的</a><br />
	分析怎么去反编译固件......
</p>

<p>
	<a href="http://www.vaikan.com/have-you-ever-chatted-with-a-hacker-within-a-virus/">中国的黑客究竟有多张狂？</a><br />
	挺不错的一个故事...或许能看到一点点别人的工作的技巧
</p>

<p>
	<a href="http://www.vaikan.com/how-to-hack-a-wordpress-site-using-sql-injection/">实战演示黑客如何利用SQL注入漏洞攻破一个WordPress网站</a><br />
	比较基础，还很详细的入侵教程~
</p>

<p>
	2013.12
</p>

<p>
	<a href="http://mux.baidu.com/?p=5149">浅析专题中的构图之美</a><br />
	非常棒的一篇文章，从构图的角度上来开讲，里面拒了很多的专题页的设计样例，推荐阅读。
</p>

<p>
	<a href="http://www.uisdc.com/ps-rotating-skills">【PS旋转技巧】让像素完美呈现</a><br />
	在PS中旋转我们的图片，你会发现很多时候你的图片会变得模糊，转的越多次就越模糊，为什么？看完这篇就知道原理还有要怎么解决了噢。他提供一个办法解决：在执行变形工具的时候，将旋转中心选择在左上方（或者任何一个角落都可以）<img alt="perfectPixel_06-1" border="0" height="23" src="http://blog.ipodmp.com/wp-content/uploads/2015/07/perfectPixel_06-1_thumb.png" title="perfectPixel_06-1" width="22" />，如此一来即可解决这个问题。
</p>

<p>
	<a href="http://andy360.blog.51cto.com/8208325/1325411">【51CTO】从假装在腾讯，到真的360 &mdash;&mdash; 一个应届准PM的独白（面经干货）</a><br />
	作者写的面试的经验，其中涉及到简历的撰写等内容，挺不错的。
</p>

<p>
	<a href="http://www.36kr.com/tag/KPCB">【36氪】KPCB CEO课程</a>
</p>

<p>
	<a href="http://bbs.taobao.com/catalog/thread/1338197-263553669.htm?spm=0.0.0.0.Lln4A8">【经验干货】淘宝UED发布：套路式Banner的排版</a><br />
	曾经看过一个高手说淘宝那边的设计十有八九都是用套路做出来的，然后看了很多的实例觉得确实是，但是苦于没有一个专门的人在传授这个套路式的设计哈~那么现在有了！这篇就是在教你怎么使用套路来设计你的Banner哈~事后还提供了淘宝内的模板噢。
</p>

<p>
	<a href="http://developer.51cto.com/art/201311/416277.htm">增加转化率的五大劝导式网页设计技巧</a>
</p>

<p>
	<a href="http://developer.51cto.com/art/201310/414327.htm">提高Banner广告点击率的14个设计建议</a>
</p>

<p>
	<a href="http://developer.51cto.com/art/201311/416934.htm">做个全能设计师！教你写作并设计出一个杀手级标题</a>
</p>

<p>
	<a href="http://developer.51cto.com/art/201309/411522.htm">如何培养良好的设计习惯：把握细节</a>很多时候经常会觉得为什么我设计的跟他的设计一样，但是为什么他人看起来很好看，就是比自己设计得要有感觉，看完这个文章你就知道，不是什么问题，仅仅是因为缺少了细节，好好看看吧~让我们的设计不再平庸。
</p>

<p>
	<a href="http://developer.51cto.com/art/201309/411886.htm">谁说设计师不会写代码？&mdash;Photoshop脚本语言简介</a><br />
	PS是内置Javascript引擎的，这个JS不是一般意义上的JS而是微软提供的JS引擎，用于自动化处理办公的脚本宿主引擎，很棒，很简单！也不会很难，学习了会加快设计的效率，比如PS里面的动作大家都有用过吧~但是，这篇文章个人觉得吧，顶多是入门，说的不深入，应该是适合入门的初学者看。
</p>

<p>
	<a href="http://developer.51cto.com/art/201309/411363.htm">9月典藏！50个超赞的免费设计师工具资源下载</a> 　　(原文 <a href="http://www.webdesignerdepot.com/2013/09/50-fantastic-free-tools-for-designers/">50-fantastic-free-tools-for-designers</a>)
</p>

<p>
	2013.10
</p>

<p>
	<a href="http://www.sevenforums.com/virtualization/19604-how-copy-virtual-xp-machine.html">Windows 7: How to Copy a Virtual XP machine（Windows 7:如何复制XP虚拟机（运行多份XP mode虚拟机） ）</a><br />
	自从用起Win7真的好不习惯，这东西不能用那东西不能用，特别是做设计得有需要测试兼容，咋办？！安装XP吧~在Win7里面安装哈~这个XP mode虚拟机非常棒，你安装一份，根据教程可以弄出好多分，然后你就可以这个安装ie6，那个安装ie8反正想安装就安装，不会影响win7系统的使用噢。
</p>

<p>
	<a href="http://www.w3cplus.com/css/create-shapes-with-css">纯CSS制作的图形效果</a> [ <a href="http://css-tricks.com/examples/ShapesOfCSS/">The Shapes of CSS</a>&nbsp;<a href="http://www.paulund.co.uk/how-to-create-different-shapes-in-css">How To Create Different Shapes In CSS</a> ]<br />
	朋友问我懂不懂用CSS设计扇形图，我哇咧，三角形我是会，扇形不会啦，查了一下，还真的有这方面的高手在分享这类资讯。但是唯一不好的是这个还是有局限的，没办法真的用来做统计，要设计那种的话看样子还是需要JS绘图才行噢。
</p>

<p>
	<a href="www.chinaz.com/web/2013/1016/322253.shtml">网络品牌运营的十二点思考（上）</a>
</p>

<p>
	<a href="http://www.chinaz.com/web/2013/1016/322256.shtml" target="_blank">网络品牌运营的十二点思考（下）</a>
</p>

<p>
	2013.05
</p>

<p>
	<a href="http://www.csdn.net/article/2012-12-17/2812936-CMDN-23th">信恩科技创始人林兴陆：QR Code二维码的前世今生</a>
</p>