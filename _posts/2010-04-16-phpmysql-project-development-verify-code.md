---
ID: 476
post_title: PHP+MySQL懒人项目开发-验证码
author: ChinaBUG
post_excerpt: >
  　　在项目开发中，验证码的应用是越来越普遍，恩，下面是我找到的，测试过能用的代码噢，提供给有需要的人用，代码不在于多，在于能用噢~~
layout: post
permalink: >
  http://blog.ipodmp.com/2010/04/phpmysql-project-development-verify-code.html
published: true
post_date: 2010-04-16 20:15:14
---
　　在项目开发中，验证码的应用是越来越普遍，恩，下面是我找到的，测试过能用的代码噢，提供给有需要的人用，代码不在于多，在于能用噢~~

　　我取名为yz.php,代码如下：

&lt;?php
session_start();
//生成验证码图片
Header("Content-type: image/PNG");
$im = imagecreate(44,18);
$back = ImageColorAllocate($im, 245,245,245);
imagefill($im,0,0,$back); //背景
srand((double)microtime()*1000000);
//生成4位数字
for($i=0;$i&lt;4;$i++){
$font = ImageColorAllocate($im, rand(100,255),rand(0,100),rand(100,255));
$authnum=rand(1,9);
$vcodes.=$authnum;
imagestring($im, 5, 2+$i*10, 1, $authnum, $font);
}
for($i=0;$i&lt;100;$i++) //加入干扰象素
{
$randcolor = ImageColorallocate($im,rand(0,255),rand(0,255),rand(0,255));
imagesetpixel($im, rand()%70 , rand()%30 , $randcolor);
}
ImagePNG($im);
ImageDestroy($im);
$_SESSION['VCODE'] = $vcodes;
?&gt;

　　使用的时候，使用下面的代码：

&lt;img src="yz.php" height="17" onclick="this.src='yz.php';" style="cursor:hand;" /&gt;

<strong><span style="text-decoration: underline;">注：</span></strong>
　　上面的代码在PHP里运行正常，即点击图片，换新的验证码，但是在ASP/ASP.NET里面这样的调用却不行，需要用如下的代码才可
&lt;img src="vcode.aspx" style="cursor:hand;" onclick="this.src='vcode.aspx?Time='+Math.random();" /&gt;