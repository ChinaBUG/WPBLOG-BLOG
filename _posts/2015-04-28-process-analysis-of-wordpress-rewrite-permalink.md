---
ID: 4003
post_title: >
  WordPress-Rewrite/Permalink内部过程分析
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/04/process-analysis-of-wordpress-rewrite-permalink.html
published: true
post_date: 2015-04-28 15:02:35
---
虫曰：非常不错的一篇关于WordPress重写，固定连接格式的文章，从源码底层的原理分析起，值得推荐。虫子就修改了一下文章格式更方便阅读。

~.~

本文说明WP 对URL rewrite并生成当前请求的过程。

<strong>关于Query Vars</strong>

这是 Wordpress全部代码中最重要的变量,所谓的query vars是一系列变量集合。

WP通过解析URL设定query vars, 并通过分析query vars值决定显示那些文章,设定标志位等。所谓标志位是WP_Query类中一系列$is_xxx形式布尔成员变量,所有的is_xxx()形式 template tag实际上都是返回$wp_query里对应成员变量值。

举例而言,如果当前页面是单篇文章, 则p这个Query Var(以下简称变量)值不为空。(在WP类里空的query var根本不存在,而WP_Query类里如果对应name的query var没有设置,$wp_query-&gt;query_vars[‘varname’]被填充为空值), 如果当前为搜索页, s变量值则为搜索关键字. 如果p和page两个变量都不为空值, 则当前为单篇文章分页页面, 依次类推。

Query Vars在WP类($wp)里根据WP_Rewrite里的rewrite规则生成, 在WP_Query($wp_query)类里这些变量被用来建立主循环.

WP和WP_Query类里都有fill_query_vars成员变量,键值为query vars里的varname. 具体的query vars包括以下公开变量,
<pre class="brush:php;">function fill_query_vars($array) {
$keys = array(
'error'
, 'm'
, 'p'
, 'post_parent'
, 'subpost'
, 'subpost_id'
, 'attachment'
, 'attachment_id'
, 'name'
, 'hour'
, 'static'
, 'pagename'
, 'page_id'
, 'second'
, 'minute'
, 'hour'
, 'day'
, 'monthnum'
, 'year'
, 'w'
, 'category_name'
, 'tag'
, 'cat'
, 'tag_id'
, 'author_name'
, 'feed'
, 'tb'
, 'paged'
, 'comments_popup'
, 'meta_key'
, 'meta_value'
, 'preview'
);
}</pre>
以及一些private变量. private变量不能由rewrite/GET等方式生成, 所以我们这里说的都指公开变量(public query vars)

<strong>准备知识:</strong>

a.WP初始化过程：基本过程是index.php -&gt;wp-blog.header.php -&gt;wp-load.php .
通过wp-load.php 先后包含了wp-config.php, wp-setting.php,classes.php,fucntions.php query.php等文件,

b. 建立变量：在wp-setting.php文件中建立了三个全局变量,$wp_the_query,$wp_rewrite和$wp ,分别为WP_Query,WP_Rewrite和WP类的实例,另外建立了一个$wp_query=&amp;$wp_the_query,
<pre class="brush:html;">$wp_the_query =&amp; new WP_Query();
$wp_query     =&amp; $wp_the_query;
$wp_rewrite   =&amp; new WP_Rewrite();
$wp           =&amp; new WP();</pre>
(之所以这样做是为了通过query_posts等方式新建自定义查询 时不会损坏WP主循环,在自定义查询结束后可以调用wp_reset_query把$wp_query还原为$wp_the-query引用). 然后,wp-blog-header执行wp()函数,并通过其调用$wp所属WP类的main方法,这个方法又调用一系列方法,

c.解析URL：最重要的是classes.php文件中parse_request方法, WP从这里开始解析URL并建立主循环.
<pre class="brush:html;">function main($query_args = ”) {
$this-&gt;init(); //初始化，获取当前用户信息
$this-&gt;parse_request($query_args); //解析请求
$this-&gt;send_headers(); //发送头信息
$this-&gt;query_posts(); //查询日志
$this-&gt;handle_404(); //操作404(URL地址不存在)
$this-&gt;register_globals(); //注册全局变量
do_action_ref_array(’wp’, array(&amp;$this));
}</pre>
我们假设使用了友好的permalink,并且通过Apache下.htaccess实现。

那么 ,WP类的parse_request方法建立一个$request变量,这个变量值是$_SERVER[‘REUEST_URI’] 或$_SERVER[‘PATH_INFO’]经过处理后的值, 是request_uri还是path_info取决于当前是否URL是否pathinfo类型请求。

处理过程包括,移除request_uri和path_info里?以后部分(即所有GET参数). 去除request url开始的/ , 如果你WP安装在子目录(如wordpress/目录), 从(request_uri和pathinfo)开头去除wordpress/ , 去除末尾的’/’ , 对request_uri进行rawurldecode等. ($_SERVER[‘PATH_INFO’]的值已经是decode过的了,所以无需由wp处理)。

