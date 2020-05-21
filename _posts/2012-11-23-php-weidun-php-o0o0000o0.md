---
ID: 2212
post_title: PHP-威盾PHP加密专家解密算法
author: ChinaBUG
post_excerpt: |
  /***********************************
  *威盾PHP加密专家解密算法 By：Neeao
  *http://Neeao.com
  *2009-09-10
  ***********************************/
layout: post
permalink: >
  http://blog.ipodmp.com/2012/11/php-weidun-php-o0o0000o0.html
published: true
post_date: 2012-11-23 00:52:11
---
&lt;?php
/***********************************
*威盾PHP加密专家解密算法 By：Neeao
*http://Neeao.com
*2009-09-10
***********************************/

$filename="shared.php";//要解密的文件
$lines = file($filename);//0,1,2行

//第一次base64解密
$content="";
if(preg_match("/O0O0000O0\('.*'\)/",$lines[1],$y))
{
$content=str_replace("O0O0000O0('","",$y[0]);
$content=str_replace("')","",$content);
$content=base64_decode($content);
}
//第一次base64解密后的内容中查找密钥
$decode_key="";
if(preg_match("/\),'.*',/",$content,$k))
{
$decode_key=str_replace("),'","",$k[0]);
$decode_key=str_replace("',","",$decode_key);
}
//查找要截取字符串长度
$str_length="";
if(preg_match("/,\d*\),/",$content,$k))
{
$str_length=str_replace("),","",$k[0]);
$str_length=str_replace(",","",$str_length);
}
//截取文件加密后的密文
$Secret=substr($lines[2],$str_length);
//echo $Secret;

//直接还原密文输出
echo "&lt;?php\n".base64_decode(strtr($Secret,$decode_key,'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'))."?&gt;";

?&gt;