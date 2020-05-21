---
ID: 4108
post_title: >
  ECStore二次开发日记之0.测试环境，请勿进行真实业务行为_怎么去掉
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/07/ecstore-2dev-diary-0-test-environment-please-do-not-conduct-real-business-how-to-remove.html
published: true
post_date: 2015-07-21 17:47:50
---
根据源码查找到这个提示所在ego.php文件的ecos_cactus_site_check_demosite方法里面，根据逻辑我们可以知道只需要设置一个常量就可以跳过这个逻辑。

方法原型如下：

function ecos_cactus_site_check_demosite($html){
if(defined('DEV_CHECKDEMO') &amp;&amp; DEV_CHECKDEMO){
$pattern = "/&lt;title&gt;(.*)&lt;/title&gt;/";
preg_match($pattern,$html,$title);
$newtitle = "&lt;title&gt;测试环境，请勿进行真实业务行为_".$title[1]."&lt;/title&gt;";
$html = preg_replace($pattern,$newtitle,$html);
}
return $html;
}

我们仅仅需要根据逻辑在config/config.php文件里面随便一个地方增加：

define('DEV_CHECKDEMO', false);

如果已经存在则设置值为false就可以了。