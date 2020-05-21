---
ID: 2491
post_title: >
  PHP-面向PHP开发人员的XML之PHP
  XML开发15分钟快速入门+xmltoArray与objecttoArray转换函数
author: ChinaBUG
post_excerpt: |
  原文摘自 面向 PHP 开发人员的 XML，第 1 部分: PHP XML 开发 15 分钟快速入门
  最近在开发一号店API、淘宝API等接口APP应用时，因为返回的是JSON或者是XML的格式，所以需要了解怎么操作，这篇文章是专业的，个人觉得蛮完整且简单易懂，所以推荐，下面是摘抄几个自己需要保留的内容，更多的内容请点击上方的链接去完整的阅读原文章。
  在本文后面附上两个函数，一个是XML格式转Array另外一个是OBJ转Array。希望能帮到你哈。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/php-xml-for-php-developers-to-develop-15-minutes-of-the-php-xml-quickstart-and-objecttoarray-and-xmltoarray-conversion-functions.html
published: true
post_date: 2013-06-21 11:21:44
---
原文摘自 <a href="http://www.ibm.com/developerworks/cn/xml/x-xmlphp1.html">面向 PHP 开发人员的 XML，第 1 部分: PHP XML 开发 15 分钟快速入门</a>

最近在开发一号店API、淘宝API等接口APP应用时，因为返回的是JSON或者是XML的格式，所以需要了解怎么操作，这篇文章是专业的，个人觉得蛮完整且简单易懂，所以推荐，下面是摘抄几个自己需要保留的内容，更多的内容请点击上方的链接去完整的阅读原文章。

在本文后面附上两个函数，一个是XML格式转Array另外一个是OBJ转Array。希望能帮到你哈。

<a name="N1010B"></a>使用 DOM

