---
ID: 2207
post_title: >
  PHP-给定一个字符串和排列组合长度生成所有可能的排列组合
author: ChinaBUG
post_excerpt: |
  今天在整理ShopEX的博饼插件时，因为使用到中奖样例，简单的就是需要获得1,2,3,4,5,6的全部组合数，自己写了一个生成方法结果太耗资源了，只好网上找一个，结果发现真的能用的没多少，下面推荐的就是可以用的，保存以防止未来可能还需要用到。
  PHP代码的作用就是指定要生成组合的字符，然后组合长度，程序将会自动生成全部的可能组合噢。
  下面代码就是全部的，直接拷贝进PHP文件内，执行即可得到111111~666666之间的全部组合噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/10/php-given-length-of-a-string-and-permutations-generate-all-possible-permutations-and-combinations.html
published: true
post_date: 2012-10-16 10:59:40
---
今天在整理ShopEX的博饼插件时，因为使用到中奖样例，简单的就是需要获得1,2,3,4,5,6的全部组合数，自己写了一个生成方法结果太耗资源了，只好网上找一个，结果发现真的能用的没多少，下面推荐的就是可以用的，保存以防止未来可能还需要用到。

PHP代码的作用就是指定要生成组合的字符，然后组合长度，程序将会自动生成全部的可能组合噢。

下面代码就是全部的，直接拷贝进PHP文件内，执行即可得到111111~666666之间的全部组合噢。

&lt;?
function permutations($letters,$num){
$last = str_repeat($letters{0},$num);
$result = array();
while($last != str_repeat(lastchar($letters),$num)){
$result[] = $last;
$last = char_add($letters,$last,$num-1);
}
$result[] = $last;
return $result;
}
function char_add($digits,$string,$char){
if($string{$char} &lt;&gt; lastchar($digits)){
$string{$char} = $digits{strpos($digits,$string{$char})+1};
return $string;
}else{
$string = changeall($string,$digits{0},$char);
return char_add($digits,$string,$char-1);
}
}
function lastchar($string){
return $string{strlen($string)-1};
}
function changeall($string,$char,$start = 0,$end = 0){
if($end == 0) $end = strlen($string)-1;
for($i=$start;$i&lt;=$end;$i++){
$string{$i} = $char;
}
return $string;
}
?&gt;

{!--To use this Generator you can do something like this : --}

&lt;?
$Array=permutations("123456",6);
for($i=0 ; $i &lt; count($Array) ; $i++) {
echo "$i." . $Array[$i] . "&lt;BR&gt;";
}
?&gt;