---
ID: 3367
post_title: 'Theme Photolux : A TimThumb error has occured'
author: ChinaBUG
post_excerpt: |
  漂亮吧，不过最近安装在phpcloud的免费空间上却是让我很烦恼，这个主题安装上去却显示不正确表现为前台显示图片都是一个叉叉，图片显示不正常，老是显示下面的错误？！
  A TimThumb error has occured
  The following error(s) occured:
  Could not find the internal image you specified.
  Query String : src=http://cmsdemo2014.my.phpcloud.com/wp-content/uploads/2014/03/dd6463e4958eaaa84b44f199333d0f8d.jpg&h=&w=290&zc=1&q=100&a=c
  TimThumb version : 2.8.11
  好神奇！
  :-P 知识点延伸：
  可能你会觉得奇怪为什么你只看到图片显示不正常，没有看到这个错误哈 :-P ，这个其实是正常的，因为这个错误是我直接访问图片文件链接而产生的错误。
  具体操作如下：
  我们知道图片显示是根据<img>标签的src属性来显示图片的，那么图片显示不出来就是src这个图片源出现问题（图片源可以是一个图片文件的路径也可以是图片的代码噢），既然这个src是图片源。那么自然就能直接打开，所以我的打叉的图片的src是：
  http://cmsdemo2014.my.phpcloud.com/wp-content/themes/photolux/lib/utils/timthumb.php?src=http://cmsdemo2014.my.phpcloud.com/wp-content/uploads/2014/03/安卓壁纸_动漫66c143d81e.jpg&h=&w=290&zc=1&q=100&a=c
  我直接从浏览器中打开，就会出现上面的提示了。另外，有很多朋友经常性的会遇上系统的验证码显示不正常的问题，然后不知道怎么解决，其实跟这个原理是一样的，先直接打开链接看看是否正常，然后再进入相关的文件内去找有问题的地方，然后不断的重试，直到图片正常显示出来，那么再调用的地方一定就是正常的了。
  如果是ShopEX/ShopNC商城系统出现验证码的问题那么请查阅本博客的另外相关的文章：
  ShopEX-登陆时验证码显示错误造成不能登陆
  PHP-Unicode签名(BOM)影响缓存输出实例
  PHP-噢~&#65279(BOM文件头)你这个坏蛋
  言归正传，既然知道是这个文件显示错误的话，那么当然就是直接找到这个文件来查看为什么出现这种错误了。
  找到wp-content\theme\photolux\lib\utils\timthumb.php文件，然后一步步的跟踪下去，我发现，程序不工作是因为找不到正确的文件路径造成的，知道原因了那就直接修改一下即可（轻描淡写的说出这个原因，实际背后是我一个个代码隐藏，一个个试过去才知道造成问题的所在噢）：
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/theme-photolux-a-timthumb-error-has-occured.html
published: true
post_date: 2014-04-02 10:20:28
---
最近在使用wordpress主题Photolux这个摄像图片主题，效果很不错，先来看看整体的介绍哈。

资料摘自：<a href="http://www.hujuntao.com/other/wordpress-photography-image-theme-photolux.html">wordpress摄影/图片主题-photolux V2.0.0</a>

photolux是一个强大的、优雅的组合和摄影WordPress主题。最适合摄影师和创意网站，可以使用组合，以展示他们的作品。photolux主题提供的选项管理可修改任何方面-这非常适合初学者与没有编码知识和开发能力者。主题带有三个基本的皮肤选择：黑、灰和透明，以及众多的后台简单定制和许多皮肤选项。
<ol>
	<li>主题包含多种图片展示方式，几乎包括了你所知道的所有图片排版格式</li>
	<li>主题可以创建多个图片集，使用不同风格，用于不同目的</li>
	<li>你可以自定义页面背景或者全屏幻灯片</li>
	<li>不受限制的侧边栏，你可以在每页使用不同的侧边栏</li>
	<li>内置SEO优化选项，无需安装其它SEO插件</li>
	<li>多层次的回复功能</li>
	<li>多级下拉菜单</li>
	<li>兼容浏览器IE7, IE8, IE9, Firefox 2,Firefox 3, Firefox 4, Safari 4, Safari 5, Opera, Chrome</li>
</ol>
我比较喜欢网格效果的布局，很不错，很棒！（当然，不排除样例里面的美女效应哈~）如下图：

