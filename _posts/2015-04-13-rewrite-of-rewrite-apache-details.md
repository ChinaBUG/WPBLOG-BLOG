---
ID: 3995
post_title: rewrite-Apache的rewrite详解
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/04/rewrite-of-rewrite-apache-details.html
published: true
post_date: 2015-04-13 14:41:22
---
<em><span style="text-decoration: underline;">虫曰：这篇解释的听详细的，推荐阅读。</span></em>

Apache的rewrite模块，提供了一个基于规则的重写(rewrite,也许译为重构更为合适)引擎，来实时重写发送到Apache的请求URL。因功能极其强大，被称为URL重写的“瑞士军刀”。
<a name="more"></a>

这个模块使用一个基于正则表达式解析器开发的重写引擎，根据web管理员定义的规则来实时(on the fly)重写请求URL。它支持任意数目的重写规则，以及附加到一条规则上的任意数目的规则条件，从而提供了一套非常灵活和功能强大的URL处理机制。 URL处理操作的实施与否，依赖于各种各样的条件检查，如检查服务器变量、环境变量、HTTP头字段、时间戳的值，甚至外部数据库的检索结果。这个模块可 以在服务器范围内(http.conf)、目录范围内(.htaccess)或请求串(query-string)的一部分处理有关的URL。重写的结果 URL，可以指向一个站内的处理程序、指向站外的重定向或者一个站内的代理。与灵活和功能强大相随的是设置的复杂，别指望一天内弄明白整个模块。(所以， 这个学习笔记也分了几部分：)
<h4>内部处理过程</h4>
<strong>API阶段</strong>
首先，Apache处理HTTP请求是分阶段进行的,Apache API为每个阶段提供了一个钩子(hook)。Mod_rewrite使用了其中的两个钩子：一个用来在HTTP请求被读取但还没有访问授权验证之前进行 URL_to_filename转换，一个用来在授权验证完成且目录设置文件(.htaccess)读取之后、但内容处理器(content handler)被调用之前激化，进行修补(fixup).因此，当一个请求到达，Apache决定了相关的服务器（或虚拟服务器）以后进行 URL_to_filename阶段，重写引擎(rewrite engine)开始处理服务器设置中的重写指令(mod_rewrite directives).接下来几个阶段过后进入修补阶段，此时最终的数据所在的物理目录已经找到，目录配置中的重写指令开始执行。在这两个阶 段，mod_rewrite都是将URL重写为新的URL或文件名，所以看起来并没有明显的区别。对API的这种应用，并不是一开始就是这样设计的，而是 Apache1.x不得已而为之。为了搞清这个问题，以下两点需要记住。
1)虽然mod_rewrite能进行URL到URL、URL到文件 名字甚至文件名字到文件名字的转换，API(1.x)目前提供了一个URL_to_filename转换。在Apache2.0中，这两个钩子会被加进 去，整个过程会更加清晰。一个事实必须清楚的记得：Apache在URL_to_filename钩子中，做得比API设计的功能更多。
2) 不可思议的是，mod_rewrite能在目录范围内（如根据.htaccess文件的指令配置）进行URL处理，虽然URL很早就已经被转换为文件名字 了。只所以会如此，是因为.htaccess文件存在于文件系统中。也就是说，在这个阶段来进行URL处理，是非常晚的时候了。为了解决这个"先有鸡还是 先有蛋"的问题，mod_rewrite用了一个小技巧：当在目<ins>录</ins>范围内处理URL/filename时，mod_rewrite 先将文件名逆转回相关的URL(虽然通常是不可能的，但请参见下面用以实现这个技巧的RewriteBase指令)，然后据这个新URL生成一个站内的子 请求(internal sub-request)，这又重开始了API进程。Mod_rewrite尽量使这些复杂的步骤对用户透明，但应要记住：虽然目录范围URL的真正处理 过程很快很高效，但这一阶段会因为这个"鸡和蛋"的问题而变得很慢和低效。从另一方面来看，这也是mod_rewrite提供给普通用户进行目录范围内的 URL处理的唯一途径.
<strong>规则集(RewriteRule指令集合)处理过程</strong>
当 mod_rewrite在上述的两个API阶段被激活时，它会从它的配置数据结构（在开始服务器上下文(per-server context)或目录上下文(per-directory context)时创建的）中读取配置的规则集，然后URL重写引擎启动来执行包含的规则集（一个或多条规则以及它们的条件）。两种上下文中的处理过程都 是一样的，差别只是在最后的结果处理过程上。
规则集中规则的顺序是非常重要的，因为重写引擎以特定的顺序来处理它们。重写引擎顺序遍历规则 集，当一条规则匹配时，引擎会去遍历与它相关的条件集(RewriteCond指令集合).由于历史的原因，条件集先被列出来，因此控制流流程有点曲折 (long-winded).如图一所示：<img src="http://hedong.3322.org/archives/pics/mod_rewrite_fig1.gif" alt="mod_rewrite_fig1.gif" width="428" height="385" align="right" border="0" />
正如所看到的，首先URL会与每条规则的模板(pattern)比较，当匹配失败时，立即停止对当前规则的处理进入下一条规则。当匹配成功 时，mod_rewrite寻找相关的规则条件。如果找不到相关的条件，则直接执行规则中定义的替换，然后回到规则遍历的过程。如果找到了相关的条件，则 启动一个内部循环，依次检查各个条件。对于检查，我们不是拿一个模板来匹配当前的URL，而是先创建一个TestString串，将串内的变量、后向引用 (bakc-reference)、查询结果(map lookups)等展开，然后用这个TestString和条件式中的CondPattern进行匹配，如果匹配失败，则整个条件集且这个规则都不再执 行，重要回到规则遍历中；如果匹配成功，则检查下一个条件，如果所有的条件都满足，则执行规则中定义的替换动作。
<strong>特殊字符的转义</strong>
既然基于正则式，则当然会有特殊字符的问题。在1.3.20版本的Apache中，通过在特殊字符前加一个“\”来将TestString或Sustitution串的特殊字符转义。
<strong>正则式的后向引用</strong>
<img src="http://hedong.3322.org/archives/pics/mod_rewrite_fig2.gif" alt="mod_rewrite_fig2.gif" width="381" height="179" align="left" border="0" />　　有一点需要记住：一旦在模板(pattern)或条件模板(CondPattern)中使用了括号，则后向引用已经自动产生了，你可以在Sustitution或TestString中通过$N或%N来引用相关的值。如图，描述了后向引用的值可以传到的位置。
<h4>配置指令(Configuration Directives)</h4>
<table border="1">
<tbody>
<tr>
<td>指令</td>
<td>语法</td>
<td>默认值</td>
<td>说明</td>
<td>备注</td>
</tr>
<tr>
<td>RewriteEngine</td>
<td>RewriteEngine on|off</td>
<td>Off</td>
<td>开关重构引擎</td>
<td>默认时不能继承，故每个虚拟主机都要有自己的开关指令。</td>
</tr>
<tr>
<td>RewriteOptions</td>
<td>RewriteOptions Option</td>
<td>MaxRedirects=10</td>
<td>设置一些特殊参数</td>
<td>inherit:配置是否继承，MaxRedirects=number:内部重定向次数</td>
</tr>
<tr>
<td>RewriteLog</td>
<td>RewriteLog file-path</td>
<td>None</td>
<td>设定重写log文件</td>
<td>用RewriteLogLevel 0来禁止日志</td>
</tr>
<tr>
<td>RewriteLogLevel</td>
<td>RewriteLogLevel Level</td>
<td>RewriteLogLevel 0</td>
<td>设置日志级别</td>
<td>0表示没有，2以上用于debug，9及以上表示全部信息</td>
</tr>
<tr>
<td>RewriteLock</td>
<td>RewriteLock file-path</td>
<td>None</td>
<td>设置RewriteMap程序的同步锁文件</td>
<td>要求是本地文件，此文件只对rewriting map-program有效。</td>
</tr>
<tr>
<td>RewriteMap</td>
<td>RewriteMap MapName MapType:MapSource</td>
<td>Notused per default</td>
<td>定义重写影射</td>
<td>具体说明参见<a href="http://hedong.3322.org/archives/000346.html"><span style="color: #1d58d1;">文档</span></a></td>
</tr>
<tr>
<td>RewriteBase</td>
<td>RewriteBase URL-path</td>
<td>physical directory path</td>
<td>设置目录范围内重写的基本URL</td>
<td>具体说明参见<a href="http://hedong.3322.org/archives/000346.html"><span style="color: #1d58d1;">文档</span></a></td>
</tr>
<tr>
<td>RewriteCond</td>
<td>RewriteCond TestString CondPattern</td>
<td>None</td>
<td>定义规则条件</td>
<td>具体说明参见<a href="http://hedong.3322.org/archives/000345.html"><span style="color: #1d58d1;">文档</span></a></td>
</tr>
<tr>
<td>RewriteRule</td>
<td>RewriteRule Pattern Substitution</td>
<td>None</td>
<td>定义重写规则</td>
<td>具体说明参见<a href="http://hedong.3322.org/archives/000344.html"><span style="color: #1d58d1;">文档</span></a></td>
</tr>
</tbody>
</table>
参考资料：
<a href="http://httpd.apache.org/docs/mod/mod_rewrite.html"><span style="color: #1d58d1;">http://httpd.apache.org/docs/mod/mod_rewrite.html</span></a>
<h3 id="startcontent" class="title">Apache的Mod_rewrite学习（二）</h3>
今天学习重写规则的语法。

