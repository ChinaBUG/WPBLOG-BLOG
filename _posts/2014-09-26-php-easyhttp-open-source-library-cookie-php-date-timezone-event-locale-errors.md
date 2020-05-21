---
ID: 3643
post_title: >
  PHP-EasyHttp开源库中Cookie.php的date.timezone事件区域设置错误
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/09/php-easyhttp-open-source-library-cookie-php-date-timezone-event-locale-errors.html
published: true
post_date: 2014-09-26 22:21:38
---
今天，一朋友使用商派的云主机居然没有支持CURL功能，只好找了EasyHttp开源库，下面是官方的说明：
<blockquote><em>EasyHttp是一个帮助你忽略不同的php环境情况，无差别地发送http请求的php类。 你不再需要关注当前php环境是否支持curl/fsockopen/fopen，EasyHttp会自动选择一个最合适的方式去发出http请求。 EasyHttp源于WordPress中的WP_Http类，去除了所有对WordPress其他函数的依赖，将其拆分到不同的文件中，并做了少量简 化。</em></blockquote>
开源仓库地址：<a href="https://github.com/duoshuo/easy-http">https://github.com/duoshuo/easy-http</a>

使用的过程不知道怎么回事出现下面的错误：
<blockquote><em>Warning: strtotime(): It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected 'Asia/Chongqing' for 'CST/8.0/no DST' instead in /mnt/httpd/www/ecstore/advertiser/EasyHttp/Cookie.php on line 98</em></blockquote>
从提示中我们知道，需要我们做一下设置哈~怎么设置呢？

两种方法：
<ol>
	<li>date_default_timezone_set('Asia/Shanghai');
取值范围：http://php.net/manual/zh/timezones.asia.php</li>
	<li> INI 设置 date.timezone 来设置默认时区
设置php.ini，具体设置参考http://php.net/manual/zh/datetime.configuration.php</li>
</ol>
好吧，简单吧~
<blockquote><em><strong>本地下载：<a title="EasyHttp开源库下载" href="http://images1.91zhaota.com/20140926/php/20140926_cb_EasyHttp.rar" target="_blank">EasyHttp开源库下载</a></strong></em></blockquote>
附录：

POST调用：

$http = new EasyHttp();
$response = $http-&gt;request(
'http://****.demo.91zhaoni.com/api.php',
array(
'method' =&gt; 'POST',
'body' =&gt; Array(
'act' =&gt; 'search_goods_list',
'api_version' =&gt; '1.0',
'counts' =&gt; '5',
'last_modify_en_time' =&gt; '1417363200',
'last_modify_st_time' =&gt; '1230739200',
'ac' =&gt; '50567c864125f9be076f06e2b14e6b71'
)
)
);
print_r($response['body']);

怎么检测服务器是否支持curl功能？

很简单只要新建一个PHP文件，然后复制下面的代码即可。

&lt;?php
if(function_exists('curl_init')){
exit("支持");
}else{
exit("不支持");
}
?&gt;

执行之后就会告诉你服务器是支持curl还是不支持了。