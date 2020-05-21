---
ID: 3270
post_title: PHP-调试堆栈
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/php-debug-stack.html
published: true
post_date: 2014-03-17 17:06:44
---
在调试的时候经常想要知道该方法在哪里被调用，传入什么参数，那个方法被执行等信息，怎么才能获取到呢？

其实，PHP有调试堆栈，什么东西我也不懂，照做吧，下面是我的一个输出方法：

具体语法请参考：<a href="http://www.w3school.com.cn/php/func_error_debug_backtrace.asp">PHP debug_backtrace() 函数</a>

[php]
function print_stack_trace(){
$array =debug_backtrace();
unset($array[0]);
foreach($array as $row){
echo '&amp;amp;lt;br/&amp;amp;gt;';
echo '文件：'.$row['file'].'&amp;amp;lt;br/&amp;amp;gt;';
echo '行数：'.$row['line'].'&amp;amp;lt;br/&amp;amp;gt;';
echo '类名：'.$row['class'].'&amp;amp;lt;br/&amp;amp;gt;';
echo '方法：'.$row['function'].'&amp;amp;lt;br/&amp;amp;gt;';
echo '类型：'.$row['type'].'&amp;amp;lt;br/&amp;amp;gt;';
echo '参数：&amp;amp;lt;br/&amp;amp;gt;';
print_r($row['args']);
echo '&amp;amp;lt;br/&amp;amp;gt;';
}
}
[/php]

在需要的时候请直接调用即可输出好多的信息。