<a name="more"></a>

<strong>RewriteRule</strong>
Syntax: RewriteRule Pattern Substitution [flags]
一条RewriteRule指令，定义一条重写规则，规则间的顺序非常重要。对Apache1.2及以后的版本，模板(pattern)是一个 POSIX正则式，用以匹配当前的URL。当前的URL不一定是用记最初提交的URL，因为可能用一些规则在此规则前已经对URL进行了处理。
对mod_rewrite来说，！是个合法的模板前缀，表示“非”的意思，这对描述“不满足某种匹配条件”的情况非常方便，或用作最后一条默认规则。当使用！时，不能在模板中有分组的通配符，也不能做后向引用。
当匹配成功后，Substitution会被用来替换相应的匹配，它除了可以是普通的字符串以外，还可以包括：
<ol>
	<li>$N,引用RewriteRule模板中匹配的相关字串，N表示序号,N=0..9</li>
	<li>%N,引用最后一个RewriteCond模板中匹配的数据，N表示序号</li>
	<li>%{VARNAME},服务器变量</li>
	<li>${mapname:key|default},映射函数调用</li>
</ol>
这些特殊内容的扩展，按上述顺序进行。
一个URL的全部相关部分都会被Substitution替换，而且这个替换过程会一直持续到所有的规则都被执行完，除非明确地用L标志中断处理过程。
当susbstitution有”-”前缀时，表示不进行替换，只做匹配检查。
利用RewriteRule，可定义含有请求串(Query String)的URL，此时只需在Sustitution中加入一个？，表示此后的内容放入QUERY_STRING变量中。如果要清空一个 QUERY_STRING变量，只需要以？结束Substitution串即可。
如果给一个Substitution增加一个http://thishost[:port]的前缀，则mod_rewrite会自动将此前缀去掉。因此，利用http://thisthost做一个无条件的重定向到自己，将难以奏效。要实现这种效果，必须使用R标志。
Flags是可选参数，当有多个标志同时出现时，彼此间以逗号分隔。
<ol>
	<li>'redirect|R [=code]' (强制重定向)
给当前的URI增加前缀 http://thishost[:thisport]/， 从而生成一个新的URL，强制生成一个外部重定向(external redirection，指生的URL发送到客户端，由客户端再次以新的URL发出请求，虽然新URL仍指向当前的服务器). 如果没有指定的code值，则HTTP应答以状态值302 (MOVED TEMPORARILY)，如果想使用300-400（不含400）间的其它值可以通过在code的位置以相应的数字指定，也可以用标志名指定： temp (默认值), permanent, seeother.
注意，当使用这个标志时，要确实substitution是个合法的URL，这个标志只是在URL前增加http://thishost[:thisport]/前缀而已，重写操作会继续进行。如果要立即将新URL重定向，用L标志来中重写流程。</li>
	<li>'forbidden|F' (强制禁止访问URL所指的资源)
立即返回状态值403 (FORBIDDEN)的应答包。将这个标志与合适的RewriteConds 联合使用，可以阻断访问某些URL。</li>
	<li>'gone|G' (强制返回URL所指资源为不存在(gone))
立即返回状态值410 (GONE)的应答包。用这个标志来标记URL所指的资源永久消失了.</li>
	<li># 'proxy|P' (强制将当前URL送往代理模块（proxy module）)