DOM 是在浏览器中使用的、用 JavaScript 操作的 W3C DOM 规范。方法都是一样的，因此可以使用熟悉的编码技术。清单 2 示范了使用 DOM 创建 XML 字符串和 XML 文档并设置格式以便查看。
<a name="c2"></a><b>清单 2. 使用 DOM</b>
<pre>&lt;?php

 //Creates XML string and XML document using the DOM
 $dom = new DomDocument('1.0');

 //add root - &lt;books&gt;
 $books = $dom-&gt;appendChild($dom-&gt;createElement('books'));

 //add &lt;book&gt; element to &lt;books&gt;
 $book = $books-&gt;appendChild($dom-&gt;createElement('book'));

 //add &lt;title&gt; element to &lt;book&gt;
 $title = $book-&gt;appendChild($dom-&gt;createElement('title'));

 //add &lt;title&gt; text node element to &lt;title&gt;
 $title-&gt;appendChild($dom-&gt;createTextNode('Great American
 Novel'));

 //generate xml
 $dom-&gt;formatOutput = true; // set the formatOutput attribute of
 domDocument to true
 // save XML as string or file
 $test1 = $dom-&gt;saveXML(); // put string in test1
 $dom -&gt; save('test1.xml'); // save as file
 ?&gt;

<a name="c6"></a><b>清单 6. 将测试 XML 文件格式化成 PHP include 文件 example.php 以备后面使用</b></pre>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre> &lt;?php
 $xmlstr = &lt;&lt;&lt;XML
 &lt;?xml version='1.0' standalone='yes'?&gt;
 &lt;books&gt;
 &lt;book&gt;
 &lt;title&gt;Great American Novel&lt;/title&gt;
 &lt;characters&gt;
 &lt;character&gt;
 &lt;name&gt;Cliff&lt;/name&gt;
 &lt;desc&gt;really great guy&lt;/desc&gt;
 &lt;/character&gt;
 &lt;character&gt;
 &lt;name&gt;Lovely Woman&lt;/name&gt;
 &lt;desc&gt;matchless beauty&lt;/desc&gt;
 &lt;/character&gt;
 &lt;character&gt;
 &lt;name&gt;Loyal Dog&lt;/name&gt;
 &lt;desc&gt;sleepy&lt;/desc&gt;
 &lt;/character&gt;
 &lt;/characters&gt;
 &lt;plot&gt;
 Cliff meets Lovely Woman. Loyal Dog sleeps, but wakes up to bark
 at mailman.
 &lt;/plot&gt;
 &lt;success type="bestseller"&gt;4&lt;/rating&gt;
 &lt;success type="bookclubs"&gt;9&lt;/rating&gt;
 &lt;/book&gt;
 &lt;/books&gt;
 XML;
 ?&gt;</pre>
</td>
</tr>
</tbody>
</table>
<pre></pre>
在 Ajax 应用程序中，可能需要从 XML 文档提取邮政编码和查询数据库。清单 7 从上面的示例 XML 中直接提取 &lt;plot&gt;。
<pre><a name="c7"></a><b>清单 7. 提取节点 —— 有多么简单？</b></pre>
<pre>&lt;?php

 include 'example.php';

 $xml = new SimpleXMLElement($xmlstr);

 echo $xml-&gt;book[0]-&gt;plot; // "Cliff meets Lovely Woman. Loyal
 Dog..."
 ?&gt;
 <a name="c8"></a><b>清单 8. 提取元素的多个实例</b></pre>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre> &lt;?php

 include 'example.php';

 $xml = new SimpleXMLElement($xmlstr);

 /* For each &lt;book&gt; node, echo a separate &lt;plot&gt;. */
 foreach ($xml-&gt;book as $book) {
 echo $book-&gt;plot, '&lt;br /&gt;';
 }

 ?&gt;</pre>
</td>
</tr>
</tbody>
</table>
<pre></pre>
除了读取元素名称及其值以外，SimpleXML 也能访问元素的属性。清单 9 中就像访问数组成员一样访问元素的属性。
<pre><a name="c9"></a><b> 清单 9. SimpleXML 访问元素的属性</b></pre>
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre>//Input XML file repeated for your convenience

 &lt;?php
 $xmlstr = &lt;&lt;&lt;XML
 &lt;?xml version='1.0' standalone='yes'?&gt;

 &lt;books&gt;
 &lt;book&gt;
 &lt;title&gt;Great American Novel&lt;/title&gt;
 &lt;characters&gt;
 &lt;character&gt;
 &lt;name&gt;Cliff&lt;/name&gt;
 &lt;desc&gt;really great guy&lt;/desc&gt;
 &lt;/character&gt;
 &lt;character&gt;
 &lt;name&gt;Lovely Woman&lt;/name&gt;
 &lt;desc&gt;matchless beauty&lt;/desc&gt;
 &lt;/character&gt;
 &lt;character&gt;
 &lt;name&gt;Loyal Dog&lt;/name&gt;
 &lt;desc&gt;sleepy&lt;/desc&gt;
 &lt;/character&gt;
 &lt;/characters&gt;
 &lt;plot&gt;
 Cliff meets Lovely Woman. Loyal Dog sleeps, but wakes up to bark
 at mailman.
 &lt;/plot&gt;
 &lt;success type="bestseller"&gt;4&lt;/rating&gt;
 &lt;success type="bookclubs"&gt;9&lt;/rating&gt;
 &lt;/book&gt;
 &lt;/books&gt;
 XML;
 ?&gt;

 &lt;?php
 include 'example.php';

 $xml = new SimpleXMLElement($xmlstr);

 /* Access the &lt;success&gt; nodes of the first book.
 * Output the success indications, too. */
 foreach ($xml-&gt;book[0]-&gt;success as $success) {
 switch((string) $success['type']) { // Get attributes as element indices
 case 'bestseller':
 echo $success, ' months on bestseller list';
 break;
 case 'bookclubs':
 echo $success, ' bookclub listings';
 break;
 }
 }
 ?&gt;</pre>
</td>
</tr>
</tbody>
</table>
<pre></pre>
要把元素或属性和字符串进行比较，或者将其传递给需要字符串参数的函数，必须使用（string）强制转换成字符串。否则，默认情况下 PHP 将元素看作对象，如清单 10 所示。
<pre><a name="c10"></a><b>清单 10. 调用字符串或丢弃</b></pre>
<pre>  &lt;?php

 include 'example.php';

 $xml = new SimpleXMLElement($xmlstr);

 if ((string) $xml-&gt;book-&gt;title == 'Great American Novel') {
 print 'My favorite book.';
 }

 htmlentities((string) $xml-&gt;book-&gt;title);
 ?&gt;</pre>
附录：xmltoArray与objecttoArray转换函数（已验证）

private function xml2array($xml){
$arr = array();
foreach ($xml-&gt;children() as $r){
$t = array();
if(count($r-&gt;children()) == 0){
$arr[$r-&gt;getName()] = strval($r);
}else{
$arr[$r-&gt;getName()][] = $this-&gt;xml2array($r);
}
}
return $arr;
}
function obj2array($array){
if(is_object($array)) $array = (array)$array;
if(is_array($array)) foreach($array as $key=&gt;$value){$array[$key] = $this-&gt;obj2array($value);}
return $array;
}