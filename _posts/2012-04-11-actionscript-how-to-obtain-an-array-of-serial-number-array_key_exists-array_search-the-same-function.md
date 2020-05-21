---
ID: 2023
post_title: >
  ActionScript-如何获取数组的序号(实现array_key_exists,array_search相同的功能)
author: ChinaBUG
post_excerpt: |
  在PHP之中有array_key_exists,array_search函数用于检查键及键值是否存在数组之中，那么在FLASH之中有吗？
  好像是没有~~不过没关系，我们自己来DIY就行~~这个正好学习一下如何扩展FALSH的方法~
layout: post
permalink: >
  http://blog.ipodmp.com/2012/04/actionscript-how-to-obtain-an-array-of-serial-number-array_key_exists-array_search-the-same-function.html
published: true
post_date: 2012-04-11 19:10:21
---
在PHP之中有array_key_exists,array_search函数用于检查键及键值是否存在数组之中，那么在FLASH之中有吗？

好像是没有~~不过没关系，我们自己来DIY就行~~这个正好学习一下如何扩展FALSH的方法~

~~代码如下~~

Array.prototype.getIndex = function(data) {
for (i=0; i&lt;this.length; ++i) {
if (this[i] == data) {
return i;
}
}
return -1;
};

测试：

resources = new Array("a", "b", "c", "d", "e", "f", "g", "h");
location = resources.getIndex("e");
trace(location);