这个标志，强制将substitution当作一个发向代理模块的请求，并立即将共送往代理模块。因此，必须确保substitution串是一个合法 的URI (如, 典型的情况是以http://hostname开头)，否则会从代理模块得到一个错误. 这个标志，是ProxyPass指令的一个更强劲的实现，将远程请求(remote stuff)映射到本地服务器的名字空间(namespace)中来。
注意，使用这个功能必须确保代理模块已经编译到Apache 服务器程序中了. 可以用“httpd -l ”命令，来检查输出中是否含有mod_proxy.c来确认一下。如果没有，而又需要使用这个功能，则需要重新编译``httpd''程序并使用 mod_proxy有效。</li>
	<li>'last|L' (最后一条规则)
中止重写流程，不再对当前URL施加更多的重写规则。这相当于perl的last命令或C的break命令。</li>
	<li>'next|N' (下一轮)
重新从第一条重写规则开始执行重写过程，新开的过程中的URL不应当与最初的URL相同。 这相当于Perl的next命令或C的continue命令. 千万小心不要产生死循环。</li>
	<li># 'chain|C' (将当前的规则与其后续规则??绑(chained))
当规则匹配时，处理过程与没有??绑一样；如果规则不匹配，则??绑在一起的后续规则也不在检查和执行。</li>
	<li>'type|T=MIME-type' (强制MIME类型)
强制将目标文件的MIME-type为某MIME类型。例如，这可用来模仿mod_alias模块对某目录的ScriptAlias指定，通过强制将该目录下的所有文件的类型改为 “application/x-httpd-cgi”.</li>
	<li>'nosubreq|NS' (used only if no internal sub-request )
这个标志强制重写引擎跳过为内部sub-request的重写规则.例如，当mod_include试图找到某一目录下的默认文件时 (index.xxx)，sub-requests 会在Apache内部发生. Sub-requests并非总是有用的，在某些情况下如果整个规则集施加到它上面，会产生错误。利用这个标志可排除执行一些规则。</li>
	<li>'nocase|NC' (模板不区分大小写)
这个标志会使得模板匹配当前URL时忽略大小写的差别。</li>
	<li>'qsappend|QSA' (追加请求串(query string))
这个标志，强制重写引擎为Substitution的请求串追加一部分串，则不是替换掉原来的。借助这个标志，可以使用一个重写规则给请求串增加更多的数据。</li>
	<li>'noescape|NE' (不对输出结果中的特殊字符进行转义处理)
通常情况下，mod_write的输出结果中，特殊字符（如'%', '$', ';', 等)会转义为它们的16进制形式(如分别为'%25', '%24', and '%3B'）。这个标志会禁止mod_rewrite对输出结果进行此类操作。 这个标志只能在 Apache 1.3.20及以后的版本中使用。</li>
	<li>'passthrough|PT' (通过下一个处理器)
这个标志 强制重写引擎用filename字段的值来替换内部request_rec数据结构中uri字段的值。. 使用这个标志，可以使后续的其它URI－to-filename转换器的Alias、ScriptAlias、Redirect等指令，也能正常处理 RewriteRule指令的输出结果。用一个小例子来说明它的语义：如果要用mod_rewrite的重写引擎将/abc转换为/def,然后用 mod_alas将/def重写为ghi，则要：
RewriteRule ^/abc(.*) /def$1 [PT]
Alias /def /ghi
如 果PT标志被忽略，则mod_rewrite也能很好完成工作,如果., 将 uri=/abc/... 转换为filename=/def/... ，完全符合一个URI-to-filename转换器的动作。接下来 mod_alias 试图做 URI-to-filename 转换时就会出问题。
注意:如果要混合都含有URL－to-filename转换器的不同的模块的指令，必须用这个标志。最典型的例子是mod_alias和mod_rewrite的使用。</li>
	<li>'skip|S=num' (跳过后面的num个规则)
当前规则匹配时，强制重写引擎跳过后续的num个规则。用这个可以来模仿if-then-else结构：then子句的最后一条rule的标志是skip=N，而N是else子句的规则条数。</li>
	<li>'env|E=VAR:VAL' (设置环境变量)
设置名为VAR的环境变量的值为VAL,其中VAL中可以含有正则式的后向引用($N或%N)。这个标志可以使用多次，以设置多个环境变量。这儿设置的 变量，可以在多种情况下被引用，如在XSSI或CGI中。另外，也可以在RewriteCond模板中以%{ENV:VAR}的形式被引用。</li>
	<li></li>
</ol>
注意：一定不要忘记，在服务器范围内的配置文件中，模板(pattern)用以匹配整个URL;而在目录范围内的配置文件中，目录前缀总是 被自动去掉后再进行模板匹配的，且在替换完成后自动再加上这个前缀。这个功能对很多种类的重写是非常重要的，因为如果没有去前缀，则要进行父目录的匹配， 而父目录的信息并不是总能得到的。一个例外是，当substitution中有http://打头时，则不再自动增加前缀了，如果P标志出现，则会强制转 向代理。
注意：如果要在某个目录范围内启动重写引擎，则需要在相应的目录配置文件中设置“RewriteEngine on”，且目录的“Options FollowSymLinks”必须设置。如果管理员由于安全原因没有打开FollowSymLinks，则不能使用重写引擎。
<h3 id="startcontent" class="title">Apache的Mod_rewrite学习（三）</h3>
今天学习重写规则的条件。

<a name="more"></a>

<strong>RewriteCond</strong>
Syntax: RewriteCond TestString CondPattern [flags]
RewriteCond指令定义一条规则条件。在一条RewriteRule指令前面可能会有一条或多条RewriteCond指令，只有当自身的模板(pattern)匹配成功且这些条件也满足时规则才被应用于当前URL处理。
TestString是一个字符串，除了包含普通的字符外，还可以包括下列的可扩展结构：
<ol>
	<li>$N,RewriteRule后向引用，其中(0 &lt;= N &lt;= 9)
$N引用紧跟在RewriteCond后面的RewriteRule中模板中的括号中的模板在当前URL中匹配的数据。</li>
	<li>%N,RewriteCond后向引用，其中(0 &lt;= N &lt;= 9)
%N引用最后一个RewriteCond的模板中的括号中的模板在当前URL中匹配的数据。</li>
	<li>${mapname:key|default},RewriteMap扩展.
具体参见RewriteMap</li>
	<li>%{ NAME_OF_VARIABLE } ,服务器变量。
变量的名字如下表（分类显示）
<table border="1">
<tbody>
<tr>
<td>HTTP headers:</td>
<td>connection &amp; request:</td>
<td>server internals:</td>
<td>system stuff:</td>
</tr>
<tr>
<td>HTTP_USER_AGENT</td>
<td>REMOTE_ADDR</td>
<td>DOCUMENT_ROOT</td>
<td>TIME_YEAR</td>
</tr>
<tr>
<td>HTTP_REFERER</td>
<td>REMOTE_HOST</td>
<td>SERVER_ADMIN</td>
<td>TIME_MON</td>
</tr>
<tr>
<td>HTTP_COOKIE</td>
<td>REMOTE_USER</td>
<td>SERVER_NAME</td>
<td>TIME_DAY</td>
</tr>
<tr>
<td>HTTP_FORWARDED</td>
<td>REMOTE_IDENT</td>
<td>SERVER_ADDR</td>
<td>TIME_HOUR</td>
</tr>
<tr>
<td>HTTP_HOST</td>
<td>REQUEST_METHOD</td>
<td>SERVER_PORT</td>
<td>TIME_MIN</td>
</tr>
<tr>
<td>HTTP_PROXY_CONNECTION</td>
<td>SCRIPT_FILENAME</td>
<td>SERVER_PROTOCOL</td>
<td>TIME_SEC</td>
</tr>
<tr>
<td>HTTP_ACCEPT</td>
<td>PATH_INFO</td>
<td>SERVER_SOFTWARE</td>
<td>TIME_WDAY</td>
</tr>
<tr>
<td></td>
<td>QUERY_STRING</td>
<td></td>
<td>TIME</td>
</tr>
<tr>
<td></td>
<td>AUTH_TYPE</td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
<table border="1">
<tbody>
<tr>
<td>specials:</td>
<td>说明</td>
</tr>
<tr>
<td>API_VERSION</td>
<td>Apache与模块间的接口的版本号</td>
</tr>
<tr>
<td>THE_REQUEST</td>
<td>客户端发送到来的HTTP请求行的整行信息，不含其它的头字段信息，如（"GET /index.html HTTP/1.1")</td>
</tr>
<tr>
<td>REQUEST_URI</td>
<td>HTTP请求行中请求的资源</td>
</tr>
<tr>
<td>REQUEST_FILENAME</td>
<td>请求中对应的服务器本地文件系统中全路径文件名</td>
</tr>
<tr>
<td>IS_SUBREQ</td>
<td>根据是否为SubRequest,分别值为”true”或”false”</td>
</tr>
</tbody>
</table>
特别说明：
<ul>
	<li>SCRIPT_FILENAME 和REQUEST_FILENAME变量含有相同的值，也就是Apache服务器内部数据结构request_rec的filename字段的值。第一个 变量是一个CGI变量，而第二个则与REQUEST_URI(含有request_rec数据结构中uri字段的值)保持一致。</li>
	<li>%{ENV:variable}中的variable可以是任何环境变量的名字。对其值的查找，先通过Apache内部的数据结构，（如找不到）再在Apache服务器进程中通过getenv()查找。</li>
	<li>%{HTTP:header}中的header可以是任何HTTP MIME-header的名字，其值通过查找HTTP请求信息而得。</li>
	<li>%{LA-U:variable} 用来引用后续API阶段中定义的、当前还不知道的值，具体实现是通过执行一个基于URL的内部的sub-request来决定的variable的最终的 值。例如，假如你想在服务器范围内利用REMOTE_USER的值来完成重写，但这个值是在验证阶段设置的，而验证阶段是在URL转换阶段的后面。从另一 方面讲，由于mod_rewrite在修补(fixup)API阶段进行目录范围的重写，而修补阶段在验证阶段的后面，所以此时只要用% {REMOTE_USER}就可以取得该值了。</li>
	<li>%{LA-F:variable}，执行一个基于文件名字(filename)的内部sub-request来决定variable的最终的值。大多数时间内，这和LA-U相同。</li>
