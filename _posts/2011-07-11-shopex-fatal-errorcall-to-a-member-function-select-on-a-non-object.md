---
ID: 1575
post_title: >
  ShopEX-Fatal error:Call to a member
  function select() on a non-object
author: ChinaBUG
post_excerpt: |
  系统暂时发生错误，请回到首页重新访问
  Fatal error: Call to a member function select() on a non-object in G:\PHPnow\htdocs\plugins\widgets\tagcloud\widget_tagcloud.php on line 19
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-fatal-errorcall-to-a-member-function-select-on-a-non-object.html
published: true
post_date: 2011-07-11 11:37:41
---
今天下载了一个<a href="http://www.shopextool.cn/" target="_blank">shopextool</a>开发的免费插件：标签云插件，他的源码是加密的，研究一下吧，破解了，然后，悲剧发生了，提示如下：

<strong>系统暂时发生错误，请回到首页重新访问</strong>

<hr />

<strong>Fatal error: Call to a member function select() on a non-object in <strong>G:\PHPnow\htdocs\plugins\widgets\tagcloud\widget_tagcloud.php</strong> on line <strong>19</strong></strong>

奇怪为啥会出现这个问题呢？我查看了源码，都没发现有什么问题，把一摸一样的代码从其他正常的文件中复制过来，正常使用，但是一用破解出来的代码，就提示这个错误。

可以肯定，肯定是破解的代码有问题，可是问题出现在那里呢？

难道是空格问题？

破解出来的代码会有好多乱七八糟的空格哈，有破解过的人都知道滴。

直接把空格位置删掉，一运行，哈~~真的可以运行了。

空格问题！

不过又不是空格，因为是特殊符号，经过查看，发觉里面是一个不可视的特殊码，比如：'　'，引号里面的符号（按住alt+41377就会出现），只要这类的符号出现之后，肯定试运行不了的。

不信你试试看哈~~~

标签云插件：

<a href="http://www.shopextool.cn/down/ShopEx_tagcloud_widget.zip" target="_blank">原版下载</a>  <a href="http://u.ipodmp.com/viewfile.php?file_id=249&amp;file_key=QGlaC3lS" target="_blank">破解版下载(格式整理过滴)</a>

======

另外，我在开发中还会出现这样的问题：

系统暂时发生错误，请回到首页重新访问

<hr />

Fatal error: Call to a member function getCart() on a non-object in <strong>G:\PHPnow\htdocs\core2\shop\controller\cct.order.php</strong> on line <strong>20</strong>

一般，都是没有定义这个对象实例，只要定义了，就可以正常访问了。