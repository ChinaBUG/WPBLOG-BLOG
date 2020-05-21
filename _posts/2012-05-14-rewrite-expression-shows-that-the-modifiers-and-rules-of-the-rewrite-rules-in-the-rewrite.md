---
ID: 2050
post_title: >
  rewrite-重写规则之修正符及规则表达式说明
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/rewrite-expression-shows-that-the-modifiers-and-rules-of-the-rewrite-rules-in-the-rewrite.html
published: true
post_date: 2012-05-14 14:15:04
---
<h1 class="post-title">.htaccess rewrite 规则详细说明</h1>
<div class="bdsharebuttonbox bdshare-button-style2-16" data-bd-bind="1434012589793"></div>
<p class="post-byline">作者 <a title="由freemouse发布" href="http://www.cnphp.info/author/admin" rel="author">freemouse</a> · 2010年07月22日</p>

<div class="clear"></div>
<div class="entry share">
<div class="entry-inner">

用Apache虚拟主机的朋友很多，apache提供的<strong>.htaccess</strong>模块可以为每个虚拟主机设定<strong>rewrite规则</strong>， 这对网站SEO优化相当有用，同时也改善了用户体验。国内的虚拟机一般不提供.htaccess功能（据我所知，discuz的主机好像提供此功能），而 在国外主机中，.htaccess功能似乎是标配，笔者的Blog架在MT上，支持.htaccess，每次看到一堆别人写好了的.htaccess设 置，很多命令都不甚了了，查看、修改起来很不方便，痛定思痛，潜心学习一下，知其所以然嘛～

学习前提：（不会的朋友要学习一下，才能更好的理解下面的文字呢）
<ul>
	<li><strong>Linux基础</strong>（不会也没事啦，写个.htaccess没必要大费周折啦，推荐：鸟哥私房菜linux基础）</li>
	<li><strong>正则表达式</strong>（Rewrite规则建立在正则的基础之上，推荐：正则表达式30分钟入门教程）</li>
</ul>
<strong>rewrite的语法格式</strong>：
<ol>
	<li>RewriteEngine On #要想rewrite起作用，必须要写上哦</li>
	<li>RewriteBase url-path #设定基准目录，例如希望对根目录下的文件rewrtie，就是”/”</li>
	<li>RewriteCond test-string condPattern #写在RewriteRule之前，可以有一或N条，用于测试rewrite的匹配条件，具体怎么写，后面会详细说到。</li>
	<li>RewriteRule Pattern Substitution #规则</li>
</ol>
&nbsp;
<h2>RewriteEngine On|Off</h2>
RewriteEngine 用于开启或停用rewrite功能。
rewrite configurations 不会自动继承，因此你得给每个你想用 rewrite功能的虚拟主机目录中加上这个指令。
<h2>RewriteBase URL-path</h2>
RewriteBase用于设定重写的基准URL。在下文中，你可以看见RewriteRule可以用于目录级的配置文件中 (.htaccess)并在局部范围内起作用，即规则实际处理的只是剥离了本地路径前缀的一部分。处理结束后，这个路径会被自动地附着回去。默认值 是”RewriteBase physical-directory-path”。
在对一个新的URL进行替换时，此模块必须把这个URL重新注入到服务器处理中。为此，它必须知道其对应的URL前缀或者说URL基准。通常，此前缀就是 对应的文件路径。但是，大多数网站URL不是直接对应于其物理文件路径的，因而一般不能做这样的假定! 所以在这种情况下，就必须用RewriteBase指令来指定正确的URL前缀。
如果你的网站服务器URL不是与物理文件路径直接对应的，而又需要使用RewriteBase指令，则必须在每个对应的.htaccess文件中指定 RewriteRule 。
<h2>RewriteCond TestString CondPattern [flags]</h2>
RewriteCond指令定义了一个规则的条件，即，在一个RewriteRule指令之前有一个或多个RewriteCond指令。 条件之后的重写规则仅在当前URI与pattern匹配并且符合这些条件的时候才会起作用。
TestString是一个纯文本的字符串，但是还可以包含下列可扩展的成分：
<ol>
	<li>RewriteRule反向引用: 引用方法是 <strong>$N</strong>  (0 &lt;= N &lt;= 9) 引用当前(带有若干RewriteCond指令的)RewriteRule中的 与pattern匹配的分组成分(圆括号!)。</li>
	<li>RewriteCond反向引用: 引用方法是 <strong>%N</strong>  (1 &lt;= N &lt;= 9) 引用当前若干RewriteCond条件中最后符合的条件中的分组成分(圆括号!)。</li>
	<li>RewriteMap 扩展: 引用方法是 <strong>${mapname:key|default}</strong></li>
	<li>服务器变量: 引用方法是 <strong>%{ NAME_OF_VARIABLE }</strong>  这个是我们最常使用到的功能</li>