当一切完成后, $request就是一个规范化的当前请求filename, 形如 post-slug, date/YYYY/mm, tag/tag-slug之类 , 接下来,WP根据rewrite规则逐条对$request进行匹配(preg_match), 如果找到一个匹配, 建立相关的变量(query vars);如果没有任何匹配,则为404。

所谓的Rewrite规则就是关联数组, 键值为用来匹配$request的正则表达式, 值为解析的变量, 如'([0-9]+)(/[0-9]+)?/?$’ =&gt; ‘index.php?p=$matches[1]&amp;page=$matches[2]’ 就是一条规则. 具体解析过程稍后会有介绍.在WP类里会调用$wp_rewrite的wp_rewrite_rules方法获取rewrite规则.注意Rewrite 规则有缓存的, 保存在数据库wp_option表,option name为rewrite_rules.如果数据库里这个option值为空,wp_rewrite_rules()会根据你的permalink structure重新生成rewrite规则, 并保存到数据库中.在后台更改永久链接结构时也会重新生成rewrite规则并保存到数据库中.

具体的过程,举例说明,如果你使用了 /%post_id% 形式的permalink, 当前URL是 <a href="http://domain.com/18">http://domain.com/18</a>. 解析出来的$request则是18 . WP对rewrite数组里每条规则$match用preg_match(“!^$match!”, $request, $matches)语句尝试匹配, 发现$request与 ‘([0-9]+)(/[0-9]+)?/?$’ =&gt; ‘index.php?p=$matches[1]&amp;page=$matches[2]’ 这条规则匹配, preg_match把 ’18’ 和空值保存到$matches数组里, 然后WP提取出匹配项值?后部分’p=$matches[1]&amp;page=$matches[2]’,对这个字符串$query执行 eval(“@\$query = \”” . addslashes($query) . “\”;”);语句, 这句的目的是把$matches[1]替换为18, 把 $matches[2]替换为空. 所以$query值变为’p=671&amp;page=’, 然后执行parse_str($query, $perma_query_vars); 这样现在$perma_query_vars数组里就有键值’为p’和’page’的值. 但这还不是最终的query vars, 最终生成的query vars是$GLOBALS, $_POST, $GET和$perma_query_vars里键值为变量名项集合.依次判断. 如果 $_POST[‘varname’]存在,则varname这个query var值为$_POST[‘varname’], 而不再继续判断$_GET[‘varname’]和$perma_query_vars[‘varname’]是否存在, 如此类推. 最后生成的query vars保存在 $wp-&gt;query_vars里.

如果使用的是默认的/?p=123 permalink形式,那么上面过程简单的多,只会从$GLOBALS, $_POST, $GET里提取query vars变量值,而不会解析REQUEST_URI和PATH_INFO.

上 面过程完成后会执行parse_request这个Action,然后执行$wp的query_posts方法,这个方法 把$wp-&gt;query_vars传给$wp_the_query的query方法,开始建立主循环,设定标志位(is_home, is_page …)等.接下来就是载入模板显示页面内容了.

<strong>下面分析一些问题:</strong>
1. 假设permalink设置为/%post_id%, 数据库中有id为 10和11的两篇文章, 访问<a href="http://domain.com/10/?p=11">http://domain.com/10/?p=11</a> ,显示什么内容? – –
答案是会显示id为11的文章,因为上面说的, $_GET里变量优先级高于rewrite rules里解析出来的变量($perma_query_vars),所以最后的p变量值为11.但这时WP通常会把当前页重定向到<a href="http://domain.com/11">http://domain.com/11</a>, 因为WP有canoninol机制(wp-includes/canonical.php),能够301重定向一些不规范地址到规范url, 典型的如把其他域名重定向到后台HOME和SITEURL里设定的域名里页面.但WP的canonical机制并不完善, 个人推荐使用permalink validator插件,这个插件判断当前URL与官方URL是否完全匹配,如果不匹配要么重定向,要么set 404,..

2. 假设permalink设置为/%post_name%, 访问<a href="http://domain.com/index.php/%post_name%">http://domain.com/index.php/%post_name%</a>结果如何?
如 果对应slug的post存在的话, 不会是404. 因为虽然你permalink没有设置为index.php形式的path info permalink,但如上面所说,WP在解析URL时并不判断后台设定的永久链接结构是path info还是request uri类型或者甚至是默认/?p=123类型. 不过这时通常会重定向到<a href="http://domain.com/%post_name%">http://domain.com/%post_name%</a> , 同样因为canonical机制.

