---
ID: 2286
post_title: >
  PHP-Unicode签名(BOM)影响缓存输出实例
author: ChinaBUG
post_excerpt: |
  今天，二次开发ShopEX的可视化模板模块，可视化模板增加是成功了，可是，模板管理的功能却不正常了，那些模板的预览图都不见了。
  真是神奇的事情噢~
  我找呀找呀，代码一行行的调试过去，都没问题，那个汗~
  什么回事噢，从代码中看到预览图其实是直接调用一个脚本生成一个图片的资源。
  从地址栏里面输入开发前跟开发后的地址，发现生成的代码有少许的不一样噢。
  是什么造成的呢？
  查阅源代码可以知道，这个预览图其实就是调用文件并且缓存输出的。
  经过排查发现，我二次开发的文件有Unicode签名，俗称BOM的东西！去掉看看，噢噢噢噢，问题解决。靠~真是神奇。
  话说在ShopEX二次开发的过程中经常会出现这种情况的，比如验证码忽然不能使用了？！估计是同一个原因噢。
  话说，因为Unicode签名(BOM)造成的ShopEX二次开发的问题还有就是会莫名其妙多出一行的空白行！请参考PHP-噢~&#65279(BOM文件头)你这个坏蛋
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/php-unicode-signature-bom-affect-cache-output-example.html
published: true
post_date: 2013-01-23 10:59:50
---
今天，二次开发ShopEX的可视化模板模块，可视化模板增加是成功了，可是，模板管理的功能却不正常了，那些模板的预览图都不见了。

真是神奇的事情噢~

我找呀找呀，代码一行行的调试过去，都没问题，那个汗~

什么回事噢，从代码中看到预览图其实是直接调用一个脚本生成一个图片的资源。

从地址栏里面输入开发前跟开发后的地址，发现生成的代码有少许的不一样噢。

是什么造成的呢？

查阅源代码可以知道，这个预览图其实就是调用文件并且缓存输出的。

经过排查发现，我二次开发的文件有Unicode签名，俗称BOM的东西！去掉看看，噢噢噢噢，问题解决。靠~真是神奇。

话说在ShopEX二次开发的过程中经常会出现这种情况的，比如验证码忽然不能使用了？！估计是同一个原因噢。

话说，因为<a title="PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋" href="http://blog.ipodmp.com/archives/php-oh-bom-header-you-scoundrel/">Unicode签名(BOM)造成的ShopEX二次开发的问题</a>还有就是会莫名其妙多出一行的空白行！请参考<a title="PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋" href="http://blog.ipodmp.com/archives/php-oh-bom-header-you-scoundrel/">PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋</a>

附录：

1.core/kernel.php

function sfile($file,$file_bak=null,$head_redect=false){
if(!file_exists($file)){
$file = $file_bak;
}

$etag = md5_file($file);
header('Etag: '.$etag);

if(isset($_SERVER['HTTP_IF_NONE_MATCH']) &amp;&amp; $_SERVER['HTTP_IF_NONE_MATCH'] == $etag){
header('HTTP/1.1 304 Not Modified',true,304);
exit(0);
}else{
set_time_limit(0);
header("Expires: " .$expires. " GMT");
header("Cache-Control: public");
session_cache_limiter('public');
sendfile($file);
}
}

2.core/func_ext.php

function sendfile($file){
$handle = fopen($file, "r");
while($buffer = fread($handle,102400)){
echo $buffer;
flush();
}
fclose($handle);
}