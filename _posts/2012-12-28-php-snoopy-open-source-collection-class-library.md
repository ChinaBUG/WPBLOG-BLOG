---
ID: 2250
post_title: PHP-snoopy开源信息采集类库
author: ChinaBUG
post_excerpt: |
  最近学习PHP采集程序编写的时候，发现一个很好的采集类库，它的名字叫Snoopy。
  sorceforge上有下载地址： http://sourceforge.net/project/showfiles.php?group_id=2091，它可以模拟浏览器来获取网页内容，甚至以get或者post方式发送表单数据都可以。
  Snoopy的一些特点:
  1、抓取网页的内容 fetch
  2、抓取网页的文本内容 (去除HTML标签) fetchtext
  3、抓取网页的链接，表单 fetchlinks fetchform
  4、支持代理主机
  5、支持基本的用户名/密码验证
  6、支持设置 user_agent, referer(来路), cookies 和 header content(头文件)
  7、支持浏览器重定向，并能控制重定向深度
  8、能把网页中的链接扩展成高质量的url(默认)
  9、提交数据并且获取返回值
  10、支持跟踪HTML框架
  11、支持重定向的时候传递cookies 要求php4以上就可以了由于本身是php一个类无需扩支持服务器不支持curl时候的最好选择。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/php-snoopy-open-source-collection-class-library.html
published: true
post_date: 2012-12-28 16:57:26
---
最近学习PHP采集程序编写的时候，发现一个很好的采集类库，它的名字叫Snoopy。

sorceforge上有下载地址： <a href="http://sourceforge.net/project/showfiles.php?group_id=2091">http://sourceforge.net/project/showfiles.php?group_id=2091</a>，它可以模拟浏览器来获取网页内容，甚至以get或者post方式发送表单数据都可以。

直接下载：<a href="/projects/snoopy/files/latest/download?source=files">点击直接下载</a>

Snoopy的一些特点:

1、抓取网页的内容 fetch

2、抓取网页的文本内容 (去除HTML标签) fetchtext

3、抓取网页的链接，表单 fetchlinks fetchform

4、支持代理主机

5、支持基本的用户名/密码验证

6、支持设置 user_agent, referer(来路), cookies 和 header content(头文件)

7、支持浏览器重定向，并能控制重定向深度

8、能把网页中的链接扩展成高质量的url(默认)

9、提交数据并且获取返回值

10、支持跟踪HTML框架

11、支持重定向的时候传递cookies 要求php4以上就可以了由于本身是php一个类无需扩支持服务器不支持curl时候的最好选择。

&nbsp;

类方法:

fetch($URI) ———–

这是为了抓取网页的内容而使用的方法。 $URI参数是被抓取网页的URL地址。 抓取的结果被存储在 $this-&gt;results 中。 如果你正在抓取的是一个框架，Snoopy将会将每个框架追踪后存入数组中，然后存入 $this-&gt;results。

fetchtext($URI) —————

本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中的文字内容。

fetchform($URI) —————

本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中表单内容(form)。

fetchlinks($URI) —————-

本方法类似于fetch()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中链接(link)。 默认情况下，相对链接将自动补全，转换成完整的URL。

submit($URI,$formvars) ———————-

本方法向$URL指定的链接地址发送确认表单。$formvars是一个存储表单参数的数组。

submittext($URI,$formvars) ————————–

本方法类似于submit()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回登陆后网页中的文字内容。

submitlinks($URI) —————-

本方法类似于submit()，唯一不同的就是本方法会去除HTML标签和其他的无关数据，只返回网页中链接(link)。 默认情况下，相对链接将自动补全，转换成完整的URL。

类属性: (缺省值在括号里)

$host 连接的主机

$port 连接的端口

$proxy_host 使用的代理主机，如果有的话

$proxy_port 使用的代理主机端口，如果有的话

$agent 用户代理伪装 (Snoopy v0.1)

$referer 来路信息，如果有的话

$cookies cookies， 如果有的话

$rawheaders 其他的头信息, 如果有的话

$maxredirs 最大重定向次数， 0=不允许 (5)

$offsiteok whether or not to allow redirects off-site. (true) $expandlinks 是否将链接都补全为完整地址 (true)

$user 认证用户名, 如果有的话

$pass 认证用户名, 如果有的话

