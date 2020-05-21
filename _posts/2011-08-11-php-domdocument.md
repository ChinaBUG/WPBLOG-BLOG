---
ID: 1718
post_title: PHP-DOMDocument操作XML类属性方法
author: ChinaBUG
post_excerpt: |
  // 首先要建一个DOMDocument对象
  $xml = new DOMDocument();
  
  // 加载Xml文件
  $xml->load("me.xml");
  
  // 获取所有的post标签
  $postDom = $xml->getElementsByTagName("post");
  
  // 循环遍历post标签
  foreach($postDom as $post){
  // 获取Title标签Node
  $title = $post->getElementsByTagName("title");
  
  /**
  * 要获取Title标签的Id属性要分两部走
  * 1. 获取title中所有属性的列表也就是$title->item(0)->attributes
  * 2. 获取title中id的属性，因为其在第一位所以用item(0)
  *
  * 小提示：
  * 若取属性的值可以用item(*)->nodeValue
  * 若取属性的标签可以用item(*)->nodeName
  * 若取属性的类型可以用item(*)->nodeType
  */
  echo "Id: " . $title->item(0)->attributes->item(0)->nodeValue . "<br />";
  echo "Title: " . $title->item(0)->nodeValue . "<br />";
  echo "Details: " . $post->getElementsByTagName("details")->item(0)->nodeValue . "<br /><br />";
  }
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/php-domdocument.html
published: true
post_date: 2011-08-11 18:34:53
---
属性:

Attributes     存储节点的属性列表(只读)
childNodes     存储节点的子节点列表(只读)
dataType     返回此节点的数据类型
Definition     以DTD或XML模式给出的节点的定义(只读)
Doctype     指定文档类型节点(只读)
documentElement     返回文档的根元素(可读写)
firstChild     返回当前节点的第一个子节点(只读)
Implementation     返回XMLDOMImplementation对象
lastChild     返回当前节点最后一个子节点(只读)
nextSibling     返回当前节点的下一个兄弟节点(只读)
nodeName     返回节点的名字(只读)
nodeType     返回节点的类型(只读)
nodeTypedValue     存储节点值(可读写)
nodeValue     返回节点的文本(可读写)
ownerDocument     返回包含此节点的根文档(只读)
parentNode     返回父节点(只读)
Parsed     返回此节点及其子节点是否已经被解析(只读)
Prefix     返回名称空间前缀(只读)
preserveWhiteSpace     指定是否保留空白(可读写)
previousSibling     返回此节点的前一个兄弟节点(只读)
Text     返回此节点及其后代的文本内容(可读写)
url     返回最近载入的XML文档的URL(只读)
Xml     返回节点及其后代的XML表示(只读)

方法:

appendChild     为当前节点添加一个新的子节点,放在最后的子节点后
cloneNode     返回当前节点的拷贝
createAttribute     创建新的属性
createCDATASection     创建包括给定数据的CDATA段
createComment     创建一个注释节点
createDocumentFragment     创建DocumentFragment对象
createElement     创建一个元素节点
createEntityReference     创建EntityReference对象
createNode     创建给定类型,名字和命名空间的节点
createPorcessingInstruction     创建操作指令节点
createTextNode     创建包括给定数据的文本节点
getElementsByTagName     返回指定名字的元素集合
hasChildNodes     返回当前节点是否有子节点
insertBefore     在指定节点前插入子节点
Load     导入指定位置的XML文档
loadXML     导入指定字符串的XML文档
removeChild     从子结点列表中删除指定的子节点
replaceChild     从子节点列表中替换指定的子节点
Save     把XML文件存到指定节点
selectNodes     对节点进行指定的匹配,并返回匹配节点列表
selectSingleNode     对节点进行指定的匹配,并返回第一个匹配节点
transformNode     使用指定的样式表对节点及其后代进行转换

实例获取标签属性.值:

Me.xml

&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;phplamp&gt;
&lt;post&gt;
&lt;title id="1"&gt;PHP XML处理介绍一&lt;/title&gt;
&lt;details&gt;详细内容一&lt;/details&gt;
&lt;/post&gt;
&lt;post&gt;
&lt;title id="2"&gt;PHP XML处理介绍二&lt;/title&gt;
&lt;details&gt;详细内容二&lt;/details&gt;
&lt;/post&gt;
&lt;post&gt;
&lt;title id="3"&gt;PHP XML处理介绍三&lt;/title&gt;
&lt;details&gt;详细内容三&lt;/details&gt;
&lt;/post&gt;
&lt;/phplamp&gt;

// 首先要建一个DOMDocument对象
$xml = new DOMDocument();

// 加载Xml文件
$xml-&gt;load("me.xml");

// 获取所有的post标签
$postDom = $xml-&gt;getElementsByTagName("post");

// 循环遍历post标签
foreach($postDom as $post){
// 获取Title标签Node
$title = $post-&gt;getElementsByTagName("title");

/**
* 要获取Title标签的Id属性要分两部走
* 1. 获取title中所有属性的列表也就是$title-&gt;item(0)-&gt;attributes
* 2. 获取title中id的属性，因为其在第一位所以用item(0)
*
* 小提示：
* 若取属性的值可以用item(*)-&gt;nodeValue
* 若取属性的标签可以用item(*)-&gt;nodeName
* 若取属性的类型可以用item(*)-&gt;nodeType
*/
echo "Id: " . $title-&gt;item(0)-&gt;attributes-&gt;item(0)-&gt;nodeValue . "&lt;br /&gt;";
echo "Title: " . $title-&gt;item(0)-&gt;nodeValue . "&lt;br /&gt;";
echo "Details: " . $post-&gt;getElementsByTagName("details")-&gt;item(0)-&gt;nodeValue . "&lt;br /&gt;&lt;br /&gt;";
}