---
ID: 3948
post_title: PHP-常用代码库
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/php-common-code-base.html
published: true
post_date: 2015-03-04 14:47:46
---
№.001.根据传入的网站域名获取根域名

/* 函数 getmasterdomain( $url , $url_dot = null )
** 功能 根据传入的网站域名获取根域名
** 参数 $url      网址
** 参数 $url_dot  网址后缀
** 返回 根域名 false为失败
** 特别注意，不适用与，www.net.cn等域名
*/
function getmasterdomain( $url , $url_dot = null ){
if( !$url ) return false;
if( strchr( $url,"." )===false ) return $url;
if( strchr( $url,"http" )===false ) $url = 'http://'.$url;
if( !$url_dot ) $url_dot = array("com","cn","net");
$url_info = parse_url( $url );
$host = explode(".",$url_info['host']);
krsort( $host );
$host = array_values( $host );
$url_out = '';
$url_cur_id = 0;
foreach ($host as $key =&gt; $value) {
if( in_array( $value, $url_dot )){
$url_out = '.' .$value . $url_out;
$url_cur_id += 1;
}
}
return $host[ $url_cur_id ] . $url_out;
}

=.=

№.002.<span class="co1">获取链接的HTML代码</span>

<span class="re0">$html</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><a href="http://www.php.net/file_get_contents"><span class="kw3">file_get_contents</span></a><span class="br0">(</span><span class="st_h">'http://www.example.com'</span><span class="br0">)</span><span class="sy0">;</span>
<span class="re0">$dom</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="kw2">new</span><span class="Apple-converted-space"> </span>DOMDocument<span class="br0">(</span><span class="br0">)</span><span class="sy0">;</span>
<span class="sy0">@</span><span class="re0">$dom</span><span class="sy0">-&gt;</span><span class="me1">loadHTML</span><span class="br0">(</span><span class="re0">$html</span><span class="br0">)</span><span class="sy0">;</span>
<span class="re0">$xpath</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="kw2">new</span><span class="Apple-converted-space"> </span>DOMXPath<span class="br0">(</span><span class="re0">$dom</span><span class="br0">)</span><span class="sy0">;</span>
<span class="re0">$hrefs</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="re0">$xpath</span><span class="sy0">-&gt;</span><span class="me1">evaluate</span><span class="br0">(</span><span class="st_h">'/html/body//a'</span><span class="br0">)</span><span class="sy0">;</span>
<span class="kw1">for</span><span class="Apple-converted-space"> </span><span class="br0">(</span><span class="re0">$i</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="nu0">0</span><span class="sy0">;</span><span class="Apple-converted-space"> </span><span class="re0">$i</span><span class="Apple-converted-space"> </span><span class="sy0">&lt;</span><span class="Apple-converted-space"> </span><span class="re0">$hrefs</span><span class="sy0">-&gt;</span><span class="me1">length</span><span class="sy0">;</span><span class="Apple-converted-space"> </span><span class="re0">$i</span><span class="sy0">++</span><span class="br0">)</span><span class="Apple-converted-space"> </span><span class="br0">{</span>
<span class="re0">$href</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="re0">$hrefs</span><span class="sy0">-&gt;</span><span class="me1">item</span><span class="br0">(</span><span class="re0">$i</span><span class="br0">)</span><span class="sy0">;</span>
<span class="re0">$url</span><span class="Apple-converted-space"> </span><span class="sy0">=</span><span class="Apple-converted-space"> </span><span class="re0">$href</span><span class="sy0">-&gt;</span><span class="me1">getAttribute</span><span class="br0">(</span><span class="st_h">'href'</span><span class="br0">)</span><span class="sy0">;</span>

<span class="co1">// 保留以http开头的链接</span>
<span class="kw1">if</span><span class="br0">(</span><a href="http://www.php.net/substr"><span class="kw3">substr</span></a><span class="br0">(</span><span class="re0">$url</span><span class="sy0">,</span><span class="Apple-converted-space"> </span><span class="nu0">0</span><span class="sy0">,</span><span class="Apple-converted-space"> </span><span class="nu0">4</span><span class="br0">)</span><span class="Apple-converted-space"> </span><span class="sy0">==</span><span class="Apple-converted-space"> </span><span class="st_h">'http'</span><span class="br0">)</span>
<span class="Apple-converted-space"> </span><span class="kw1">echo</span><span class="Apple-converted-space"> </span><span class="re0">$url</span><span class="sy0">.</span><span class="st_h">'&lt;br /&gt;'</span><span class="sy0">;</span>
<span class="br0">}</span>

=.=

№.003.