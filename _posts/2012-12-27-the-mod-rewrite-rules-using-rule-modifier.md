---
ID: 2234
post_title: >
  XAMP-mod_rewrite规则的使用与规则修正符与.htaccess
  RewriteRule详解
author: ChinaBUG
post_excerpt: |
  一、mod_rewrite 规则的使用
  二、mod_rewrite 规则修正符
  三、附录：一些问题的处理方案
  2013.04.10 处理ShopEX开启伪静态造成中文乱码、搜索编码错误导致搜索不到商品信息
  打开.htaccess文件，找到如下内容（没有的话就自己在ShopEX开启伪静态，让ShopEX自动生成一个吧）
  RewriteRule ^(.*)$ index.php?$1 [L]
  然后修改为下面的内容：
  RewriteRule ^(.*)$ index.php?$1 [QSA,NU,PT,L]
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/the-mod-rewrite-rules-using-rule-modifier.html
published: true
post_date: 2012-12-27 14:21:51
---
一、mod_rewrite 规则的使用

RewriteEngine on

RewriteCond %{HTTP_HOST} !^www.ipodmp.com [NC]

RewriteRule ^/(.*) <a href="http://www.php100.com/">http://www.ipodmp.com/</a> [L]

RewriteEngine on

RewriteRule ^/test([0-9]*).html$ /test.php?id=$1

RewriteRule ^/new([0-9]*)/$ /new.php?id=$1 [R]

二、mod_rewrite 规则修正符

1、 R[=code](force redirect) 强制外部重定向
强制在替代字符串加上http://thishost[:thisport]/前缀重定向到外部的URL.如果code不指定，将用缺省的302 HTTP状态码。
2、F(force URL to be forbidden)禁用URL,返回403HTTP状态码。
3、G(force URL to be gone) 强制URL为GONE，返回410HTTP状态码。
4、P(force proxy) 强制使用代理转发。
5、L(last rule) 表明当前规则是最后一条规则，停止分析以后规则的重写。
6、N(next round) 重新从第一条规则开始运行重写过程。
7、C(chained with next rule) 与下一条规则关联
8、T=MIME-type(force MIME type) 强制MIME类型
9、NS (used only if no internal sub-request) 只用于不是内部子请求
10、NC(no case) 不区分大小写
11、QSA(query string append) 追加请求字符串
12、NE(no URI escaping of output) 不在输出转义特殊字符
例如：RewriteRule /foo/(.*) /bar?arg=P1\%3d$1 [R,NE] 将能正确的将/foo/zoo转换成/bar?arg=P1=zed
13、PT(pass through to next handler) 传递给下一个处理
例如：
RewriteRule ^/abc(.*) /def$1 [PT] # 将会交给/def规则处理   Alias /def /ghi
14、S=num(skip next rule(s)) 跳过num条规则
15、E=VAR:VAL(set environment variable) 设置环境变量

三、附录：一些问题的处理方案

2013.04.10 处理ShopEX开启伪静态造成中文乱码、搜索编码错误导致搜索不到商品信息

打开.htaccess文件，找到如下内容（没有的话就自己在ShopEX开启伪静态，让ShopEX自动生成一个吧）

RewriteRule ^(.*)$ index.php?$1 [L]

然后修改为下面的内容：

RewriteRule ^(.*)$ index.php?$1 [QSA,NU,PT,L]

保存，清空缓存，然后你会发现问题解决噢。

另外，推荐阅读基础版的信息：<a title="rewrite-重写规则之修正符及规则表达式说明" href="http://blog.ipodmp.com/archives/rewrite-expression-shows-that-the-modifiers-and-rules-of-the-rewrite-rules-in-the-rewrite/">rewrite-重写规则之修正符及规则表达式说明</a>

-+-+-+-+-+

文摘

-+-+-+-+-+
<h2>.htaccess RewriteRule详解</h2>
<strong>1、Rewrite规则简介：</strong>

Rewirte 主要的功能就是实现URL的跳转，它的正则表达式是基于Perl语言。可基于服务器级的(httpd.conf)和目录级的(.htaccess)两种方 式。如果要想用到rewrite模块，必须先安装或加载rewrite模块。方法有两种一种是编译apache的时候就直接安装rewrite模块，别一 种是编译apache时以DSO模式安装apache,然后再利用源码和apxs来安装rewrite模块。

