---
ID: 1418
post_title: >
  PHP-通过自动获取IP地址判断所在省份地区(淘宝IP库腾讯IP库新浪IP库纯真版IP库)
author: ChinaBUG
post_excerpt: |
  虫神曰：
  这边收集两篇，个人觉得比较有代表性的，推荐大家阅读。
  虫神提醒：
  解决办法1，请不要放在你的服务器上，放了，得出的地址只是你服务器所在的地区，为什么噢？因为执行的获取的IP是服务器端，不是你这个客户端。
  解决办法2，目前代码都有些问题（已提供修改建议），有能力的请自己修改，反正虫神笨，没办法修改哈，你有修改能运行请发一份给我哈。
  解决办法3，是我自己实在没办法下自己编的，一个字偷！这个麻烦的情况是，用的数据是别人的，要记得时常更新。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/php-auto-determine-the-ip-address-areas-where-the-provinces.html
published: true
post_date: 2011-06-26 01:18:55
---
虫神曰：

这边收集两篇，个人觉得比较有代表性的，推荐大家阅读。

虫神提醒：

解决办法1，请不要放在你的服务器上，放了，得出的地址只是你服务器所在的地区，为什么噢？因为执行的获取的IP是服务器端，不是你这个客户端。

解决办法2，目前代码都有些问题（已提供修改建议），有能力的请自己修改，反正虫神笨，没办法修改哈，你有修改能运行请发一份给我哈。

解决办法3，是我自己实在没办法下自己编的，一个字偷！这个麻烦的情况是，用的数据是别人的，要记得时常更新。

虫神服务：

虫子测试的代码打包下载了 <a href="http://u.ipodmp.com/viewfile.php?file_id=250&amp;file_key=Nx5Bdb9A" target="_blank">请点击下载</a>

~~~~~~~~~

解决方法1：

原文地址：<a title="Permanent Link: 网页中通过ip地址判断所在地区" href="http://fly2008.w10.haohaohost.cn/?p=13">网页中通过ip地址判断所在地区</a>

如果我们想通过ip地址来判断用户所在地，这需要我们预先知道ip地址与现实中城市的对应。然后分析用户ip从而在数据库中提取相对应的城市。不幸的是ip与城市对应这项数据我们手中是没有的，万幸的是我们可以借助其他网站的数据。

php有一个方法实现，需要在<a href="http://fw.qq.com/ipaddress">http://fw.qq.com/ipaddress</a>中获取数据。这是腾讯的一个网页在其网页源代码中仅有var IPData = new Array("112.226.232.66","","山东省","青岛市");这么一段字符串。这个是我的电脑ip地址和所在的信息。我们可以通过file_get_content()函数来获取这一条信息到本地。然后通过对字符串的分析提取出ip和所在地。代码如下：