</ol>
<em>NAME_OF_VARIABLE</em>具体数值见下表:
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<th>HTTP headers:</th>
<th>connection &amp; request:</th>
<th></th>
</tr>
<tr class="alt">
<td>HTTP_USER_AGENT
HTTP_REFERER
HTTP_COOKIE
HTTP_FORWARDED
HTTP_HOST
HTTP_PROXY_CONNECTION
HTTP_ACCEPT</td>
<td>REMOTE_ADDR
REMOTE_HOST
REMOTE_USER
REMOTE_IDENT
REQUEST_METHOD
SCRIPT_FILENAME
PATH_INFO
QUERY_STRING
AUTH_TYPE</td>
<td></td>
</tr>
<tr>
<th>server internals:</th>
<th>system stuff:</th>
<th>specials:</th>
</tr>
<tr class="alt">
<td>DOCUMENT_ROOT
SERVER_ADMIN
SERVER_NAME
SERVER_ADDR
SERVER_PORT
SERVER_PROTOCOL
SERVER_SOFTWARE</td>
<td>TIME_YEAR
TIME_MON
TIME_DAY
TIME_HOUR
TIME_MIN
TIME_SEC
TIME_WDAY
TIME</td>
<td>API_VERSION
THE_REQUEST
REQUEST_URI
REQUEST_FILENAME
IS_SUBREQ</td>
</tr>
</tbody>
</table>
<blockquote>这些都对应于类似命名的HTTP MIME头、Apache服务器的C变量以及Unix系统中的 struct tm字段，大多数都在其他的手册或者CGI规范中有所讲述。 而其中为mod_rewrite所特有的变量有:
IS_SUBREQ
如果正在处理的请求是一个子请求，它包含字符串”true”，否则就是”false”。 模块为了解析URI中的附加文件，有可能会产生子请求。
API_VERSION
这是正在使用的httpd中(服务器和模块之间内部接口)的Apache模块API的版本， 其定义位于include/ap_mmn.h中。此模块版本对应于正在使用的Apache的版本 (比如，在Apache 1.3.14的发行版中，这个值是19990320:10)。 通常，对它感兴趣的是模块的作者。
THE_REQUEST
这是由浏览器发送给服务器的完整的HTTP请求行。(比如, “GET /index.html HTTP/1.1″). 它不包含任何浏览器发送的附加头信息。
REQUEST_URI
这是在HTTP请求行中所请求的资源。(比如上述例子中的”/index.html”.)
REQUEST_FILENAME
这是与请求相匹配的完整的本地文件系统的文件路径名或描述.</blockquote>
CondPattern是条件pattern, 即, 一个应用于当前实例TestString的正则表达式, 即, TestString将会被计算然后与CondPattern匹配.

<strong>注意：</strong><em>CondPattern</em>是一个<em>兼容perl的正则表达式</em>， 但是还有若干补充：
<ol>
	<li>可以在pattern串中使用’<code>!</code>‘ 字符(惊叹号)来实现匹配的<strong>反转</strong>。</li>
</ol>
<h2>RewriteOptions Options</h2>
<code>RewriteOptions</code>指令为当前服务器级和目录级的配置设置一些选项。 <em>Option</em>可以是下列值之一：