基于服务器级的 (httpd.conf)有两种方法，一种是在httpd.conf的全局下直接利用RewriteEngine on来打开rewrite功能;另一种是在局部里利用RewriteEngine on来打开rewrite功能,下面将会举例说明，需要注意的是,必须在每个virtualhost里用RewriteEngine on来打开rewrite功能。否则virtualhost里没有RewriteEngine on它里面的规则也不会生效。

基于目录级的(.htaccess),要注意一点那就是必须打开此目录的FollowSymLinks属性且在.htaccess里要声明RewriteEngine on。

<strong>2、举例说明：</strong>

下 面是在一个虚拟主机里定义的规则。功能是把client请求的主机前缀不是www.colorme.com和203.81.23.202都跳转到主机前缀 为http://www.colorme.com.cn，避免当用户在地址栏写入http://colorme.com.cn时不能以会员方式登录网站。

NameVirtualHost 192.168.100.8:80

ServerAdmin webmaster@colorme.com.cn
DocumentRoot "/web/webapp"
ServerName www.colorme.com.cn
ServerName colorme.com.cn
RewriteEngine on #打开rewirte功能
RewriteCond %{HTTP_HOST} !^www.colorme.com.cn [NC] #声明Client请求的主机中前缀不是www.colorme.com.cn,[NC]的意思是忽略大小写
RewriteCond %{HTTP_HOST} !^203.81.23.202 [NC] #声明Client请求的主机中前缀不是203.81.23.202,[NC]的意思是忽略大小写
RewriteCond %{HTTP_HOST} !^$ #声明Client请求的主机中前缀不为空,[NC]的意思是忽略大小写
RewriteRule ^/(.*) http://www.colorme.com.cn/ [L]
# 含义是如果Client请求的主机中的前缀符合上述条件，则直接进行跳转到http://www.colorme.com.cn/,[L]意味着立即停止 重写操作，并不再应用其他重写规则。这里的.*是指匹配所有URL中不包含换行字符，()括号的功能是把所有的字符做一个标记，以便于后面的应用.就是引 用前面里的(.*)字符。
例二.将输入 folio.test.com 的域名时跳转到profile.test.com

listen 8080
NameVirtualHost 10.122.89.106:8080
ServerAdmin webmaster@colorme.com.cn
DocumentRoot "/usr/local/www/apache22/data1/"
ServerName profile.test.com
RewriteEngine on
RewriteCond %{HTTP_HOST} ^folio.test.com [NC]
RewriteRule ^/(.*) http://profile.test.com/ [L]

<strong>3.Apache mod_rewrite规则重写的标志一览</strong>

1) R[=code](force redirect) 强制外部重定向
强制在替代字符串加上http://thishost[:thisport]/前缀重定向到外部的URL.如果code不指定，将用缺省的302 HTTP状态码。
2) F(force URL to be forbidden)禁用URL,返回403HTTP状态码。
3) G(force URL to be gone) 强制URL为GONE，返回410HTTP状态码。
4) P(force proxy) 强制使用代理转发。
5) L(last rule) 表明当前规则是最后一条规则，停止分析以后规则的重写。
6) N(next round) 重新从第一条规则开始运行重写过程。
7) C(chained with next rule) 与下一条规则关联

如果规则匹配则正常处理，该标志无效，如果不匹配，那么下面所有关联的规则都跳过。

8) T=MIME-type(force MIME type) 强制MIME类型
9) NS (used only if no internal sub-request) 只用于不是内部子请求
10) NC(no case) 不区分大小写
11) QSA(query string append) 追加请求字符串
12) NE(no URI escaping of output) 不在输出转义特殊字符
例如：RewriteRule /foo/(.*) /bar?arg=P1\%3d$1 [R,NE] 将能正确的将/foo/zoo转换成/bar?arg=P1=zed
13) PT(pass through to next handler) 传递给下一个处理
例如：
RewriteRule ^/abc(.*) /def$1 [PT] # 将会交给/def规则处理
Alias /def /ghi
14) S=num(skip next rule(s)) 跳过num条规则
15) E=VAR:VAL(set environment variable) 设置环境变量

<strong>4.Apache rewrite例子集合</strong>

