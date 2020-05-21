---
ID: 2092
post_title: 'PHP-噢~&#038;#65279(BOM文件头)你这个坏蛋'
author: ChinaBUG
post_excerpt: |
  自从使用了PHP来做ShopEX的二次开发以来，经常性的会出现一些莫名其妙的问题，比如，老是莫名其妙的多出一行对吧，怎么情况？！
  在火狐里面我们能看到多出来的哪行编码是&#65279，你查一下就知道是什么鸟东西了噢。多的不说，嗯，从网上下载了一段代码，运行，顿时轻松许多。
  这边就把我用的代码贴出来，作为备份吧。少许修改了一点点哈~~~
  使用说明：不带参数则处理当前目录下的全部文件，带参数自然就是处理参数指定的文件夹了。
  参数列表：dir=目录，你可以bom.php?dir=abc或者bom.php?dir=abc\cde均可
  感谢天，感谢地，感谢作者吧~
layout: post
permalink: >
  http://blog.ipodmp.com/2012/06/php-oh-bom-header-you-scoundrel.html
published: true
post_date: 2012-06-05 17:11:37
---
自从使用了PHP来做ShopEX的二次开发以来，经常性的会出现一些莫名其妙的问题，比如，老是莫名其妙的多出一行对吧，怎么情况？！

在火狐里面我们能看到多出来的哪行编码是&amp;#65279，你查一下就知道是什么鸟东西了噢。多的不说，嗯，从网上下载了一段代码，运行，顿时轻松许多。

这边就把我用的代码贴出来，作为备份吧。少许修改了一点点哈~~~

-- CODE --

使用说明：不带参数则处理当前目录下的全部文件，带参数自然就是处理参数指定的文件夹了。

参数列表：dir=目录，你可以bom.php?dir=abc或者bom.php?dir=abc\cde均可

感谢天，感谢地，感谢作者吧~

&lt;?php
header("Content-type: text/html; charset=utf-8");

//bom.php?dir=html
if(isset($_GET['dir'])){ //设置文件目录
$basedir=$_GET['dir'];
}else{
$basedir = '.';
}
$auto = 1;
checkdir($basedir);
function checkdir($basedir){
if($dh = opendir($basedir)){
while (($file = readdir($dh)) !== false) {
if ($file != '.' &amp;&amp; $file != '..'){
if (!is_dir($basedir."/".$file)) {
echo "文件: $basedir/$file ".checkBOM("$basedir/$file")." &lt;br&gt;";
}else{
$dirname = $basedir."/".$file;
checkdir($dirname);
}
}
}
closedir($dh);
}
}
function checkBOM ($filename) {
global $auto;
$contents = file_get_contents($filename);
$charset[1] = substr($contents, 0, 1);
$charset[2] = substr($contents, 1, 1);
$charset[3] = substr($contents, 2, 1);
if (ord($charset[1]) == 239 &amp;&amp; ord($charset[2]) == 187 &amp;&amp; ord($charset[3]) == 191) {
if ($auto == 1) {
$rest = substr($contents, 3);
rewrite ($filename, $rest);
return ("&lt;font color=red&gt;发现BOM,自动移除.&lt;/font&gt;");
}else{
return ("&lt;font color=red&gt;发现BOM.&lt;/font&gt;");
}
}else return ("没有发现BOM.");
}
function rewrite ($filename, $data) {
$filenum = fopen($filename, "w");
flock($filenum, LOCK_EX);
fwrite($filenum, $data);
fclose($filenum);
}
?&gt;