</ul>
</li>
</ol>
CondPattern是一个条件模板，也就是说，是一个扩展正则式（extended regular expression），用与跟TestString进行匹配。作为一个标准的扩展正则式，CondPattern有以下补充：
<ol>
	<li>可以在模板串前增加一个!前缀，以用表示不匹配模板。但并不是所有的test都可以加！前缀。</li>
	<li>CondPattern中可以使用以下特殊变量：
<ul>
	<li>' 将condPattern当作一个普通字符串，将它和TestString进行比较，当TestString 的字符小于CondPattern为真.</li>
	<li>'&gt;CondPattern' (大于)
将condPattern当作一个普通字符串，将它和TestString进行比较，当TestString 的字符大于CondPattern为真.</li>
	<li>'=CondPattern' (等于)
将condPattern当作一个普通字符串，将它和TestString进行比较，当TestString 与CondPattern完全相同时为真.如果CondPattern只是 "" (两个引号紧挨在一起) 此时需TestString 为空字符串方为真.</li>
	<li>'-d' (是否为目录)
将testString当作一个目录名，检查它是否存在以及是否是一个目录.</li>
	<li>'-f' (是否是regular file)
将testString当作一个文件名，检查它是否存在以及是否是一个regular文件.</li>
	<li>'-s' (是否为长度不为0的regular文件)
将testString当作一个文件名，检查它是否存在以及是否是一个长度大于0的regular文件</li>
	<li>'-l' (是否为symbolic link)
将testString当作一个文件名，检查它是否存在以及是否是一个 symbolic link.</li>
	<li>'-F' (通过subrequest来检查某文件是否可访问)
检查TestString是否是一个合法的文件，而且通过服务器范围内的当前设置的访问控制进行访问。这个检查是通过一个内部subrequest完成的, 因此需要小心使用这个功能以降低服务器的性能。</li>
	<li>'-U' (通过subrequest来检查某个URL是否存在)
检查TestString是否是一个合法的URL，而且通过服务器范围内的当前设置的访问控制进行访问。这个检查是通过一个内部subrequest完成的, 因此需要小心使用这个功能以降低服务器的性能。</li>
</ul>
</li>
</ol>
[flags]是第三个参数，多个标志之间用逗号分隔。
<ol>
	<li>'nocase|NC' (不区分大小写)
在扩展后的TestString和CondPattern中，比较时不区分文本的大小写。注意，这个标志对文件系统和subrequest检查没有影响.</li>
	<li>'ornext|OR' (建立与下一个条件的或的关系)
默认的情况下，二个条件之间是AND的关系，用这个标志将关系改为OR。例如：
<div class="code">RewriteCond %{REMOTE_HOST} ^host1.* [OR]
RewriteCond %{REMOTE_HOST} ^host2.* [OR]
RewriteCond %{REMOTE_HOST} ^host3.*
RewriteRule ...</div>
如果没有[OR]标志，需要写三个条件/规则.</li>
</ol>
例子：根据客户端浏览器的不同，返回不同的首页面。
<div class="code">RewriteCond %{HTTP_USER_AGENT} ^Mozilla.*
RewriteRule ^/$ /homepage.max.html [L]
RewriteCond %{HTTP_USER_AGENT} ^Lynx.*
RewriteRule ^/$ /homepage.min.html [L]
RewriteRule ^/$ /homepage.std.html [L]</div>
<h3 id="startcontent" class="title">Apache的Mod_rewrite学习（四）</h3>
今天学习重写影射等内容。

<a name="more"></a>

<strong>RewriteMap</strong>
Syntax: RewriteMap MapName MapType:MapSource
RewriteMap指令定义一个重写影射(Rewriting Map),在规则的substitution串中，通过影射函数(mapping-functions)来查找关键字(key)，并用关键字对应的值来进 行来行插入或替换操作。这个查找的对象，可以是各种各样的。
MapName是影射的名字，将用来通过下列的某种结构来为substitution定义影射函数:
${ MapName : LookupKey }
${ MapName : LookupKey | DefaultValue }
当 这些结构之一出现substitution串中时，重写引擎会到mapname影射中查找lookupkey关键字，如果找到了就用返回的值 (substvalue)来替换该结构，如果找不到就用defaultvalue来替换该结构，如果没有defaultvalue，就用空串来替换。
MapType 和mapSource组合有以下几种：
<ol>
	<li>标准的普通文本(Standard Plain Text)
MapType: txt, MapSource: Unix文件系统中合法的带有路径的regular file名
此种情况下，MapSource文件是一个普通的ASCII文本文件,可以含有空行、注释行（以＃打头），及以下结构的键值对行（每个键值对一行）。
MatchingKey SubstValue
例如：Mapsource文件叫/path/to/file/map.txt,其内容为
<div class="code">##
## map.txt -- rewriting map
##
Ralf.S.Engelschall rse # Bastard Operator From Hell
Mr.Joe.Average joe # Mr. Average</div>
在配置文件中则可以这样定义重写映射：
<div class="code">RewriteMap real-to-user txt:/path/to/file/map.txt</div></li>
	<li>随机的普通文本(Randomized Plain Text)