在 httpd 中将一个域名转发到另一个域名虚拟主机世界近期更换了域名，新域名为 www.wbhw.com, 更加简短好记。这时需要将原来的域名webhosting-world.com, 以及论坛所在地址 webhosting-world.com/forums/定向到新的域名，以便用户可以找到，并且使原来的论坛 URL 继续有效而不出现 404 未找到，比如原来的http://www.webhosting-world.com/forums/-f60.html, 让它在新的域名下继续有效，点击后转发到http://bbs.wbhw.com/-f60.html, 这就需要用 apache 的 Mod_rewrite 功能来实现。

在中添加下面的重定向规则：

RewriteEngine On
# Redirect webhosting-world.com/forums to bbs.wbhw.com
RewriteCond %{REQUEST_URI} ^/forums/
RewriteRule /forums/(.*) http://bbs.wbhw.com/$1 [R=permanent,L]
# Redirect webhosting-world.com to wbhw.com
RewriteCond %{REQUEST_URI} !^/forums/
RewriteRule /(.*) http://www.wbhw.com/$1 [R=permanent,L]

添加了上面的规则以后， 里的全部内容如下：

ServerAlias webhosting-world.com
ServerAdmin admin@webhosting-world.com
DocumentRoot /path/to/webhosting-world/root
ServerName www.webhosting-world.com
RewriteEngine On
# Redirect webhosting-world.com/forums to bbs.wbhw.com
RewriteCond %{REQUEST_URI} ^/forums/
RewriteRule /forums/(.*) http://bbs.wbhw.com/$1 [R=permanent,L]
# Redirect webhosting-world.com to wbhw.com
RewriteCond %{REQUEST_URI} !^/forums/
RewriteRule /(.*) http://www.wbhw.com/$1 [R=permanent,L]

<strong>URL重定向</strong>

例子一:

1.http://www.zzz.com/xxx.php-&gt; http://www.zzz.com/xxx/
2.http://yyy.zzz.com-&gt; http://www.zzz.com/user.php?username=yyy 的功能
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www.zzz.com
RewriteCond %{REQUEST_URI} !^user\.php$
RewriteCond %{REQUEST_URI} \.php$
RewriteRule (.*)\.php$ http://www.zzz.com/$1/ [R]
RewriteCond %{HTTP_HOST} !^www.zzz.com
RewriteRule ^(.+) %{HTTP_HOST} [C]
RewriteRule ^([^\.]+)\.zzz\.com http://www.zzz.com/user.php?username=$1

例子二：

/type.php?typeid=*   --&gt; /type*.html
/type.php?typeid=*&amp;page=*   --&gt; /type*page*.html
RewriteRule ^/type([0-9]+).html$ /type.php?typeid=$1 [PT]
RewriteRule ^/type([0-9]+)page([0-9]+).html$ /type.php?typeid=$1&amp;page=$2 [PT]

<strong>5.使用Apache的URL Rewrite配置多用户虚拟服务器</strong>

要实现这个功能，首先要在DNS服务器上打开域名的泛域名解析（自己做或者找域名服务商做）。比如，我就把 *.semcase.com和 *.semcase.cn全部解析到了我的这台Linux Server上。

然后，看一下我的Apache中关于*.semcase.com的虚拟主机的设定。

#*.com,*.osall.net

ServerAdmin webmaster@semcase.com
DocumentRoot /home/www/www.semcase.com
ServerName dns.semcase.com
ServerAlias dns.semcase.com semcase.com semcase.net *.semcase.com *.semcase.net
CustomLog /var/log/httpd/osa/access_log.log" common
ErrorLog /var/log/httpd/osa/error_log.log"
AllowOverride None
Order deny,allow
#AddDefaultCharset GB2312

RewriteEngine on
RewriteCond %{HTTP_HOST}        ^[^.]+\.osall\.(com|net)$
RewriteRule ^(.+)     %{HTTP_HOST}$1   [C]
RewriteRule ^([^.]+)\.osall\.(com|net)(.*)$
/home/www/www.semcase.com/sylvan$3?un=$1&amp;%{QUERY_STRING} [L]
在这段设定中，我把*.semcase.net和*.semcase.com 的Document Root都设定到了 /home/www/www.semcase.com

