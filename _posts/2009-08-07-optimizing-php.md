---
ID: 87
post_title: PHP 性能优化技巧
author: ChinaBUG
post_excerpt: |
  1. 不要随便就复制变量
  2. 对字符串使用单引号
  3. 使用 echo 函数来输出字符串
  4. 不要在 echo 中使用连接符
  5. 使用 switch/case 代替 if/else
  作者：Denis
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/optimizing-php.html
published: true
post_date: 2009-08-07 10:00:52
---
　　Google 在 Google Code 制作了 “<a href="http://code.google.com/speed/" target="_blank">Let’s make the web faster</a>” （让我们使得 Web 更快）的网站中，分享了一些如网页性能优化的技巧和教程以及工具，今天我就翻译一篇技巧文章：PHP 性能优化技巧，他说的5条技巧我都不知道。

1. 不要随便就复制变量

　　有时候为了使 PHP 代码更加整洁，一些 PHP 新手（包括我）会把预定义好的变量复制到一个名字更简短的变量中，其实这样做的结果是增加了一倍的内存消耗，只会使程序更加慢。试想一下，在下面的例子中，如果用户恶意插入 512KB 字节的文字到文本输入框中，这样就会导致 1MB 的内存被消耗！
<blockquote>BAD:

$description = $_POST['description'];
echo $description;

GOOD:

echo $_POST['description'];</blockquote>
2. 对字符串使用单引号

　　PHP 引擎允许使用单引号和双引号来封装字符串变量，但是这个是有很大的差别的！使用双引号的字符串告诉 PHP 引擎首先去读取字符串内容，查找其中的变量，并改为变量对应的值。一般来说字符串是没有变量的，所以使用双引号会导致性能不佳。最好是使用字符串连接而不是双引号字符串。
<blockquote>BAD:

$output = "This is a plain string";

GOOD:

$output = 'This is a plain string';

BAD:

$type = "mixed";
$output = "This is a $type string";

GOOD:

$type = 'mixed';$output = 'This is a ' . $type .' string';</blockquote>
3. 使用 echo 函数来输出字符串

　　使用 echo() 函数来打印结果出了有更容易阅读之外，在下个例子中，你还可以看到有更好的性能。
<blockquote>BAD:

print($myVariable);

GOOD:

echo $myVariable;</blockquote>
4. 不要在 echo 中使用连接符

　　很多 PHP 程序员（有包括我）不知道在用 恶臭 输出多个变量的时候，其实可以使用逗号来分开的，而不必用字符串先把他们先连起来，如下面的第一个例子中，由于使用了连接符就会有性能问题，因为这样就会需要 PHP 引擎首先把所有的变量连接起来，然后在输出，而在第二个例子中，PHP 引擎就会按照循序输出他们。
<blockquote>BAD:

echo 'Hello, my name is' . $firstName . $lastName . ' and I live in ' . $city;

GOOD:

echo 'Hello, my name is' , $firstName , $lastName , ' and I live in ' , $city;</blockquote>
5. 使用 switch/case 代替 if/else

　　对于只有单个变量的判断，使用 switch/case 语句而不是 if/else 语句，会有更好的性能，并且代码更加容易阅读和维护。
<blockquote>BAD:

if($_POST['action'] == 'add') {
　　addUser();
} elseif ($_POST['action'] == 'delete') {
　　deleteUser();
} elseif ($_POST['action'] == 'edit') {
　　editUser();
} else {
　　defaultAction();
}

GOOD:

switch($_POST['action']) {
　　case 'add':  addUser();  break;
　　case 'delete':   deleteUser();  break;
　　case 'edit':   editUser();break;
　　default:   defaultAction();   break;
}</blockquote>
<p style="text-align: right;">作者：Denis</p>