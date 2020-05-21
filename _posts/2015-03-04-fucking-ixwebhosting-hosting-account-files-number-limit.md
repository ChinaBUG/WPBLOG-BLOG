---
ID: 3946
post_title: >
  操蛋的ixwebhosting主机有账户文件数限制
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/fucking-ixwebhosting-hosting-account-files-number-limit.html
published: true
post_date: 2015-03-04 11:11:48
---
早就在网上看到ixwebhosting主机有文件数限制，30万个文件，当时理解是一个网站的文件，结果上星期遇上了，账户被锁死了，没有操作权限了，问了客服说是网站的文件数满了，一问才知道是账户内全部的文件数！靠~这怎么做多站呢？！

咆哮是没啥用的，贪便宜的下场往往就是如此的噢，敬告其他朋友要小心呀^_^

其实，文件数限制一点都不操蛋，操蛋的是居然没有提供工具查询文件数，这让我怎么删除文件呢~

好吧，DIY比较实际，网上找了一段代码，修改一下，就可以查询了噢。

下面是原来的方法：


[php]

/* 函数 listDirTree( $dirName = null )
** 功能 列出目录下所有文件及子目录
** 参数 $dirName 目录名称
** 返回 目录结构数组 false为失败
*/
function listDirTree( $dirName = null ){
if( empty( $dirName ) ) exit( &quot;IBFileSystem: directory is empty.&quot; );
if( is_dir( $dirName ) ){
if( $dh = opendir( $dirName ) ){
$tree = array();
while( ( $file = readdir( $dh ) ) !== false ){
if( $file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot; ){
$filePath = $dirName . &quot;/&quot; . $file;
if( is_dir( $filePath ) ){ //为目录,递归
$tree[$file] = listDirTree( $filePath );
}else{ //为文件,添加到当前数组
$tree[] = $file;
}
}
}
closedir( $dh );
}else{
exit( &quot;IBFileSystem: can not open directory $dirName.&quot;);
}
//返回当前的$tree
return $tree;
}else{
exit( &quot;IBFileSystem: $dirName is not a directory.&quot;);
}
}[/php]


方法摘自《<a href="http://www.jb51.net/article/31490.htm">php列出一个目录下的所有文件的代码</a>》

做了一点修改，适应需求噢


[php]/* 函数 listDirTree( $dirName = null )
** 功能 列出目录下所有文件及子目录
** 参数 $dirName 目录名称
** 返回 目录结构数组 false为失败
*/
function listDirTree( $dirName = null ){
global $allcount;
if( empty( $dirName ) ) exit( &quot;IBFileSystem: directory is empty.&quot; );
if( is_dir( $dirName ) ){
if( $dh = opendir( $dirName ) ){
$tree = array();
while( ( $file = readdir( $dh ) ) !== false ){
if( $file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot; ){
$filePath = $dirName . &quot;/&quot; . $file;
if( is_dir( $filePath ) ){ //为目录,递归
$tree[$file] = listDirTree( $filePath );
}else{ //为文件,添加到当前数组
//$tree[] = $file;
$tree[] = $filePath;
$allcount += 1;
}
}
}
closedir( $dh );
}else{
exit( &quot;IBFileSystem: can not open directory $dirName.&quot;);
}
//返回当前的$tree
return $tree;
}else{
exit( &quot;IBFileSystem: $dirName is not a directory.&quot;);
}
}[/php]


然后，就可以调用了噢


[php]
$dir = &quot;../&quot;;
if( isset($_GET['dir'])&amp;&amp;!empty($_GET['dir']) ) $dir = $_GET['dir'];

if( $dh = opendir( $dir ) ){
while( ( $file = readdir( $dh ) ) !== false ){
$allcount = 0;
if( $file != &quot;.&quot; &amp;&amp; $file != &quot;..&quot; ){
$filePath = $dir . &quot;/&quot; . $file;
if( is_dir( $filePath ) ){ //为目录,递归
//$tree[$file] = listDirTree( $filePath );
echo &quot;&lt;a href=\&quot;dir.php?dir={$filePath}\&quot;&gt;{$file}&lt;/a&gt;&lt;br/&gt;\n&quot;;
listDirTree( $filePath );
echo $allcount.&quot;&lt;br/&gt;\n&quot;;
echo '-------'.&quot;&lt;br/&gt;\n&quot;;;
}
}
}
closedir( $dh );
}else{
exit( &quot;IBFileSystem: can not open directory $dir.&quot;);
}[/php]


文件放在任何一个站内，执行之后就会显示账户下全部的网站及相应的文件数。

现在就可以根据自己的需要找到文件数占用较多的网站检查一下，删除呗。

噢~文件我放在 <a href="http://files1.91zhaota.com/php/201503/php_dir_count_files.rar">这里</a>，点击下载呗。