但是，继续看下去，看到...配置了吗？在这里我就配置了URL Rewrite规则。
RewriteEngine on #打开URL Rewrite功能
RewriteCond %{HTTP_HOST} ^[^.]+.osall.(com|net)$ #匹配条件，如果用户输入的URL中主机名是类似 xxxx.semcase.com 或者 xxxx.semcase.cn 就执行下面一句
RewriteRule ^(.+) %{HTTP_HOST}$1 [C] #把用户输入完整的地址（GET方式的参数除外）作为参数传给下一个规则，[C]是Chain串联下一个规则的意思
RewriteRule ^([^.]+).osall.(com|net)(.*)$ /home/www/dev.semcase.com/sylvan$3?un=$1&amp;%{QUERY_STRING} [L]
# 最关键的是这一句，使用证则表达式解析用户输入的URL地址，把主机名中的用户名信息作为名为un的参数传给/home/www /dev.semcase.com目录下的脚本，并在后面跟上用户输入的GET方式的传入参数。并指明这是最后一条规则（[L]规则）。注意，在这一句中 指明的重写后的地址用的是服务器上的绝对路径，这是内部跳转。如果使用<a href="http://xxxx/">http://xxxx</a>这样的URL格式，则被称为外部跳转。使用外部跳转的话，浏览着的浏览器中的URL地址会改变成新的地址，而使用内部跳转则浏览器中的地址不发生改变，看上去更像实际的二级域名虚拟服务器。

例子：

&lt;FilesMatch "\.(bak|inc|lib|sh|tpl|lbi|dwt)$"&gt;
order deny,allow
deny from all
&lt;/FilesMatch&gt;

RewriteEngine on

RewriteCond %{HTTP_HOST} ^(flowerworld.cn)(:80)? [NC]
RewriteRule ^(.*)<a href="http://www.flowerworld.com.cn/$1">http://www.flowerworld.com.cn/$1</a>[R=301,L]
RewriteCond %{HTTP_HOST} ^(<a href="http://www.flowerworld.cn%29%28/">www.flowerworld.cn)(:80</a>)? [NC]
RewriteRule ^(.*)<a href="http://www.flowerworld.com.cn/$1">http://www.flowerworld.com.cn/$1</a>[R=301,L]
RewriteCond %{HTTP_HOST} ^(flowerworld.com.cn)(:80)? [NC]
RewriteRule ^(.*)<a href="http://www.flowerworld.com.cn/$1">http://www.flowerworld.com.cn/$1</a>[R=301,L]
RewriteRule ^index\.html$    index\.php [QSA,L]
RewriteRule ^m\.html$   view/admin/adminView\.php?pageAction=quit [QSA,L]

RewriteRule ^info/([0-9]+)\.html$ view/infoView.php?pageAction=viewInfo&amp;id=$1 [QSA,L]
RewriteRule ^sell/([0-9]+)\.html$ view/module/sellView.php?pageAction=viewSellInfo&amp;sellId=$1 [QSA,L]

RewriteRule ^superMarket/([0-9]+)\.html$ view/module/superMarketView.php?pageAction=viewSuperMarketInfo&amp;superMarketId=$1 [QSA,L]

RewriteRule ^product/([0-9]+)\.html$ view/productPostView.php?pageAction=productPostShow&amp;id=$1 [QSA,L]

RewriteRule ^productClass/([0-9]+)\.html$ view/productView.php?pageAction=productClassIndex&amp;id=$1 [QSA,L]

RewriteRule ^enterprise/([0-9]+)/(.*)-([0-9]+)-([0-9]+)$ view/enterpriseMemberView.php?pageAction=memberShow&amp;id=$1&amp;actionShow=$2&amp;showType=$3&amp;appendId=$4 [QSA,L]
RewriteRule ^enterprise/([0-9]+)/(.*)$ view/enterpriseMemberView.php?pageAction=memberShow&amp;id=$1&amp;actionShow=$2 [QSA,L]
RewriteRule ^enterprise/([0-9]+)\.html$ view/enterpriseMemberView.php?pageAction=memberShow&amp;id=$1 [QSA,L]
RewriteRule ^enterprise/([0-9]+)$ view/enterpriseMemberView.php?pageAction=memberShow&amp;id=$1 [QSA,L]

RewriteRule ^news\.html$ view/newsView.php [QSA,L]

RewriteRule ^enterprise\.html$ view/enterpriseView.php [QSA,L]
RewriteRule ^quotedPrice\.html$ newView/searchQpView.php [QSA,L]
RewriteRule ^product\.html$ view/productView.php [QSA,L]
RewriteRule ^sell\.html$ view/module/sellView.php [QSA,L]
RewriteRule ^wantToBuy\.html$ view/module/wantToBuyView.php [QSA,L]
RewriteRule ^superMarket\.html$ view/module/superMarketView.php [QSA,L]
RewriteRule ^help\.html$ view/newsView.php?pageAction=help [QSA,L]