&lt;?php
$ip=file_get_contents("<a href="http://fw.qq.com/ipaddress&quot;);//">http://fw.qq.com/ipaddress");//</a>获取字符串
$ip=str_replace("\"","",$ip);                                       //去掉"
$ip2=explode(‘(‘,$ip);                                           //将字符串在(处分成一个有2个元素的数组
$a=substr($ip2[1],0,-2);                                      //截取掉后面的);
$b=explode(‘,’,$a);                                              //将最后剩下的字符串处理成数组
echo "ip地址为".$b[0];                                         //输出ip
echo "所在地区为".$b[2].$b[3];                            // 输出所在地
?&gt;

还可以用js实现：

利用 JavaScript 调用。

&lt;script type="text/javascript" src="<a href="http://fw.qq.com/ipaddress">http://fw.qq.com/ipaddress"&gt;&lt;/script</a>&gt;
&lt;script type="text/javascript"&gt;
&lt;!–
alert("您的IP：" + IPData[0] + "\r\n" +  "所属省：" + IPData[2] + "\r\n" +    "所属市：" + IPData[3]);
//–&gt;
&lt;/script&gt;

重要说明

接口地址 <a href="http://fw.qq.com/ipaddress">http://fw.qq.com/ipaddress</a> 最后面没有斜杠。

技术分析

直接访问 <a href="http://fw.qq.com/ipaddress">http://fw.qq.com/ipaddress</a>，可以看到类似如下的代码：

var IPData = new Array("1.2.3.4","","重庆市","");
我们就是使用这个 IPData 数组。

2012.01.10补充：

新浪IP地址接口地址获取IP地址

新浪的接口 ：

http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js

多地域测试方法：

http://int.dpool.sina.com.cn/iplookup/iplookup.php?format=js&amp;ip=218.192.3.42

返回值：

var remote_ip_info = {"ret":1,"start":"218.192.0.0","end":"218.192.7.255","country":"\u4e2d\u56fd","province":"\u5e7f\u4e1c","city":"\u5e7f\u5dde","district":"","isp":"\u6559\u80b2\u7f51","type":"\u5b66\u6821","desc":"\u5e7f\u5dde\u5927\u5b66\u7eba\u7ec7\u670d\u88c5\u5b66\u9662"};

与腾讯的同样，直接调用，那个JS的编码也不需要转码噢~

2013.01.03补充

使用淘宝IP地址库获取，官方地址：<a href="http://ip.taobao.com/index.php">http://ip.taobao.com/index.php</a>

调用说明：

1. 请求接口（GET）：

<a href="http://ip.taobao.com/service/getIpInfo.php?ip=[ip">http://ip.taobao.com/service/getIpInfo.php?ip=[ip</a>地址字串]

2. 响应信息：

（json格式的）国家 、省（自治区或直辖市）、市（县）、运营商

3. 返回数据格式：

{"code":0,"data":{"ip":"210.75.225.254","country":"\u4e2d\u56fd","area":"\u534e\u5317", "region":"\u5317\u4eac\u5e02","city":"\u5317\u4eac\u5e02","county":"","isp":"\u7535\u4fe1", "country_id":"86","area_id":"100000","region_id":"110000","city_id":"110000", "county_id":"-1","isp_id":"100017"}}

其中code的值的含义为，0：成功，1：失败。

具体使用案例请参照：<a href="http://www.phpchina.com/archives/view-42424-1.html">PHP获取用户真实IP</a>

~~~

解决方法2：

原文地址：<a href="http://www.uuhar.com/program/php/shili/9.html" target="_blank">PHP获取IP地址实现不同地区城市跳转的功能</a>

前言：

经我的测试，PHP获取IP地址实现不同地区城市跳转的功能 该文中所写代码有误，在执行时会提醒错误（解决方法在下面），很无语的一个问题，另外在网上找到的其他版本的，凡是涉及 <span style="text-decoration: underline;">最新腾讯QQ IP数据库 2011.04.15 纯真版</span> 这个IP库的代码均运行不起来。最起码我找到的几个代码全部是提示错误的。

若你想要使用这办法的话，还请自己根据情况调整代码，如果有修改，请发一份给我吧，谢谢。

错误修改：

如果你非要使用这篇文章里面写的代码，那么请在复制代码之前使用 error_reporting(E_ALL &amp; ~E_NOTICE); 语句，那么你的代码就可以运行成功啦！！

具体原因，就是变量未定义造成错误提示，我们只要忽略掉就可以了。详细信息请看本博客内的 <a title="PHP-error_reporting(E_ALL ^ E_NOTICE)与error_reporting(0);" href="http://blog.ipodmp.com/archives/php-error_reportinge_all-e_notice-error_reporting/" target="_blank">PHP-error_reporting(E_ALL ^ E_NOTICE)与error_reporting(0);</a>

在下载的打包里面已经修正这个问题，你可以直接使用。

原文：

PHP获取IP地址

这个比较简单了，利用PHP自带函数就可以了，PHP中文手册看一下，都有现成的例子，就不过多说明了，直接上代码，A段：
<ul>
	<li>//PHP获取当前用户IP地址方法</li>
	<li>$xp_UserIp = ($_SERVER["HTTP_VIA"]) ? $_SERVER["HTTP_X_FORWARDED_FOR"] : $_SERVER["REMOTE_ADDR"];</li>
	<li>$xp_UserIp = ($xp_UserIp) ? $xp_UserIp : $_SERVER["REMOTE_ADDR"];</li>
</ul>
PHP通过IP地址判断用户所在城市

上文已经获得了用户IP地址，接下来，我们就是根据这个IP地址获得用户所在城市了。开始之前，我们需要下载一个现成的数据库QQ IP数据库。
<div>　　附：<a href="http://www.crsky.com/soft/2611.html">最新腾讯QQ IP数据库 2011.04.15 纯真版下载</a></div>
<div>　　使用方法：解压后QQWry.Dat就是我们想要IP地址数据库，我们新建一个ipcity文件夹，将数据库放在下面。QQ IP数据库使用非常方便，数据也很齐全，你可以及时关注官方更新以保持数据最新，强力推荐一下：）</div>
<div>　　接下来，我们在上面的ipcity目录下新建一个ipaddress.php文件，直接复制以下代码进去即可，重要的地方也作了相应注释。B段：</div>
<blockquote>
<ul>
	<li>&lt;?</li>
	<li>/*</li>
	<li>函数名称：ipCity</li>
	<li>参数说明：$userip——用户IP地址</li>
	<li>函数功能：PHP通过IP地址判断用户所在城市</li>
	<li>author:lee</li>
	<li>contact:xpsem2010@gmail.com</li>
	<li>*/</li>
	<li>function ipCity($userip) {</li>
	<li>    //IP数据库路径，这里用的是QQ IP数据库 20110405 纯真版</li>
	<li>    $dat_path = 'QQWry.dat';</li>
	<li>    //判断IP地址是否有效</li>
	<li>    if(!ereg("^([0-9]{1,3}.){3}[0-9]{1,3}$", $userip)){</li>
	<li>        return 'IP Address Invalid';</li>
	<li>    }</li>
	<li>    //打开IP数据库</li>
	<li>    if(!$fd = @fopen($dat_path, 'rb')){</li>
	<li>        return 'IP data file not exists or access denied';</li>
	<li>    }</li>
	<li>    //explode函数分解IP地址，运算得出整数形结果</li>
	<li>    $userip = explode('.', $userip);</li>
	<li>    $useripNum = $userip[0] * 16777216 + $userip[1] * 65536 + $userip[2] * 256 + $userip[3];</li>
	<li>    //获取IP地址索引开始和结束位置</li>
	<li>    $DataBegin = fread($fd, 4);</li>
	<li>    $DataEnd = fread($fd, 4);</li>
	<li>    $useripbegin = implode('', unpack('L', $DataBegin));</li>
	<li>    if($useripbegin &lt; 0) $useripbegin += pow(2, 32);</li>
	<li>    $useripend = implode('', unpack('L', $DataEnd));</li>
	<li>    if($useripend &lt; 0) $useripend += pow(2, 32);</li>
	<li>    $useripAllNum = ($useripend - $useripbegin) / 7 + 1;</li>
	<li>    $BeginNum = 0;</li>
	<li>    $EndNum = $useripAllNum;</li>
	<li>     //使用二分查找法从索引记录中搜索匹配的IP地址记录</li>
	<li>    while($userip1num&gt;$useripNum || $userip2num&lt;$useripNum) {</li>
	<li>        $Middle= intval(($EndNum + $BeginNum) / 2);</li>
	<li>        //偏移指针到索引位置读取4个字节</li>
	<li>        fseek($fd, $useripbegin + 7 * $Middle);</li>
	<li>        $useripData1 = fread($fd, 4);</li>
	<li>        if(strlen($useripData1) &lt; 4) {</li>
	<li>            fclose($fd);</li>
	<li>            return 'File Error';</li>
	<li>        }</li>
	<li>        //提取出来的数据转换成长整形，如果数据是负数则加上2的32次幂</li>
	<li>        $userip1num = implode('', unpack('L', $useripData1));</li>
	<li>        if($userip1num &lt; 0) $userip1num += pow(2, 32);</li>
	<li>         //提取的长整型数大于我们IP地址则修改结束位置进行下一次循环</li>
	<li>        if($userip1num &gt; $useripNum) {</li>
	<li>            $EndNum = $Middle;</li>
	<li>            continue;</li>
	<li>        }</li>
	<li>        //取完上一个索引后取下一个索引</li>
	<li>        $DataSeek = fread($fd, 3);</li>
	<li>        if(strlen($DataSeek) &lt; 3) {</li>
	<li>            fclose($fd);</li>
	<li>            return 'File Error';</li>
	<li>        }</li>
	<li><span style="color: #ff0000;">//// ---- 请注意，这边的代码我的WP没办法保存成功地~~请看原文哈</span></li>
	<li><span style="color: #ff0000;">////-----敬请注意哈---------------------------------------------------------</span></li>
	<li>    //返回IP地址对应的城市结果</li>
	<li>    if(preg_match('/http/i', $useripAddr2)) {</li>
	<li>        $useripAddr2 = '';</li>
	<li>    }</li>
	<li>    $useripaddr = "$useripAddr1 $useripAddr2";</li>
	<li>    $useripaddr = preg_replace('/CZ88.Net/is', '', $useripaddr);</li>
	<li>    $useripaddr = preg_replace('/^s*/is', '', $useripaddr);</li>
	<li>    $useripaddr = preg_replace('/s*$/is', '', $useripaddr);</li>
	<li>    if(preg_match('/http/i', $useripaddr) || $useripaddr == '') {</li>
	<li>        $useripaddr = 'No Data';</li>
	<li>    }</li>
	<li>    return $useripaddr;</li>
	<li>}</li>
	<li>?&gt;</li>
</ul>
</blockquote>
PHP根据IP地址实现城市切换或跳转
<div>到这里，其实问题已经很简单了，用简单的js就通通搞定。C段如下：</div>
<blockquote>
<ul>
	<li>//根据IP地址跳转指定页面js取得城市</li>
	<li>var city='&lt;?echo ipCity($xp_UserIp);?&gt;';</li>
	<li>//根据IP地址所有城市跳转到指定页面</li>
	<li>if(city.indexOf("上海市")&gt;=0){</li>
	<li>        window.location.href="http://shanghai.demo.com/";</li>
	<li>}</li>
</ul>
</blockquote>
<div>　　将开头的A段代码和上面的C段代码分别放在B段代码的头和尾，然后我们在需要跳转的页面加入以下代码：</div>
<div>
<blockquote>
<ul>
	<li>&lt;script src="/ipcity/ipaddress.php" type="text/javascript" language="javascript"&gt;&lt;/script&gt;</li>
</ul>
</blockquote>
</div>
<h3>刷新页面，是不是达到预想的效果了呢？</h3>
<div>　　以上就是PHP获取IP地址、PHP根据IP地址判断城市以及PHP根据IP地址实现城市切换或跳转的详细介绍了，事实上，像PHP中通过IP地址自动切换城市就是这个方法的典型应用。</div>
~~~

解决方法3：

善于使用别人的东西才是王道呀~

function AutoGetArea(){
$url = "<a href="http://www.123cha.com/ip/?q">http://www.123cha.com/ip/?q</a>=" . $this-&gt;getIp();
$get_content = $this-&gt;safeEncoding( file_get_contents( $url ) );

$word_find_left =  "&lt;ul id=\"csstb\"&gt;";
$word_find_right =  "&lt;table cellspacing=0 cellpadding=8 width=530 border=0&gt;";

$pos_left = strrpos($get_content,$word_find_left);
$pos_right = strrpos($get_content,$word_find_right);
$ip2area = substr($get_content,$pos_left,$pos_right-$pos_left);
$ip2area = str_ireplace($word_find_left,"",$ip2area);
$ip2area = str_ireplace($word_find_right,"",$ip2area);
$ip2area = str_ireplace("(","",$ip2area);
$ip2area = str_ireplace(")","",$ip2area);
$ip2area = str_ireplace("（","",$ip2area);
$ip2area = str_ireplace("）","",$ip2area);
if(isset($ip2area)){
if( strstr($ip2area,"厦门")!=false ){
$area = 'xm';
}
if( strstr($ip2area,"晋江")!=false ){
$area = 'jj';
}
if( strstr($ip2area,"石狮")!=false ){
$area = 'ss';
}
}
if( !isset($area) ){
$area = 'jj';
}
setCookie(COOKIE_PFIX."[area]",$area);
echo $area;
}

/*
函数名称：getIp
参数说明：无
函数功能：自动获取客户端的IP
*/
function getIp() {
if (getenv("HTTP_CLIENT_IP") &amp;&amp; strcasecmp(getenv("HTTP_CLIENT_IP"), "unknown")) $ip = getenv("HTTP_CLIENT_IP");
else if (getenv("HTTP_X_FORWARDED_FOR") &amp;&amp; strcasecmp(getenv("HTTP_X_FORWARDED_FOR"), "unknown")) $ip = getenv("HTTP_X_FORWARDED_FOR");
else if (getenv("REMOTE_ADDR") &amp;&amp; strcasecmp(getenv("REMOTE_ADDR"), "unknown")) $ip = getenv("REMOTE_ADDR");
else if (isset($_SERVER['REMOTE_ADDR']) &amp;&amp; $_SERVER['REMOTE_ADDR'] &amp;&amp; strcasecmp($_SERVER['REMOTE_ADDR'], "unknown")) $ip = $_SERVER['REMOTE_ADDR'];
else $ip = "unknown";
$ip = str_replace(" ","",$ip);
//判断IP地址是否有效
if(!ereg("^([0-9]{1,3}.){3}[0-9]{1,3}$", $ip)){
return '127.0.0.1';  //IP Address Invalid
}
return ($ip);
}