<dl><dt><code>inherit</code></dt><dd>此值强制当前配置可以继承其父配置。 在虚拟主机级配置中，它意味着主服务器的映射表、条件和规则可以被继承。 在目录级配置中，它意味着其父目录的<code>.htaccess</code>中的条件和规则可以被继承。</dd><dt><code>MaxRedirects=<var>number</var></code></dt><dd>为了避免目录级<code>RewriteRule</code>的无休止的内部重定向， 在此类重定向和500内部服务器错误次数达到一个最大值的时候， <code>mod_rewrite</code>会停止对此请求的处理。 如果你确实需要对每个请求允许大于10次的内部重定向，可以增大这个值。</dd></dl>
<h2>RewriteRule Pattern Substitution [flags]</h2>
<code>RewriteRule</code>指令是重写引擎的根本。此指令可以多次使用。 每个指令定义一个简单的重写规则。这些规则的<strong>定义顺序</strong>尤为<strong>重要</strong>, 因为，在运行时刻，规则是按这个顺序逐一生效的.

<a id="patterns" name="patterns"></a><em>Pattern</em>是一个作用于当前URL的兼容perl的<a id="regexp" name="regexp"></a>正则表达式。

此外，还可以使用否字符(‘<code>!</code>‘)的pattern前缀，以实现pattern的反转。但是，需要注意的是使用否字符以反转pattern时，pattern中不能使用分组的通配成分；即$N。

<a id="rhs" name="rhs"></a>重写规则中的<em>Substitution</em>是， 当原始URL与<em>Pattern</em>相匹配时，用以替代(或替换)的字符串。除了纯文本，还可以使用

&nbsp;
<ul>
	<li><code>$N</code> 反向引用RewriteRule的pattern</li>
	<li><code>%N</code> 反向引用最后匹配的RewriteCond pattern</li>
	<li>规则条件测试字符串中(<code>%{VARNAME}</code>)的服务器变量</li>
	<li>映射函数调用(<code>${mapname:key|default})</code></li>
</ul>
&nbsp;

&nbsp;
<pre><strong>下面给出几个完整的例子供各位参考：</strong></pre>
<blockquote><strong>一、防盗链功能</strong>
只这四行就实现了防盗链是不是很神奇^_^，编写起来是不是又觉得复杂。
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^http://(.+.)?mysite.com/ [NC]
RewriteCond %{HTTP_REFERER} !^$
RewriteRule .*.(jpe?g|gif|bmp|png)$ /images/nohotlink.jpg [L]</blockquote>
<blockquote><strong>二、网址规范化</strong>
这个是把所有二级域名都重定向到www.yourdomain.com的例子，现在看来是不是很简单了？
Options +FollowSymLinks
rewriteEngine on
rewriteCond %{http_host} ^yourdomain.com [NC]
rewriteRule ^(.*)$ http://www.yourdomain.com/$1 [R=301,L]</blockquote>
<blockquote><strong>三、临时错误页面</strong>
当你的网站在升级、修改的时候，你最好让访客转到指定的页面，而不是没做完的页面或者是错误页。
RewriteEngine on
RewriteCond %{REQUEST_URI} !/maintenance.html$
RewriteCond %{REMOTE_ADDR} !^123.123.123.123
RewriteRule $ /error.html [R=302,L]</blockquote>
<blockquote><strong>四、重定向RSS地址到FeedSky</strong>
除了可以更改模板里的RSS地址外，.htaccess也能实现RSS地址的更改，并更加方便。
RewriteEngine on
RewriteCond %{HTTP_USER_AGENT} !FeedSky [NC]
RewriteCond %{HTTP_USER_AGENT} !FeedValidator [NC]
RewriteRule ^feed/?([_0-9a-z-]+)?/?$ http://feed.feedsky.com/yours</blockquote>
=========================================================================================
<strong>附录：flags</strong>
<ol>
	<li>