RewriteRule ^column/([0-9]+)\.html$ view/newsView.php?pageAction=column&amp;id=$1 [QSA,L]

RewriteRule ^product/([0-9]+)\.html$ view/productPostView.php?pageAction=productPostShow&amp;id=$1 [QSA,L]

RewriteRule ^enterprise/([0-9]+)/(.*)-([0-9]+)-([0-9]+).html$ view/enterpriseMemberView.php?pageAction=memberShow&amp;id=$1&amp;actionShow=$2&amp;showType=$3&amp;appendId=$4 [QSA,L]

//结果是 enterprise/32/open-12-13.html
//32是参数1，open是参数2,12是参数3,13是参数4

&nbsp;

~..~..~..~..~..~..~..~..~..~..~..~..~..~

&nbsp;

<strong>1、.htaccess rewrite实例开始部分</strong>

Options +FollowSymLinks
RewriteEngine On
RewriteBase /

<strong>2、把不带www的域名地址重定向到带www地址</strong>

Options +FollowSymLinks
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} !^www\.metsky\.com$ [NC]
RewriteRule ^(.*)$ http://www.metsky.com/$1 [R=301,L]

<strong>3、终止重写死循环</strong>

RewriteCond %{REQUEST_URI} ^/(stats/|missing\.html|failed_auth\.html|error/).* [NC]
RewriteRule .* - [L]

RewriteCond %{ENV:REDIRECT_STATUS} 200
RewriteRule .* - [L]

<strong>4、缓存友好文件名</strong>

这个是原作者的最爱，一般是面向JS和CSS等缓冲文件处理，通过设置不同的名字（比如版本或数字编号）来达到让用户更新（定期）缓存的目的。然后 在服务器端，很可能始终就是同一个文件。下面的例子似乎重写 /zap/j/anything-（任意数字）.js有关的所有文件，重写为/zap/j/anything.js，CSS处理也类似，把/zap/c /anything-（任意数字）.css重写为/zap/c/anything.css。

RewriteRule ^zap/(j|c)/([a-z]+)-([0-9]+)\.(js|css)$ /zap/$1/$2.$4 [L]

<strong>5、为无FLASH浏览器设置SEO友好链接</strong>

如果用户的浏览器不支持FLASH，可能需要进行友好提醒，那么以下的rewrite重写规则可以让我们少些很多代码。使用时只需要调用自己的域名加上地址调用就可以了。

RewriteRule ^getflash/?$ http://www.adobe.com/shockwave/download/download.cgi?P1_Prod_Version=ShockwaveFlash [NC,L,R=307]

<strong>6、移除查询Query string字符串</strong>

以下的重写规则会让你少很多SEO上的重复内容，比如page.html和page.html?anything=anything文件，简单的方法就是通过重定向这些带query string的外部请求到没有query string目标地址。

RewriteCond %{THE_REQUEST} ^GET\ /.*\;.*\ HTTP/
RewriteCond %{QUERY_STRING} !^$
RewriteRule .* http://www.metsky.com%{REQUEST_URI}? [R=301,L]

<strong>7、发送请求给PHP脚本</strong>

这个.htaccess rewrite示例对pdf-script.php处理Adobe pdf请求文件完全隐匿。

RewriteRule ^(.+)\.pdf$ /cgi-bin/pdf-script.php?file=$1.pdf [L,NC,QSA]

<strong>8、设置客户端的语言变量</strong>

对支持多语言显示的网页，可以看一下下面的重写。

RewriteCond %{HTTP:Accept-Language} ^.*(de|es|fr|it|ja|ru|en).*$ [NC]
RewriteRule ^(.*)$ - [env=prefer-language:%1]

<strong>9、只支持PHP fopen访问，其它一概拒绝</strong>

RewriteEngine On
RewriteBase /
RewriteCond %{THE_REQUEST} ^.+$ [NC]
RewriteRule .* - [F,L]

<strong>10、除php fopen可以访问某个子文件夹，其它一概拒绝</strong>

下面的示例可以让你的服务器媒体文件或特殊下载文件只能通过PHP代理脚本下载。

RewriteEngine On
RewriteBase /
RewriteCond %{THE_REQUEST} ^[A-Z]{3,9}\ /([^/]+)/.*\ HTTP [NC]
RewriteRule .* - [F,L]