MapType: rnd, MapSource: Unix文件系统中合法的带有路径的regular file名　 　这种情况与标准普通文本情况很相似，差别只是在SubstValue的格式上：此时的SubstValue由一些用”|”分隔的值组成的串，这个“｜” 是“或者”的意思。当根据键值找到对应的SubstValue后，mod_rewrite借助“｜”将此串分解为一些候选项，然后随机选择一项作为最终的 SubstValue的值返回。这听起来有点疯狂或毫无用处，其实这是设计用来在反向代理(reverse proxy)的情况下做负载均衡，用来查找服务器的名字。
例如：MapSource文件的名字为/path/to/file/map.txt，内容如下：
<div class="code">##
## map.txt -- rewriting map
##
static www1|www2|www3|www4
dynamic www5|www6</div>
在配置文件中定义的重写影射为：
<div class="code">RewriteMap servers rnd:/path/to/file/map.txt</div></li>
	<li>Hash File
MapType: dbm, MapSource: Unix文件系统中合法的带有路径的regular file名　　这儿的Mapsource文件是一个二进制的NDBM格式的文件，含有与普通文本格式文件时相同的内容，为了实现快速查找进行了优化处理后以一种特殊的格式来表达。 可以用任何NDBM工具或者用下面的Perl脚本txt2dbm.pl来创建这种格式的文件：
<div class="code">#!/path/to/bin/perl
##
## txt2dbm -- convert txt map to dbm format
##use NDBM_File;
use Fcntl;($txtmap, $dbmmap) = @ARGV;

open(TXT, "&lt;$txtmap") or die "Couldn't open $txtmap!\n";
tie (%DB, 'NDBM_File', $dbmmap,O_RDWR|O_TRUNC|O_CREAT, 0644) or die "Couldn't create $dbmmap!\n";