<img title="wordpress摄影/图片主题-photolux" alt="wordpress摄影/图片主题-photolux" src="http://www.4mudi.com/wp-content/uploads/2012/05/2_home.jpg" width="650" data-original="http://www.4mudi.com/wp-content/uploads/2012/05/2_home.jpg" />
<img title="wordpress摄影/图片主题-photolux" alt="wordpress摄影/图片主题-photolux" src="http://www.4mudi.com/wp-content/uploads/2012/05/3_home.jpg" width="650" data-original="http://www.4mudi.com/wp-content/uploads/2012/05/3_home.jpg" />
<img title="wordpress摄影/图片主题-photolux" alt="wordpress摄影/图片主题-photolux" src="http://www.4mudi.com/wp-content/uploads/2012/05/4_home.jpg" width="650" data-original="http://www.4mudi.com/wp-content/uploads/2012/05/4_home.jpg" />
<img title="wordpress摄影/图片主题-photolux" alt="wordpress摄影/图片主题-photolux" src="http://www.4mudi.com/wp-content/uploads/2012/05/5_home.jpg" width="650" data-original="http://www.4mudi.com/wp-content/uploads/2012/05/5_home.jpg" />

漂亮吧，不过最近安装在<a href="http://www.phpcloud.com/">phpcloud</a>的免费空间上却是让我很烦恼，这个主题安装上去却显示不正确表现为前台显示图片都是一个叉叉，图片显示不正常，老是显示下面的错误？！

<hr />

<h1>A TimThumb error has occured</h1>
The following error(s) occured:
<ul>
	<li>Could not find the internal image you specified.</li>
</ul>
Query String : src=http://cmsdemo2014.my.phpcloud.com/wp-content/uploads/2014/03/dd6463e4958eaaa84b44f199333d0f8d.jpg&amp;h=&amp;w=290&amp;zc=1&amp;q=100&amp;a=c
TimThumb version : 2.8.11

好神奇！
<blockquote><em> :-P 知识点延伸：
可能你会觉得奇怪为什么你只看到图片显示不正常，没有看到这个错误哈 :-P ，这个其实是正常的，因为这个错误是我直接访问图片文件链接而产生的错误。
具体操作如下：
我们知道图片显示是根据&lt;img&gt;标签的src属性来显示图片的，那么图片显示不出来就是src这个图片源出现问题（图片源可以是一个图片文件的路径也可以是图片的代码噢），既然这个src是图片源。那么自然就能直接打开，所以我的打叉的图片的src是：
http://cmsdemo2014.my.phpcloud.com/wp-content/themes/photolux/lib/utils/timthumb.php?src=http://cmsdemo2014.my.phpcloud.com/wp-content/uploads/2014/03/安卓壁纸_动漫66c143d81e.jpg&amp;h=&amp;w=290&amp;zc=1&amp;q=100&amp;a=c
我直接从浏览器中打开，就会出现上面的提示了。另外，有很多朋友经常性的会遇上系统的验证码显示不正常的问题，然后不知道怎么解决，其实跟这个原理是一样的，先直接打开链接看看是否正常，然后再进入相关的文件内去找有问题的地方，然后不断的重试，直到图片正常显示出来，那么再调用的地方一定就是正常的了。
如果是ShopEX/ShopNC商城系统出现验证码的问题那么请查阅本博客的另外相关的文章：
<a title="PHP-Unicode签名(BOM)影响缓存输出实例" href="http://blog.ipodmp.com/archives/php-unicode-signature-bom-affect-cache-output-example/">ShopEX-登陆时验证码显示错误造成不能登陆
PHP-Unicode签名(BOM)影响缓存输出实例
</a><a title="PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋" href="http://blog.ipodmp.com/archives/php-oh-bom-header-you-scoundrel/">PHP-噢~&amp;#65279(BOM文件头)你这个坏蛋</a></em></blockquote>
言归正传，既然知道是这个文件显示错误的话，那么当然就是直接找到这个文件来查看为什么出现这种错误了。

找到wp-content\theme\photolux\lib\utils\timthumb.php文件，然后一步步的跟踪下去，我发现，程序不工作是因为找不到正确的文件路径造成的，知道原因了那就直接修改一下即可（轻描淡写的说出这个原因，实际背后是我一个个代码隐藏，一个个试过去才知道造成问题的所在噢）：

[php]
//找到下面代码所在，将$base的值直接改为虚拟主机的真实地址！
//记住：其他主机可能没有这么麻烦，但是phpcloud的路径就有点特殊了
//当然，你也可以重新根据执行脚本的路径处理一下获取这个地址，更加智能一下，省的每次都要修改这个哈~
foreach ($sub_directories as $sub){
    //$base .= $sub . '/';
    $base = '/home/cmsdemo2014/.apps/http/__default__/0/3.6-zdc/';
    //或者使用下面的
    $base = str_ireplace('wp-content/themes/photolux/lib/utils/timthumb.php','',$_SERVER['SCRIPT_FILENAME']);
    //
    $this-&gt;debug(3, &quot;Trying file as: &quot; . $base . $src);
    if(file_exists($base . $src)){
    return $base . $src;
    //......
[/php]