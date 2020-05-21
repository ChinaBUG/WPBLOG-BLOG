---
ID: 2245
post_title: 'PHP-花括号({})的应用说明'
author: ChinaBUG
post_excerpt: |
  ⑴、正常的教程上告诉我们，花括号是方法的分隔符，如 function name(){}, for(){}, ….
  ⑵、像前言的代码之中的$OOO000000{4}这样在字符串的变量的后面跟上{}的形式，则代表是把这个字符串变量当成数组处理。
  ⑶、我们知道双引号内的变量会自动替换，但是，一旦这个变量是一个数组变量时（如$abc['name']），就可能会出现错误，怎么解决呢？其实就是加上花括号即可！这时候大括号起的作用就是，告诉PHP，括起来的要当成变量处理。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/application-notes-curly-braces.html
published: true
post_date: 2012-12-27 14:59:46
---
前言

最近在破解一个插件的时候发现里面有些东西自己都不是很懂，哎，基础不好就是悲剧呀，只要有机会还是要多学学，最要命的还是基础手册上没怎么说明的，还是google好，找了一下，学习了，这边就做下记录。

程序部分代码如下：
<code>
$OOO000000=urldecode('%61%68%36%73%62%65%68%71%6c%61%34%63%6f%5f%73%61%64');
$OOO0000O0=$OOO000000{4}.$OOO000000{9}.$OOO000000{3}.$OOO000000{5};
$OOO0000O0.=$OOO000000{2}.$OOO000000{10}.$OOO000000{13}.$OOO000000{16};
$OOO0000O0.=$OOO0000O0{3}.$OOO000000{11}.$OOO000000{12}.$OOO0000O0{7}.$OOO000000{5};
$O0O0000O0='OOO0000O0';
;
echo '';
</code>
这几段代码涉及到花括号的应用，哎，基础教程里面没怎么见过说明。

正文

⑴、正常的教程上告诉我们，花括号是方法的分隔符，如 function name(){}, for(){}, ….

⑵、像前言的代码之中的$OOO000000{4}这样在字符串的变量的后面跟上{}的形式，则代表是把这个字符串变量当成数组处理。

⑶、我们知道双引号内的变量会自动替换，但是，一旦这个变量是一个数组变量时（如$abc['name']），就可能会出现错误，怎么解决呢？其实就是加上花括号即可！这时候大括号起的作用就是，告诉PHP，括起来的要当成变量处理。

下面来下例子，帮助理解哈：
<code>
$abc='123';
echo $abc;echo "&lt;br/&gt;";
echo "{$abc}";echo "&lt;br/&gt;";
echo "${abc}";echo "&lt;br/&gt;";
echo ${abc}; echo "&lt;br/&gt;";
echo ${'abc'}[1]; echo "&lt;br/&gt;";
</code>
运行结果会出现
<code>
123
123
123
<b>Notice</b>: Use of undefined constant abc - assumed 'abc' in <b>...</b><b>18</b>
123
2
</code>
证明echo ${abc};的写法是错误的，只能写成${'abc'}才不会产生歧义，当然你要是开启error_reporting(E_ALL ^ E_NOTICE);的话，那么是不会有提示的。