$accept http 接受类型 (image/gif, image/x-xbitmap, image/jpeg, image/pjpeg, */*)

$error 哪里报错, 如果有的话

$response_code 从服务器返回的响应代码

$headers 从服务器返回的头信息

$maxlength 最长返回数据长度

$read_timeout 读取操作超时 (requires PHP 4 Beta 4+) 设置为0为没有超时

$timed_out 如果一次读取操作超时了，本属性返回 true (requires PHP 4 Beta 4+)

$maxframes 允许追踪的框架最大数量

$status 抓取的http的状态

$temp_dir 网页服务器能够写入的临时文件目录 (/tmp)

$curl_path cURL binary 的目录, 如果没有cURL binary就设置为 false

以下是用法示例：
<code>include "Snoopy.class.php";
$snoopy = new Snoopy;
$snoopy-&gt;proxy_host = "<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>";
$snoopy-&gt;proxy_port = "80";
$snoopy-&gt;agent = "(compatible; MSIE 4.01; MSN 2.5; AOL 4.0; Windows 98)";
$snoopy-&gt;referer = "<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>";
$snoopy-&gt;cookies["SessionID"] = 238472834723489l;
$snoopy-&gt;cookies["favoriteColor"] = "RED";
$snoopy-&gt;rawheaders["Pragma"] = "no-cache";     $snoopy-&gt;maxredirs = 2;
$snoopy-&gt;offsiteok = false;
$snoopy-&gt;expandlinks = false;
$snoopy-&gt;user = "joe";
$snoopy-&gt;pass = "bloe";
if($snoopy-&gt;fetchtext("<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>")){
echo "&lt;PRE&gt;".htmlspecialchars($snoopy-&gt;results)."&lt;/PRE&gt;\n";
}else echo "error fetching document: ".$snoopy-&gt;error."\n";
</code>

以下是一些代码片段：

1、获取指定url内容
<code>&lt;?
$url = "<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>";
include("snoopy.php");
$snoopy = new Snoopy;
$snoopy-&gt;fetch($url); //获取所有内容
echo $snoopy-&gt;results; //显示结果
//可选以下
$snoopy-&gt;fetchtext //获取文本内容（去掉html代码）
$snoopy-&gt;fetchlinks //获取链接
$snoopy-&gt;fetchform  //获取表单
?&gt;
</code>
2、表单提交
<code>&lt;?php
$formvars["username"] = "admin";
$formvars["pwd"] = "admin";
$action = "<a href="http://www.ipodmp.com&quot;;//">http://www.ipodmp.com";//</a>表单提交地址
$snoopy-&gt;submit($action,$formvars);//$formvars为提交的数组
echo $snoopy-&gt;results; //获取表单提交后的 返回的结果
//可选以下
$snoopy-&gt;submittext; //提交后只返回 去除html的 文本
$snoopy-&gt;submitlinks;//提交后只返回 链接
?&gt;
</code>
既然已经提交的表单 那就可以做很多事情 接下来我们来伪装ip,伪装浏览器

3、伪装
<code>&lt;?php
$formvars["username"] = "admin";
$formvars["pwd"] = "admin";
$action = "<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>";
include "snoopy.php";
$snoopy = new Snoopy;
$snoopy-&gt;cookies["PHPSESSID"] = 'fc106b1918bd522cc863f36890e6fff7'; //伪装sessionid
$snoopy-&gt;agent = "(compatible; MSIE 4.01; MSN 2.5; AOL 4.0; Windows 98)"; //伪装浏览器
$snoopy-&gt;referer = <a href="http://www.ipodmp.com">http://www.ipodmp.com</a>; //伪装来源页地址 http_referer
$snoopy-&gt;rawheaders["Pragma"] = "no-cache"; //cache 的http头信息
$snoopy-&gt;rawheaders["X_FORWARDED_FOR"] = "127.0.0.101"; //伪装ip
$snoopy-&gt;submit($action,$formvars);
echo $snoopy-&gt;results;
?&gt;
</code>
原来我们可以伪装session 伪装浏览器 ，伪装ip， haha 可以做很多事情了。 例如 带验证码，验证ip 投票， 可以不停的投。 ps:这里伪装ip ，其实是伪装http头, 所以一般的通过 REMOTE_ADDR 获取的ip是伪装不了， 反而那些通过http头来获取ip的(可以防止代理的那种) 就可以自己来制造ip。 关于如何验证码 ，简单说下： 首先用普通的浏览器， 查看页面 ， 找到验证码所对应的sessionid， 同时记下sessionid和验证码值， 接下来就用snoopy去伪造 。 原理:由于是同一个sessionid 所以取得的验证码和第一次输入的是一样的。

4、有时我们可能需要伪造更多的东西,snoopy完全为我们想到了
<code>&lt;?php
$snoopy-&gt;proxy_host = "<a href="http://www.ipodmp.com">http://www.ipodmp.com</a>";
$snoopy-&gt;proxy_port = "8080"; //使用代理
$snoopy-&gt;maxredirs = 2; //重定向次数
$snoopy-&gt;expandlinks = true; //是否补全链接 在采集的时候经常用到
$snoopy-&gt;maxframes = 5 //允许的最大框架数
//注意抓取框架的时候 $snoopy-&gt;results 返回的是一个数组
$snoopy-&gt;error //返回报错信息
?&gt;
</code>

参考链接：<a href="http://www.6a8a.com/2011/PHP_0626/2615.html">推荐一个很好的PHP采集类：snoopy</a>