while () {
next if (/^\s*#/ or /^\s*$/);
$DB{$1} = $2 if (/^\s*(\S+)\s+(\S+)/);
}

untie %DB;
close(TXT);

</div>
此脚本的使用方法如下：
<div class="code">$ perl txt2dbm.pl map.txt map.db</div></li>
	<li>Apache内部函数(Internal Function)
MapType: int, MapSource: Apache内部函数　　这时，MapSource是一个Apache内置函数，目前还不能创建自己的内部函数，只能使用Apache已经定义好的：
<ul>
	<li>toupper
将键值转换为大写</li>
	<li>tolower
将键值转换为小写</li>
	<li>escape
将键值中的特殊字符转换为以16进制表示。</li>
	<li>unescape
将键值中16进制表示的特殊字符转换为原来的样子。</li>
</ul>
</li>
	<li>外部重写程序（External Rewriting Program）
MapType: prg, MapSource: Unix文件系统中合法的带有路径的regular file名　　这儿的MapSource是一个程序而不是一个影射文件。你可以使用任何语言创建这个程序，但这个程序必须是可执行的（也就是说，要么是二进制码，要么是首行带有”#!/path/to/interpreter”格式的解释器的可执行脚本）。这个程序在Apache启动时被运行，然后它就用它的标准输入与标准输出的文件句柄与重写引擎通信（暗指程序能无限时等待标准输入，见下列）。对每个影 射函数的查找要求，它把其标准输入中得到的字符串(newline-terminated string?)作为键值，然后通过标准输出返回查找到的值，如果找不到相应的值，则返回以四字符的字符串”NULL“。下面一个简单的例子，实现将键值 作为查找结果返回：
<div class="code">#!/usr/bin/perl
$| = 1;
while () {
# ...put here any transformations or lookups...
print $_;
}</div>
<ins>注意:</ins>
<ol>
	<li>``Keep it simple, stupid'' (KISS),因为当规则执行时，一量这个外部程序挂起，整个Apache服务器就挂起了</li>
	<li>避免下述常见错误：缓存了标准输出的I/O。这会导致死循环。这也是为什么上例中有``$|=1''这么一行的原因</li>
	<li>用RewriteLock指令定义一个lockfile，以同步与外部程序的通信。默认情况下并没有这样的同步</li>
</ol>
</li>
</ol>
RewriteMap可以出现不只一次。每个影射函数用RewriteMap来声明它的影射文件. While you cannot declare a map in per-directory context it is of course possible to use this map in per-directory context.（？）对普通文本和DBM格式的文件，其键值被缓存在Apache内核中，直到影射文件的mtime 变化了或Apache重启动了。此时，可以将规则中的影射函数用于每个请求。
<strong>RewriteBase</strong>
Syntax: RewriteBase URL-path
RewriteBase明确地指出目录范围内的重写结果的baseURL.RewriteRule指令可以用在目录范围内的配置文件里 (.htaccess)。在目录范围的重写实施时，由本地路径信息构成的前缀已经被去掉，重写规则只对剩余的部分进行处理。处理结束后，被去掉的路径信息 要自动加上，因为当对一个URL进行替换后，重新引擎需要将它重新插入到服务器的处理流程中去。如果服务器端的URL与文件的物理路径有直接有关系，则每 当在.htacess文中定义重写规则时都需要用RewriteBase指URL-path。
<strong>杂项</strong>Environment Variables
mod_rewrite还维护着两个非标准的CGI/SSI环境变量，名为SCRIPT_URL和 SCRIPT_URI，存放着当前资源的逻辑web视图(logical Web-view)。标准的CGI/SSI变量SCRIPT_NAME 和 SCRIPT_FILENAME中存放着当前资源的物理系统视图(physical System-view)。请看例子：
<div class="code">SCRIPT_NAME=/sw/lib/w3s/tree/global/u/rse/.www/index.html
SCRIPT_FILENAME=/u/rse/.www/index.html
SCRIPT_URL=/u/rse/
SCRIPT_URI=http://en1.engelschall.com/u/rse/</div>
<h3 id="startcontent" class="title">Apache的Mod_rewrite学习（五）</h3>
今天主要列出一些例子。由于有些例子是针对特殊路径或特别情况的，列出供大家在思路上参考。因为它们就是些例子。

<a name="more"></a>
<table border="1">
<tbody>
<tr>
<td>目标</td>
<td>重写设置</td>
<td>说明</td>
</tr>
<tr>
<td>规范化URL</td>
<td>RewriteRule ^/~([^/]+)/?(.*) /u/$1/$2 [R]</td>
<td>将/~user重写为/u/user的形式</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/([uge])/([^/]+)$ /$1/$2/ [R]</td>
<td>将/u/user末尾漏掉的/补上</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>规范化HostName</td>
<td>RewriteCond %{HTTP_HOST} !^fully\.qualified\.domain\.name [NC]</td>
<td>域名不合格</td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_HOST} !^$</td>
<td>不空</td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{SERVER_PORT} !^80$</td>
<td>不是80端口</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/(.*) http://fully.qualified.domain.name:%{SERVER_PORT}/$1 [L,R]</td>
<td>重写</td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_HOST} !^fully\.qualified\.domain\.name [NC]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_HOST} !^$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/(.*) http://fully.qualified.domain.name/$1 [L,R]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>URL根目录转移</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/$ /e/www/ [R]</td>
<td>从/移到/e/www/</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>末尾目录补斜线</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td>（目录范围内）</td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo$ foo/ [R]</td>
<td>/~quux/foo是一个目录，补/</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REQUEST_FILENAME} -d</td>
<td>如果请文件名是个目录</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+[^/])$ $1/ [R]</td>
<td>URL末尾不是斜线时补上</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Web集群</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteMap user-to-host txt:/path/to/map.user-to-host</td>
<td>用户－服务器映射</td>
</tr>
<tr>
<td></td>
<td>RewriteMap group-to-host txt:/path/to/map.group-to-host</td>
<td>组－服务器映射</td>
</tr>
<tr>
<td></td>
<td>RewriteMap entity-to-host txt:/path/to/map.entity-to-host</td>
<td>实体－服务器映射</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/u/([^/]+)/?(.*) http://${user-to-host:$1|server0}/u/$1/$2</td>
<td>用户均衡</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/g/([^/]+)/?(.*) http://${group-to-host:$1|server0}/g/$1/$2</td>
<td>组均衡</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/e/([^/]+)/?(.*) http://${entity-to-host:$1|server0}/e/$1/$2</td>
<td>实体均衡</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/([uge])/([^/]+)/?$ /$1/$2/.www/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/([uge])/([^/]+)/([^.]+.+) /$1/$2/.www/$3\</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>URL根目录搬迁</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/~(.+) http://newserver/~$1 [R,L]</td>
<td>到其它服务器</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>所用户名首字母分</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/~(([a-z])[a-z0-9]+)(.*) /home/$2/$1/.www$3</td>
<td>内一层括号为$2</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>NCSA imagemap移</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td>植为mod_imap</td>
<td>RewriteRule ^/cgi-bin/imagemap(.*) $1 [PT]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>多目录查找资源</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td># first try to find it in custom/...</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond /your/docroot/dir1/%{REQUEST_FILENAME} -f</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+) /your/docroot/dir1/$1 [L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td># second try to find it in pub/...</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond /your/docroot/dir2/%{REQUEST_FILENAME} -f</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+) /your/docroot/dir2/$1 [L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td># else go on for other Alias or ScriptAlias directives,</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+) - [PT]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>据URL设置环境变量</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.*)/S=([^/]+)/(.*) $1/$3 [E=STATUS:$2]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>虚拟主机</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_HOST} ^www\.[^.]+\.host\.com$</td>
<td>基于用户名</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+) %{HTTP_HOST}$1 [C]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^www\.([^.]+)\.host\.com(.*) /home/$1$2</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>内外人有别</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REMOTE_HOST} !^.+\.ourdomain\.com$</td>
<td>基于远程主机</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(/~.+) http://www.somewhere.com/$1 [R,L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>错误重定向</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond /your/docroot/%{REQUEST_FILENAME} !-f</td>
<td>不是regular文件</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.+) http://webserverB.dom/$1</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>程序处理特殊协议</td>
<td>RewriteRule ^xredirect:(.+) /path/to/nph-xredirect.cgi/$1 \</td>
<td>Xredirect协议</td>
</tr>
<tr>
<td></td>
<td>[T=application/x-httpd-cgi,L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>最近镜像下载</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteMap multiplex txt:/path/to/map.cxan</td>
<td>顶级域名与最近ftp服务器映射</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/CxAN/(.*) %{REMOTE_HOST}::$1 [C]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^.+\.([a-zA-Z]+)::(.*)$ ${multiplex:$1|ftp.default.dom}$2 [R,L]</td>
<td>据顶级域名不同提供不同的FTP服务器</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>基于时间重写</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{TIME_HOUR}%{TIME_MIN} &gt;0700</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{TIME_HOUR}%{TIME_MIN} &lt;1900</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.day.html</td>
<td>白天为早晚7点间</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.night.html</td>
<td>其余为夜间</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>向前兼容扩展名</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td># parse out basename, but remember the fact</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.*)\.html$ $1 [C,E=WasHTML:yes]</td>
<td></td>
</tr>
<tr>
<td></td>
<td># rewrite to document.phtml if exists</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REQUEST_FILENAME}.phtml -f</td>
<td>如果存在$1.phtml则重写</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.*)$ $1.phtml [S=1]</td>
<td></td>
</tr>
<tr>
<td></td>
<td># else reverse the previous basename cutout</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{ENV:WasHTML} ^yes$</td>
<td>如果不存在$1.phtml，则保持不变</td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^(.*)$ $1.html</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>文件改名（目录级）</td>
<td>RewriteEngine on</td>
<td>内部重写</td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ bar.html</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteEngine on</td>
<td>重定向由客户端再次提交</td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ bar.html [R]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>据浏览器类型重写</td>
<td>RewriteCond %{HTTP_USER_AGENT} ^Mozilla/3.*</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.NS.html [L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_USER_AGENT} ^Lynx/.* [OR]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_USER_AGENT} ^Mozilla/[12].*</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.20.html [L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.32.html [L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>动态镜像远程资源</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^hotsheet/(.*)$ http://www.tstimpreso.com/hotsheet/$1 [P]</td>
<td>利用了代理模块</td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^usa-news\.html$ http://www.quux-corp.com/news/index.html [P]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>反向动态镜像</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond /mirror/of/remotesite/$1 -U</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^http://www\.remotesite\.com/(.*)$ /mirror/of/remotesite/$1</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>负载均衡</td>
<td>RewriteEngine on</td>
<td>利用代理实现round-robin效果</td>
</tr>
<tr>
<td></td>
<td>RewriteMap lb prg:/path/to/lb.pl</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/(.+)$ ${lb:$1} [P,L]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>#!/path/to/perl</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$| = 1;</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$name = "www"; # the hostname base</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$first = 1; # the first server (not 0 here, because 0 is myself)</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$last = 5; # the last server in the round-robin</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$domain = "foo.dom"; # the domainname</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$cnt = 0;</td>
<td></td>
</tr>
<tr>
<td></td>
<td>while (&lt;STDIN&gt;) {</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$cnt = (($cnt+1) % ($last+1-$first));</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$server = sprintf("%s%d.%s", $name, $cnt+$first, $domain);</td>
<td></td>
</tr>
<tr>
<td></td>
<td>print "http://$server/$_";</td>
<td></td>
</tr>
<tr>
<td></td>
<td>}</td>
<td></td>
</tr>
<tr>
<td></td>
<td>##EOF##</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>静态页面变脚本</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteBase /~quux/</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^foo\.html$ foo.cgi [T=application/x-httpd-cgi]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>阻击机器人</td>
<td>RewriteCond %{HTTP_USER_AGENT} ^NameOfBadRobot.*</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REMOTE_ADDR} ^123\.45\.67\.[8-9]$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/~quux/foo/arc/.+ - [F]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>阻止盗连你的图片</td>
<td>RewriteCond %{HTTP_REFERER} !^$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_REFERER} !^http://www.quux-corp.de/~quux/.*$ [NC]</td>
<td>自己的连接可不能被阻止</td>
</tr>
<tr>
<td></td>
<td>RewriteRule .*\.gif$ - [F]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_REFERER} !^$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{HTTP_REFERER} !.*/foo-with-gif\.html$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^inlined-in-foo\.gif$ - [F]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>拒绝某些主机访问</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteMap hosts-deny txt:/path/to/hosts.deny</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond ${hosts-deny:%{REMOTE_HOST}|NOT-FOUND} !=NOT-FOUND [OR]</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond ${hosts-deny:%{REMOTE_ADDR}|NOT-FOUND} !=NOT-FOUND</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/.* - [F]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>用户授权</td>
<td>RewriteCond %{REMOTE_IDENT}@%{REMOTE_HOST} !^friend1@client1.quux-corp\.com$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REMOTE_IDENT}@%{REMOTE_HOST} !^friend2@client2.quux-corp\.com$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteCond %{REMOTE_IDENT}@%{REMOTE_HOST} !^friend3@client3.quux-corp\.com$</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/~quux/only-for-friends/ - [F]</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>外部重写程序模板</td>
<td>RewriteEngine on</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteMap quux-map prg:/path/to/map.quux.pl</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/~quux/(.*)$ /~quux/${quux-map:$1}</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td></td>
<td>#!/path/to/perl</td>
<td></td>
</tr>
<tr>
<td></td>
<td>$| = 1;</td>
<td></td>
</tr>
<tr>
<td></td>
<td>while (&lt;&gt;) {</td>
<td></td>
</tr>
<tr>
<td></td>
<td>s|^foo/|bar/|;</td>
<td></td>
</tr>
<tr>
<td></td>
<td>print $_;</td>
<td></td>
</tr>
<tr>
<td></td>
<td>}</td>
<td></td>
</tr>
<tr>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>搜索引擎友好</td>
<td>RewriteRule ^/products$ /content.php</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/products/([0-9]+)$ /content.php?id=$1</td>
<td></td>
</tr>
<tr>
<td></td>
<td>RewriteRule ^/products/([0-9]+),([ad]*),([0-9]{0,3}),([0-9]*),([0-9]*$) /marso/content.php?id=$1&amp;sort=$2&amp;order=$3&amp;start=$4</td>
<td></td>
</tr>
</tbody>
</table>
参考文献：
URL Rewriting Guide
http://httpd.apache.org/docs/misc/rewriteguide.html