<h4>‘redirect|R [=code]‘ (强制重定向 redirect)</h4>
以http://thishost[:thisport]/(使新的URL成为一个URI) 为前缀的Substitution可以强制性执行一个外部重定向。 如果code没有指定，则产生一个HTTP响应代码302(临时性移动)。 如果需要使用在300-400范围内的其他响应代码，只需在此指定这个数值即可， 另外，还可以使用下列符号名称之一: temp (默认的), permanent, seeother. 用它可以把规范化的URL反馈给客户端，如, 重写“/~”为 “/u/”，或对/u/user加上斜杠，等等。 注意: 在使用这个标记时，必须确保该替换字段是一个有效的URL! 否则，它会指向一个无效的位置! 并且要记住，此标记本身只是对URL加上 http://thishost[:thisport]/的前缀，重写操作仍然会继续。 通常，你会希望停止重写操作而立即重定向，则还需要使用’L’标记.</li>
	<li>
<h4>‘forbidden|F’ (强制URL为被禁止的 forbidden)</h4>
强制当前URL为被禁止的，即，立即反馈一个HTTP响应代码403(被禁止的)。 使用这个标记，可以链接若干RewriteConds以有条件地阻塞某些URL。</li>
	<li>
<h4>‘gone|G’ (强制URL为已废弃的 gone)</h4>
强制当前URL为已废弃的，即，立即反馈一个HTTP响应代码410(已废弃的)。 使用这个标记，可以标明页面已经被废弃而不存在了.</li>
	<li>
