---
ID: 2008
post_title: 'PHP-Fatal error: Cannot redeclare class mdl_memberattr in G:\PHPnow\htdocs\core\model\member\mdl.memberattr.php on line 3'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/03/php-fatal-error-cannot-redeclare-class-mdl_memberattr-in-gphpnowhtdocscoremodelmembermdl-memberattr-php-on-line-3.html
published: true
post_date: 2012-03-31 10:28:54
---
<strong>Fatal error</strong>: Cannot redeclare class mdl_memberattr in <strong>G:\PHPnow\htdocs\core\model\member\mdl.memberattr.php</strong> on line <strong>3</strong>

在做ShopEX商城二次开发时，经常会建立cmd控制器文件，但是有时候会因为某种原因多写了一些代码就会出现莫名其妙的错误噢。

这个错误就是多写了一个引用，所以会提示不能重新定义哈~检查cmd.memberattr.php文件，就会发现该文件引用了

require_once( CORE_DIR."/model/member/mdl.memberattr.php" );

在ShopEX二次开发中，有点奇怪，奇怪在哪里噢，那就是有的地方需要引用，有的地方又不用引用，都没有一个统一的引用哈。当然也不是没有规律哈。

在
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>类型</td>
<td>必须</td>
<td>非必须</td>
<td>说明</td>
</tr>
<tr>
<td>mdl</td>
<td>include_once('shopObject.php');</td>
<td>-</td>
<td>-</td>
</tr>
<tr>
<td>cmd</td>
<td>-</td>
<td>require_once( "shopObject.php" );</td>
<td>-</td>
</tr>
<tr>
<td>admin:ctl</td>
<td>-</td>
<td>include_once('objectPage.php');</td>
<td>-</td>
</tr>
<tr>
<td>admin:cct</td>
<td>include_once( "objectPage.php" );include_once CORE_DIR.'/admin/controller/system/ctl.tools.php';</td>
<td>-</td>
<td>-</td>
</tr>
<tr>
<td>shop:ctl</td>
<td>-</td>
<td>-</td>
<td>引用的时候有特殊作用</td>
</tr>
<tr>
<td>shop:cct</td>
<td>-</td>
<td>-</td>
<td>-</td>
</tr>
</tbody>
</table>