3. 关于URL末尾的 ‘/’
URL 末尾是否有/对rewrite没有影响, 如上所述, WP类在生成$request时会去除requset_uri和path_info末尾/ ,另外,你注意到所有WP生成的rewrite规则, 键值正则式的末尾均为 /?$ ,表示URL末尾可选 / . 不过wp会判断当前URL末尾是否有/ 以及后台设置的permalink末尾是否有/, 如果不一致则301重定向, 还是因为canonical代码. 请注意WP的 canonical代码在template_redirect这个action处执行,这时WP实际上已经解析完query vars, 主循环也设置好了, 即将载入模板..

4. 假设permalink设置为/%post_name%,数据库里两个有slug都为’about’的post和page,访问<a href="http://domain.com/about">http://domain.com/about</a>, 显示哪个?
这 种情况由rewrite规则数组顺序决定, 通常wp生成的rewrite规则里 ‘(.+?)(/[0-9]+)?/?$’ =&gt; ‘index.php?pagename=$matches[1]&amp;page=$matches[2]’ 这条规则是最后一个, 所以这时会显示post而不是page.

5. 有人使用”古怪”的permalink结构,如/?p=%post_id%.html, 然后发现所有分类/tag页全部显示最新文章= =
原 因是在这种古怪的permalink结构下,WP根本无法正确获取query vars变量,无论从GET还是url rewrite,因为如上面所说,wp rewrite 的第一步就是去除request_uri和path_info里?以后部分.事实上在这种情况下单篇文章页能够正常显示也仅仅因为有一个通过GET传递的 p变量,值为 18.html这样形式,然后php 的 (int) 或intval() 把它变成了18 = =

6 .关于Verbose rewrite
以Apache为例,wp默认生成的.htaccess里mod_rewrite规则非常简单:
<pre class="brush:html;"># BEGIN WordPress
&lt;IfModule mod_rewrite.c&gt;
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
&lt;/IfModule&gt;
# END WordPress</pre>
如果当前请求不为文件或目录,把URL请求里第一个字符重写到index.php并停止继续 rewrite.然后WP会通过request_uri或path_info解析query vars, 如上面所述. WP还提供一种non verbose rewrite rules, 但并没有提供前台接口. 在wp-include/rewrite.php里把WP_Rewrite里var $use_verbose_rules = true; 这句赋值改为false, 后台重新保存一下permalink,你会发现.htaccess里内容已经变了:
<pre class="brush:html;"># BEGIN WordPress
&lt;IfModule mod_rewrite.c&gt;
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} -f [OR]
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^robots\.txt$ /index.php?robots=1 [QSA,L]
RewriteRule ^.*wp-atom.php$ /index.php?feed=atom [QSA,L]
RewriteRule ^.*wp-rdf.php$ /index.php?feed=rdf [QSA,L]
RewriteRule ^.*wp-rss.php$ /index.php?feed=rss [QSA,L]
RewriteRule ^.*wp-rss2.php$ /index.php?feed=rss2 [QSA,L]
RewriteRule ^.*wp-feed.php$ /index.php?feed=feed [QSA,L]
RewriteRule ^.*wp-commentsrss2.php$ /index.php?feed=rss2&amp;withcomments=1 [QSA,L]
RewriteRule ^feed/(feed|rdf|rss|rss2|atom)/?$ /index.php?&amp;feed=$1 [QSA,L]
RewriteRule ^(feed|rdf|rss|rss2|atom)/?$ /index.php?&amp;feed=$1 [QSA,L]
.....
RewriteRule ^(.+)/page/?([0-9]{1,})/?$ /index.php?pagename=$1&amp;paged=$2 [QSA,L]
RewriteRule ^(.+)/comment-page-([0-9]{1,})/?$ /index.php?pagename=$1&amp;cpage=$2 [QSA,L]
RewriteRule ^(.+)(/[0-9]+)?/?$ /index.php?pagename=$1&amp;page=$2 [QSA,L]
&lt;/IfModule&gt;
# END WordPress</pre>
你可能对这种rewrite规则更为熟悉,国内的程序基本上都是用这种Rewrite. 请注意这时WP的内部过程完全不同. 在这种情况下, WP 的query vars值均来源于$_GET (Apache直接rewrite生成的), 但Request_uri或Path_Info仍会被解析并且生成的$perma_query_vars完全正确,! 只是不会被用于query vars而已. 因为$_GET优先级高于对url rewrite获得的值. 有人在windows下IIS的httpd.ini里加入rewrite规则,后台permalink设置为默认后rewrite后友好地址仍可以访问, 就是这个原因.

如果你能看懂(或早已知道)上面内容, 那么, 我相信对所有关于Wordpress rewrite/permalink方面的问题,你都能够解决,或者至少找到思路.至少,我是这样的.XT.

~.~

非常棒的一篇文章，感謝作者，原文摘自《<a href="http://www.onexin.net/wordpress-rewrite-permalink-internal-process-analysis/">WordPress Rewrite / Permalink内部过程分析</a>》