<strong>11、重定向带www到不带www网址</strong>

Options +FollowSymLinks
RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_HOST} !^abc\.com$ [NC]
RewriteRule ^(.*)$ http://abc.com/$1 [R=301,L]

<strong>12、检查Query string关键字</strong>

下面的示例是检查QUERY_STRING是否包含passkey，否则将跳到登录脚本去。

RewriteEngine On
RewriteBase /
RewriteCond %{QUERY_STRING} !passkey
RewriteRule ^/logged-in/(.*)$ /login.php [L]

<strong>13、去除URL中的QUERY_STRING </strong>

以下示例去除所有的QUERY_STRING，包括在问好后面跟个空格，都将被mod_rewrite先去除，然后再重定向。

RewriteEngine On
RewriteBase /
RewriteCond %{QUERY_STRING} .
RewriteRule ^login.php /login.php? [L]

<strong>14、修复无限循环</strong>

由于潜在的配置错误引起的内部重定向请求超过10次就会有一个条错误消息，如果需要增加次数，可以使用LimitInternalRecursion。还可以使用LogLevel debug进行后向跟踪。

RewriteCond %{ENV:REDIRECT_STATUS} 200
RewriteRule .* - [L]

<strong>15、重定向PHP文件到HTML文件</strong>

RewriteRule ^(.*)\.php$ /$1.html [R=301,L]

<strong>16、重定向.html到.php文件</strong>

RewriteRule ^(.*)\.html$ $1.php [R=301,L]

<strong>17、规定时间阻止访问某些文件</strong>

Options +FollowSymLinks
RewriteEngine On
RewriteBase /
# If the hour is 16 (4 PM) Then deny all access
RewriteCond %{TIME_HOUR} ^16$
RewriteRule ^.*$ - [F,L]

<strong>18、下划线改成连字符</strong>

Options +FollowSymLinks
RewriteEngine On
RewriteBase /

RewriteRule !\.(html|php)$ - [S=4]
RewriteRule ^([^_]*)_([^_]*)_([^_]*)_([^_]*)_(.*)$ $1-$2-$3-$4-$5 [E=uscor:Yes]
RewriteRule ^([^_]*)_([^_]*)_([^_]*)_(.*)$ $1-$2-$3-$4 [E=uscor:Yes]
RewriteRule ^([^_]*)_([^_]*)_(.*)$ $1-$2-$3 [E=uscor:Yes]
RewriteRule ^([^_]*)_(.*)$ $1-$2 [E=uscor:Yes]

RewriteCond %{ENV:uscor} ^Yes$
RewriteRule (.*) http://d.com/$1 [R=301,L]

<strong>19、重定向Wordpress Feeds到Feedburner</strong>

RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_URI} ^/feed\.gif$
RewriteRule .* - [L]

RewriteCond %{HTTP_USER_AGENT} !^.*(FeedBurner|FeedValidator) [NC]
RewriteRule ^feed/?.*$ http://feeds.feedburner.com/<a href="http://www.metsky.com/archives/35.html" target="_blank">apache</a>/htaccess [L,R=302]

RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]

<strong>20、只允许GET和PUT请求</strong>

RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_METHOD} !^(GET|PUT)
RewriteRule .* - [F]

<strong>21、图像、文件防盗链</strong>

RewriteEngine On
RewriteBase /
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)?metsky.com/.*$ [NC]
RewriteRule \.(gif|jpg|swf|flv|png)$ /feed/ [R=302,L]

<strong>22、停止浏览器预存取</strong>

RewriteEngine On
SetEnvIfNoCase X-Forwarded-For .+ proxy=yes
SetEnvIfNoCase X-moz prefetch no_access=yes

# 阻止X-moz头的预存取请求
RewriteCond %{ENV:no_access} yes
RewriteRule .* - [F,L]
<h2>23、把abc.php?id=123重写到def.php?fid=123</h2>
RewriteCond %{QUERY_STRING} ^id=(.*)$
RewriteRule ^abc.php$ def.php?fid=%1 [L]

本文来源于国外文章翻译整理，有所添加，附原文地址：<a href="http://www.askapache.com/htaccess/mod_rewrite-tips-and-tricks.html" target="_blank">http://www.askapache.com/htaccess/mod_rewrite-tips-and-tricks.html</a>