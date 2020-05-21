---
ID: 2125
post_title: PHP-简单的PHP模板引擎
author: ChinaBUG
post_excerpt: PHP-简单的PHP模板引擎
layout: post
permalink: >
  http://blog.ipodmp.com/2012/07/php-simple-php-template-engine.html
published: true
post_date: 2012-07-17 17:19:41
---
01 function template_compile($tplfile,$tplcachefile)

02 {

03  $str = file_get_contents($tplfile);

04  $str = template_parse($str);

05  $strlen = file_put_contents($tplcachefile, $str);

06  @chmod($tplcachefile, 0777);

07  return $strlen;

08 }

09

10 function template_parse($tpl)

11 {

12  $tpl = preg_replace("/([\n\r]+)\t+/s","\\1",$tpl);

13  $tpl = preg_replace("/\&lt;\!\-\-\{(.+?)\}\-\-\&gt;/s", "{\\1}",$tpl);

14  $tpl = preg_replace("/\{template\s+(.+)\}/","\n&lt;?php include template(\\1); ?&gt;\n",$tpl);

15  $tpl = preg_replace("/\{include\s+(.+)\}/","\n&lt;?php include \\1; ?&gt;\n",$tpl);

16  $tpl = preg_replace("/\{php\s+(.+)\}/","\n&lt;?php \\1?&gt;\n",$tpl);

17  $tpl = preg_replace("/\{if\s+(.+?)\}/","&lt;?php if(\\1) { ?&gt;",$tpl);

18  $tpl = preg_replace("/\{else\}/","&lt;?php } else { ?&gt;",$tpl);

19  $tpl = preg_replace("/\{elseif\s+(.+?)\}/","&lt;?php } elseif (\\1) { ?&gt;",$tpl);

20  $tpl = preg_replace("/\{\/if\}/","&lt;?php } ?&gt;",$tpl);

21  $tpl = preg_replace("/\{loop\s+(\S+)\s+(\S+)\}/","&lt;?php if(is_array(\\1)) foreach(\\1 AS \\2) { ?&gt;",$tpl);

22  $tpl = preg_replace("/\{loop\s+(\S+)\s+(\S+)\s+(\S+)\}/","\n&lt;?php if(is_array(\\1)) foreach(\\1 AS \\2 =&gt; \\3) { ?&gt;",$tpl);

23  $tpl = preg_replace("/\{\/loop\}/","\n&lt;?php } ?&gt;\n",$tpl);

24  $tpl = preg_replace("/\{([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*\(([^{}]*)\))\}/","&lt;?php echo \\1;?&gt;",$tpl);

25  $tpl = preg_replace("/\{\\$([a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*\(([^{}]*)\))\}/","&lt;?php echo \\1;?&gt;",$tpl);

26  $tpl = preg_replace("/\{(\\$[a-zA-Z_\x7f-\xff][a-zA-Z0-9_\x7f-\xff]*)\}/","&lt;?php echo \\1;?&gt;",$tpl);

27  $tpl = preg_replace("/\{(\\$[a-zA-Z0-9_\[\]\'\"\$\x7f-\xff]+)\}/es", "addquote('&lt;?php echo \\1;?&gt;')",$tpl);

28  $tpl = preg_replace("/\{([A-Z_\x7f-\xff][A-Z0-9_\x7f-\xff]*)\}/s", "&lt;?php echo \\1;?&gt;",$tpl);

29  $tpl = "&lt;?php if(!defined('IN_PHPMPS'))die('Access Denied'); ?&gt;".$tpl;

30  return $tpl;

31 }