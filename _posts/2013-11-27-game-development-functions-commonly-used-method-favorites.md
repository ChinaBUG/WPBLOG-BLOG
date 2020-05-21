---
ID: 2914
post_title: 游戏开发常用的方法函数收藏
author: ChinaBUG
post_excerpt: |
  PHP篇
  1、全概率函数
  ASP篇
  FLASH篇
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/game-development-functions-commonly-used-method-favorites.html
published: true
post_date: 2013-11-27 11:09:09
---
<h1>PHP篇</h1>
1、全概率函数
<pre>/**
* 全概率计算
* @param array $p array('a'=&gt;0.5,'b'=&gt;0.2,'c'=&gt;0.4)
* @return string 返回上面数组的key
* @author Lukin &lt;my@lukin.cn&gt;
*/
function random($ps){
    static $arr = array(); 
    $key = md5(serialize($ps));
    if (!isset($arr[$key])) {
        $max = array_sum($ps);
        foreach ($ps as $k=&gt;$v) {
            $v = $v / $max * 10000;
            for ($i=0; $i&lt;$v; $i++) $arr[$key][] = $k;
        }
    }
    return $arr[$key][mt_rand(0,count($arr[$key])-1)];
}
实例应用：
/*黑点出现概率99%，白点出现概率1%*/
$setting = array(
    // 黑色概率
    0 =&gt; 0.99,
    // 白色概率
    1 =&gt; 0.01,
);
// Requires the GD Library
header("Content-type: image/png");
$im = imagecreatetruecolor(256, 256) or die("Cannot Initialize new GD image stream");
$white = imagecolorallocate($im, 255, 255, 255);
$start = microtime(true);
for ($y=0; $y&lt;256; $y++) {
    for ($x=0; $x&lt;256; $x++) {
        if (random($setting) === 1) {
            imagesetpixel($im, $x, $y, $white);
        }
    }
}
$time = microtime(true) - $start;
header("X-Exec-Time: ".$time);
imagepng($im);
imagedestroy($im);</pre>
2、
<pre></pre>
&nbsp;
<h1>ASP篇</h1>
1、
<pre></pre>
<h1>FLASH篇</h1>
1、