mod_rewrite: A Beginner's Guide to URL Rewriting
http://www.sitepoint.com/article/910
<h3 id="startcontent" class="title">Apache的Mod_rewrite学习（六）</h3>
狗尾续貂一把，把我在实际中用的apache+mod_jk+tomcat+mod_rewrite来重写含有.jsp的URL设置帖出来，其实就是简单地把前面学的用了一下，供参考。

<a name="more"></a>

假定你已经利用mod_jk把apache和tomcat协同起来工作了，并且已经开发了一个web应用叫mywebapp。对httpd.conf改动如下：
<ol>
	<li>两个模块的加载顺序
<div class="code">...
LoadModule rewrite_module modules/mod_rewrite.so
...
LoadModule jk_module modules/mod_jk.so
...
AddModule mod_jk.c
....
AddModule mod_rewrite.c
....</div></li>
	<li>两个模块的设置顺序
<div class="code">...
&lt;IfModule mod_jk.c&gt;
...
JkWorkersFile "/path/to/workers.properties"
JkLogFile "/path/to/mod_jk.log"
JkLogLevel info
...
JkMount /*.jsp ajp13
&lt;/IfModule&gt;
...
&lt;VirtualHost *&gt;
...
ServerName hedong.3322.org
...
&lt;IfModule mod_rewrite.c&gt;
RewriteEngine on
RewriteMap id2jsp txt:/path/to/id2jsp.txt #jsp文件名编码表
RewriteMap id2db txt:/path/to/id2db.txt #数据库名字编码表
RewriteRule ^/mywebapp/([0-9]*)/([0-9]*),([0-9]*)\.html ${id2jsp:$1}\?dbname=${id2db:$2}&amp;r=$3 [PT] #参数r表示记录ID，PT允许mod_jk继续处理请求URL
RewriteRule ^/mywebapp$ /mywebapp/ [R] #被全目录名后被省略的/
RewriteRule ^/mywebapp/$ /mywebapp/index\.jsp [PT] #PT允许mod_jk继续处理请求URL
....
&lt;/IfModule&gt;
JkMount /mywebapp ajp13
JkMount /mywebapp/* ajp13
....
&lt;/VirtualHost&gt;</div></li>
	<li>一点感想
当用mod_rewrite在服务器范围内对请求URL进行重写时，如果涉及到URL中目录层次的变化（如本来是一层目录/a/page.html，现 在被写成/jsps/part1/page.jsp）时，要小心，因为：HTML/jsp页面中很多链接是相对链接，尤其是图片链接尤为常见， 即如../images/1.jpg等。
当你重写了URL的目录层次时，页面中的相对链接不受重写的影响，而致出现显示问题。
接 着上面的例子，假定/jsps/part1/page.jsp中有一个链接为../images/1.jpg，而且/jsps/images/1.jpg 确实存在，当不用mod_rewrite重写时，/jsps/part1/page.jsp显示正常。而重写以后，page.jsp能运行，而1.jpg 的URL变成了/images/1.jpg了，虽然你在重写是改动了目录，此1.jpg的URL还是相对于/a/的。</li>
</ol>
版权声明：可以任意转载，转载时请务必以超链接形式标明文章原始出处和作者信息及本声明
<a href="http://www.chedong.com/tech/google_url.html"><span style="color: #1d58d1;">http://www.chedong.com/tech/google_url.html</span></a>

关键词："url rewrite" mod_rewrite isapi rewrite path_info iis "search engine friendly"

内容摘要：不得不承认，将动态网页链接rewriting成静态链接是最保险和稳定的面向搜索引擎优化方式

此外随着互联网上的内容以惊人速度的增长也越来越突出了搜索引擎的重要性，如果网站想更好地被搜索引擎收录，网站设计除了面向用户友好（User Friendly）外，<a href="http://www.chedong.com/tech/google.html"><span style="color: #1d58d1;">搜索引擎友好（Search Engine Friendly）的设计也是非常重要的</span></a>。进入搜索引擎的页面内容越多，则被用户用不同的关键词找到的几率越大。<a href="http://pr.efactory.de/e-number-of-pages.shtml"><span style="color: #1d58d1;">在Google的算法调查</span></a>一 文中提到一个站点被Google索引页面的数量其实对PageRank也是有一定影响的。由于Google 突出的是整个网络中相对静态的部分（动态网页索引量比较小）,链接地址相对固定的静态网页比较适合被Google索引（怪不得很多大网站的邮件列表归档和 BLOG按日期归档的文档很容被搜的到），因此很多关于面向搜索引擎 URL设计优化(URI Pretty)的文章中提到了很多利用一定机制将动态网页参数变成像静态网页的形式：
比如可以将：<a href="http://phpunixman.sourceforge.net/index.php?mode=man&amp;parameter=ls">
<span style="color: #1d58d1;">http://phpunixman.sourceforge.net/index.php?mode=man&amp;parameter=ls</span></a>
变成：
<a href="http://phpunixman.sourceforge.net/index.php/man/ls"><span style="color: #1d58d1;">http://phpunixman.sourceforge.net/index.php/man/ls</span></a>

实现方式主要有2种：
<ul>
	<li><a href="http://blog.ccw.com.cn/vvstone/%23rewrite"><span style="color: #1d58d1;">基于url rewrite</span></a>
<a href="http://www.helicontech.com/download/"><span style="color: #1d58d1;">IIS的ISAPI REWRITE下载（免费）</span></a></li>
	<li><a href="http://blog.ccw.com.cn/vvstone/%23path_info"><span style="color: #1d58d1;">基于path_info</span></a></li>
</ul>
<h2><a name="rewrite"></a>把URI地址用作参数传递：URL REWRITE</h2>
最简单的是基于各种WEB服务器中的URL重写转向（Rewrite）模块的URL转换：
这样几乎可以不修改程序的实现将 news.asp?id=234 这样的链接映射成 news/234.html，从外面看上去和静态链接一样。Apache服务器上有一个模块（非缺省）：mod_rewrite：URL REWRITE功能之强大足够写上一本书。

当我需要将将news.asp?id=234的映射成news/234.html时，只需设置：
RewriteRule /news/(\d+)\.html /news\.asp\?id=$1 [N,I]
这样就把 /news/234.html 这样的请求映射成了 /news.asp?id=234
当有对/news/234.html的请求时：web服务器会把实际请求转发给/news.asp?id=234

而在IIS也有相应的REWRITE模块：比如<a href="http://www.helicontech.com/"><span style="color: #1d58d1;">ISAPI REWRITE</span></a>和<a href="http://www.qwerksoft.com/products/iisrewrite/"><span style="color: #1d58d1;">IIS REWRITE</span></a>，语法都是基于正则表达式，因此配置几乎和apache的mod_rewrite是相同的：

比对于某一个简单应用可以是：
RewriteRule /news/(\d+)\.html /news/news\.php\?id=$1 [N,I]
这样就把 http://www.chedong.com/news/234.html 映射到了 http://www.chedong.com/news/news.php?id=234

一个更通用的能够将所有的动态页面进行参数映射的表达式是：
把 http://www.myhost.com/foo.php?a=A&amp;b=B&amp;c=C
表现成 http://www.myhost.com/foo.php/a/A/b/B/c/C。
RewriteRule (.*?\.php)(\?[^/]*)?/([^/]*)/([^/]*)(.+?)?$1(?2$2&amp;:\?)$3=$4?5$5: [N,I]

以下是针对phpBB的一个Apache mod_rewrite配置样例：
<pre>    RewriteEngine On
    RewriteRule /forum/topic_(.+)\.html$  /forum/viewtopic.php?t=$1 [L]
    RewriteRule /forum/forum_(.+)\.html$ /forum/viewforum.php?f=$1 [L]
    RewriteRule /forum/user_(.+)\.html$  /forum/profile.php?mode=viewprofile&amp;u=$1  [L]</pre>
这样设置后就可以通过topic_1234.html forum_2.html user_34.html这样的链接访问原来的动态页面了。

通过URL REWRITE还有一些好处：
mod_rewrite和isapirewrite基本兼容，但是还是有些不同，比如：isapirewrite中"?"需要转义成"\?"，mod_rewrite不用，isapirewrite支持 "\d+" （全部数字），mod_rewrite不支持
<ul>
	<li>隐藏后台实现：这在后台应用平台的迁移时非常有用：当从asp迁移到java平台时，对于前台用户来说，根本感受不到后台应用的变化；</li>
	<li>简化数据校验：因为像(\d+)这样的参数，可以有效的控制数字的格式甚至位数；</li>
</ul>
比如我们需要将应用从news.asp?id=234迁移成news.php?query=234时，前台的表现可以一直保持为 news/234.html。从实现应用和前台表现的分离：保持了URL的稳定性，而使用mod_rewrite甚至可以把请求转发到其他后台服务器上。
<h2><a name="path_info"></a>基于PATH_INFO的URL美化</h2>
Url美化的另外一个方式就是基于PATH_INFO：
PATH_INFO是一个CGI 1.1的标准，经常发现很多跟在CGI后面的"/value_1/value_2"就是PATH_INFO参数：
比如：<a href="http://phpunixman.sourceforge.net/index.php/man/ls"><span style="color: #1d58d1;">http://phpunixman.sourceforge.net/index.php/man/ls</span></a> 中：$PATH_INFO = "/man/ls"

PATH_INFO是CGI标准，因此PHP Servlet等都有的支持。比如Servlet中就有request.getPathInfo()方法。
注 意：/myapp/servlet/Hello/foo的 getPathInfo()返回的是/foo，而/myapp/dir/hello.jsp/foo的getPathInfo()将返回的 /hello.jsp，从这里你也可以知道jsp其实就是一个Servlet的PATH_INFO参数。ASP不支持PATH_INFO
PHP中基于PATH_INFO的参数解析的例子如下：
//注意：参数按"/"分割，第一个参数是空的：从/param1/param2中解析出$param1 $param2这2个参数
if ( isset($_SERVER["PATH_INFO"]) ) {
list($nothing, $param1, $param2) = explode('/', $_SERVER["PATH_INFO"]);
}

如何隐蔽应用：例如 .php，的扩展名：
在APACHE中这样配置：
&lt;FilesMatch "^app_name$"&gt;
ForceType application/x-httpd-php
&lt;/FilesMatch&gt;

如何更像静态页面：app_name/my/app.html
解析的PATH_INFO参数的时候，把最后一个参数的最后5个字符“.html”截断即可。
注意：APACHE2中缺省是不允许PATH_INFO的，需要设置 AcceptPathInfo on

特别是针对使用虚拟主机用户，无权安装和配置mod_rewrite的时候，PATH_INFO往往就成了唯一的选择。

OK， 这样以后看见类似于http://www.example.com/article/234这样的网页你就知道可能是 article/show.php?id=234这个php程序生成的动态网页，很多站点表面看上去可能有很多静态目录，其实很有可能都是使用1，2个程 序实现的内容发布。比如很多WIKIWIKI系统都使用了这个机制：整个系统就一个简单的wiki程序，而看上去的目录其实都是这个应用拿后面的地址作为 参数的查询结果。

利用基于MOD_REWRITE/PATH_INFO ＋ CACHE服务器的解决方案对原有的动态发布系统进行改造，也可以大大降低旧有系统升级到新的内容管理系统的成本。并且方便了搜索引擎收录入索引。