<h4>‘proxy|P’ (强制为代理 proxy)</h4>
此标记使替换成分被内部地强制为代理请求，并立即(即， 重写规则处理立即中断)把处理移交给代理模块。 你必须确保此替换串是一个有效的(比如常见的以 http://hostname开头的)能够为Apache代理模块所处理的URI。 使用这个标记，可以把某些远程成分映射到本地服务器名称空间， 从而增强了ProxyPass指令的功能。 注意: 要使用这个功能，代理模块必须编译在Apache服务器中。 如果你不能确定，可以检查“httpd -l”的输出中是否有mod_proxy.c。 如果有，则mod_rewrite可以使用这个功能； 如果没有，则必须启用mod_proxy并重新编译“httpd”程序。</li>
	<li>
<h4>‘last|L’ (最后一个规则 last)</h4>
立即停止重写操作，并不再应用其他重写规则。 它对应于Perl中的last命令或C语言中的break命令。 这个标记可以阻止当前已被重写的URL为其后继的规则所重写。 举例，使用它可以重写根路径的URL(‘/’)为实际存在的URL, 比如, ‘/e/www/’.</li>
	<li>
<h4>‘next|N’ (重新执行 next round)</h4>
重新执行重写操作(从第一个规则重新开始). 这时再次进行处理的URL已经不是原始的URL了，而是经最后一个重写规则处理的URL。 它对应于Perl中的next命令或C语言中的continue命令。 此标记可以重新开始重写操作，即, 立即回到循环的头部。 但是要小心，不要制造死循环!</li>
	<li>
<h4>‘chain|C’ (与下一个规则相链接 chained)</h4>
此标记使当前规则与下一个(其本身又可以与其后继规则相链接的， 并可以如此反复的)规则相链接。 它产生这样一个效果: 如果一个规则被匹配，通常会继续处理其后继规则， 即，这个标记不起作用；如果规则不能被匹配， 则其后继的链接的规则会被忽略。比如，在执行一个外部重定向时， 对一个目录级规则集，你可能需要删除“.www” (此处不应该出现“.www”的)。</li>
	<li>
<h4>‘type|T=MIME-type’ (强制MIME类型 type)</h4>
强制目标文件的MIME类型为MIME-type。 比如，它可以用于模拟mod_alias中的ScriptAlias指令， 以内部地强制被映射目录中的所有文件的MIME类型为“application/x-httpd-cgi”.</li>
	<li>
<h4>‘nosubreq|NS’ (仅用于不对内部子请求进行处理 no internal sub-request)</h4>
在当前请求是一个内部子请求时，此标记强制重写引擎跳过该重写规则。 比如，在mod_include试图搜索可能的目录默认文件(index.xxx)时， Apache会内部地产生子请求。对子请求，它不一定有用的，而且如果整个规则集都起作用， 它甚至可能会引发错误。所以，可以用这个标记来排除某些规则。 根据你的需要遵循以下原则: 如果你使用了有CGI脚本的URL前缀，以强制它们由CGI脚本处理， 而对子请求处理的出错率(或者开销)很高，在这种情况下，可以使用这个标记。</li>
	<li>
<h4>‘nocase|NC’ (忽略大小写 no case)</h4>
它使Pattern忽略大小写，即, 在Pattern与当前URL匹配时，’A-Z’ 和’a-z’没有区别。</li>
	<li>
<h4>‘qsappend|QSA’ (追加请求串 query string append)</h4>
此标记强制重写引擎在已有的替换串中追加一个请求串，而不是简单的替换。 如果需要通过重写规则在请求串中增加信息，就可以使用这个标记。</li>
	<li>
<h4>‘noescape|NE’ (在输出中不对URI作转义 no URI escaping)</h4>
此标记阻止mod_rewrite对重写结果应用常规的URI转义规则。 一般情况下，特殊字符(如’%’, ‘$’, ‘;’等)会被转义为等值的十六进制编码。 此标记可以阻止这样的转义，以允许百分号等符号出现在输出中，如： RewriteRule /foo/(.*) /bar?arg=P1\%3d$1 [R,NE]
可以使’/foo/zed’转向到一个安全的请求’/bar?arg=P1=zed’.</li>
	<li>
<h4>‘passthrough|PT’ (移交给下一个处理器 pass through)</h4>
此标记强制重写引擎将内部结构request_rec中的uri字段设置为 filename字段的值，它只是一个小修改，使之能对来自其他URI到文件名翻译器的 Alias，ScriptAlias, Redirect 等指令的输出进行后续处理。举一个能说明其含义的例子： 如果要通过mod_rewrite的重写引擎重写/abc为/def， 然后通过mod_alias使/def转变为/ghi，可以这样: RewriteRule ^/abc(.*) /def$1 [PT]
Alias /def /ghi
如果省略了PT标记，虽然mod_rewrite运作正常， 即, 作为一个使用API的URI到文件名翻译器， 它可以重写uri=/abc/…为filename=/def/…， 但是，后续的mod_alias在试图作URI到文件名的翻译时，则会失效。
注意: 如果需要混合使用不同的包含URI到文件名翻译器的模块时， 就必须使用这个标记。混合使用mod_alias和mod_rewrite就是个典型的例子。
<div>For Apache hackers
如果当前Apache API除了URI到文件名hook之外，还有一个文件名到文件名的hook， 就不需要这个标记了! 但是，如果没有这样一个hook，则此标记是唯一的解决方案。 Apache Group讨论过这个问题，并在Apache 2.0 版本中会增加这样一个hook。</div>
&nbsp;</li>
	<li>
<h4>’skip|S=num’ (跳过后继的规则 skip)</h4>
此标记强制重写引擎跳过当前匹配规则后继的num个规则。 它可以实现一个伪if-then-else的构造: 最后一个规则是then从句，而被跳过的skip=N个规则是else从句. (它和’chain|C’标记是不同的!)</li>
	<li>
<h4>‘env|E=VAR:VAL’ (设置环境变量 environment variable)</h4>
此标记使环境变量VAR的值为VAL, VAL可以包含可扩展的反向引用的正则表达式$N和%N。 此标记可以多次使用以设置多个变量。 这些变量可以在其后许多情况下被间接引用，但通常是在XSSI (via or CGI (如 $ENV{‘VAR’})中， 也可以在后继的RewriteCond指令的pattern中通过%{ENV:VAR}作引用。 使用它可以从URL中剥离并记住一些信息。</li>
	<li>
<h4>‘cookie|CO=NAME:VAL:domain[:lifetime[:path]]’ (设置cookie)</h4>
它在客户端浏览器上设置一个cookie。 cookie的名称是NAME，其值是VAL。 domain字段是该cookie的域，比如’.apache.org’, 可选的lifetime是cookie生命期的分钟数， 可选的path是cookie的路径。</li>
</ol>
<strong>深入阅读：http://oss.org.cn/man/newsoft/ApacheManual/mod/mod_rewrite.html</strong>

</div>
</div>
~.~.~.~.~.~.~

apache中mod_rewrite的修正符:

F(force URL to be forbidden)

禁用URL,返回403HTTP状态码。

R[=code](force redirect)

强制外部重定向强制在替代字符串加上http://thishost[:thisport]/前缀重定向到外部的URL.如果code不指定，将用缺省的302 HTTP状态码。

G(force URL to be gone)

强制URL为GONE，返回410HTTP状态码。

P(force proxy)

强制使用代理转发。

L(last rule)

表明当前规则是最后一条规则，停止分析以后规则的重写。

N(next round)

重新从第一条规则开始运行重写过程。

C(chained with next rule)

与下一条规则关联如果规则匹配则正常处理，该标志无效，如果不匹配，那么下面所有关联的规则都跳过。

T=MIME-type(force MIME type)

强制MIME类型

NS (used only if no internal sub-request)

只用于不是内部子请求

NC(no case)

不区分大小写

QSA(query string append)

追加请求字符串

NE(no URI escaping of output)

不在输出转义特殊字符

例如：RewriteRule /foo/(.*) /bar?arg=P1\%3d$1 [R,NE] 能正确的将/foo/zoo转换成/bar?arg=P1=zed

PT(pass through to next handler)

传递给下一个处理

例如：
RewriteRule ^/abc(.*) /def$1 [PT] # 将会交给/def规则处理

Alias /def /ghi

S=num(skip next rule(s)) 跳过num条规则

E=VAR:VAL(set environment variable) 设置环境变量

/**/

'nocase|NC'(no case)忽略大小

'ornext|OR' (or next condition)逻辑或，可以同时匹配多个RewriteCond条件

RewriteRule适用的标志符

'redirect|R [=code]' (force redirect)强迫重写为基于http开头的外部转向(注意URL的变化) 如：[R=301,L]

'forbidden|F' (force URL to be forbidden)重写为禁止访问

'proxy|P' (force proxy)重写为通过代理访问的http路径

'last|L' (last rule)最后的重写规则标志，如果匹配，不再执行以后的规则

'next|N' (next round)循环同一个规则，直到不能满足匹配

'chain|C' (chained with next rule)如果匹配该规则，则继续下面的有Chain标志的规则。

'type|T=MIME-type' (force MIME type)指定MIME类型

'nocase|NC' (no case)忽略大小

'nosubreq|NS' (used only if no internal sub-request)如果是内部子请求则跳过

'qsappend|QSA' (query string append)附加查询字符串

'noescape|NE' (no URI escaping of output)禁止URL中的字符自动转义成%[0-9]+的形式。

'passthrough|PT' (pass through to next handler)将重写结果运用于mod_alias

'skip|S=num' (skip next rule(s))跳过下面几个规则

'env|E=VAR:VAL' (set environment variable)添加环境变量

/**/

Rewrite规则表达式的说明：

. 匹配任何单字符

[chars] 匹配字符串:chars

[^chars] 不匹配字符串:chars

text1|text2 可选择的字符串:text1或text2

? 匹配0到1个字符

* 匹配0到多个字符

+ 匹配1到多个字符

^ 字符串开始标志

$ 字符串结束标志

\n 转义符标志

反向引用 $N 用于 RewriteRule 中匹配的变量调用(0 &lt;= N &lt;= 9)

反向引用 %N 用于 RewriteCond 中最后一个匹配的变量调用(1 &lt;= N &lt;= 9)

推荐阅读本站的：<a title="XAMP-mod_rewrite规则的使用与规则修正符与.htaccess RewriteRule详解" href="http://blog.ipodmp.com/archives/the-mod_rewrite-rules-using-rule-modifier/">XAMP-mod_rewrite规则的使用与规则修正符与.htaccess RewriteRule详解</a>