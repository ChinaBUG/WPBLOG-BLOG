---
ID: 2166
post_title: >
  PHP-for,while,dowhile,foreach与continue,break,goto,exit,return详解
author: ChinaBUG
post_excerpt: |
  continue
  continue 是用来用在循环结构中，控制程序放弃本次循环continue语句之后的代码并转而进行下一次循环。continue本身并不跳出循环结构，只是放弃这一次循环。如果在非循环结构中(例如if语句中，switch语句中)使用continue，程序将会出错。
  
  break
  break是被用在上面所提的各种循环和switch语句中的。他的作用是跳出当前的语法结构，执行下面的语句。break语句可以带一个参数n，表示跳出循环的层数，如果要跳出多重循环的话，可以用n来表示跳出的层数，如果不带参数默认是跳出本重循环。
  
  goto
  goto实 际上只是一个运算符，和其他语言一样，PHP中也不鼓励滥用goto，滥用goto会导致程序的可读性严重下降。goto的作用是将程序的执行从当前位置 跳转到其他任意位置，goto本身并没有要结束的循环的作用，但其跳转位置的作用使得其可以作为跳出循环使用。但PHP5.3及以上版本停止了对goto 的支持，所以应该尽量避免使用goto。
  
  exit
  exit是用来结束程序执行的。可以用在任何地方，本身没有跳出循环的含义。exit可以带一个参数，如果参数是字符串，PHP将会直接把字符串输出，如果参数是integer整形（范围是0-254），那个参数将会被作为结束状态使用。
  
  return
  return 语句是用来结束一段代码，并返回一个参数的。可以从一个函数里调用，也可以从一个include()或者require()语句包含的文件里来调用，也可 以是在主程序里调用，如果是在函数里调用程序将会马上结束运行并返回参数，如果是include()或者require()语句包含的文件中被调用，程序 执行将会马上返回到调用该文件的程序，而返回值将作为include()或者require()的返回值。而如果是在主程序中调用，那么主程序将会马上停 止执行
layout: post
permalink: >
  http://blog.ipodmp.com/2012/09/php-for-while-dowhile-foreach-detailed-continue-break-goto-exit-return.html
published: true
post_date: 2012-09-05 09:41:57
---
<div>

PHP中的循环结构大致有for循环，while循环，do{} while 循环以及foreach循环几种，不管哪种循环中，在PHP中跳出循环大致有这么几种方式：

代码：

&lt;?php
$i = 1;
while (true) { // 这里看上去这个循环会一直执行
<wbr> <wbr> <wbr> if ($i==2) {// 2跳过不显示
<wbr> <wbr> <wbr> <wbr> <wbr> <wbr> <wbr> $i++;
<wbr> <wbr> <wbr> <wbr> <wbr> <wbr> <wbr> continue;
<wbr> <wbr> <wbr> } else if ($i==5) {// 但到这里$i=5就跳出循循环了
<wbr> <wbr> <wbr> <wbr> <wbr> <wbr> <wbr> break;
<wbr> <wbr> <wbr> } else {
<wbr> <wbr> <wbr> <wbr> <wbr> <wbr> <wbr> echo $i . '&lt;br&gt;';
<wbr> <wbr> <wbr> }
<wbr> <wbr> <wbr> $i++;
}
exit;</wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr></wbr>

echo '这里不输出';
?&gt;

结果：

1
3
4
<h3>continue</h3>
continue 是用来用在循环结构中，控制程序放弃本次循环continue语句之后的代码并转而进行下一次循环。continue本身并不跳出循环结构，只是放弃这一次循环。如果在非循环结构中(例如if语句中，switch语句中)使用continue，程序将会出错。

例如在下面的这段PHP代码片段中：
&lt;?php
for($i = 1;$i &lt;= 100; $i++ ){
if($i % 3 == 0 || $i % 7 == 0){
continue;
}
&amp; #160; <wbr> else{
echo”$i n&lt;br/&gt;”;
}
}
?&gt;</wbr>

PHP的代码片段的作用是输出100以内，既不能被7整除又不能被3整除的那些自然数，循环中先用if条件语句判断那些能被整除的数，然后执行 continue;语句，就直接进入了下个循环。不会执行下面的输出语句了。
<h3>break</h3>
break是被用在上面所提的各种循环和switch语句中的。他的作用是跳出当前的语法结构，执行下面的语句。break语句可以带一个参数n，表示跳出循环的层数，如果要跳出多重循环的话，可以用n来表示跳出的层数，如果不带参数默认是跳出本重循环。

看下面这个多重循环嵌套的例子：
for($i = 1;$i &lt;= 10; $i++ ){
for($j = 1;$j &lt;= 10;$j++){
$m = $i * $i + $j * $j;
echo”$m n&lt;br/&gt;”;
if($m &lt; 90 || $m &gt; 190) {
break 2;
}
}
}

这里使用了break 2跳出了两重循环，你可以试验一眼，将2去掉，得到的结果是完全不一样的。如果不使用参数，跳出的只是本次循环，第一层循环会继续执行下去。
<h3>goto</h3>
goto实 际上只是一个运算符，和其他语言一样，PHP中也不鼓励滥用goto，滥用goto会导致程序的可读性严重下降。goto的作用是将程序的执行从当前位置 跳转到其他任意位置，goto本身并没有要结束的循环的作用，但其跳转位置的作用使得其可以作为跳出循环使用。但PHP5.3及以上版本停止了对goto 的支持，所以应该尽量避免使用goto。
下面的是一个使用了goto跳出循环的例子
for($i = 1000;$i &gt;= 1 ; $i– ){
if( sqrt($i) &lt;= 29){
goto a;
}
echo “$i”;
}
a:
echo” this is the end”;

例子中使用了goto来跳出循环，这个例子用来检测1000以内，那些数的平方根大于29。
<h3>exit</h3>
exit是用来结束程序执行的。可以用在任何地方，本身没有跳出循环的含义。exit可以带一个参数，如果参数是字符串，PHP将会直接把字符串输出，如果参数是integer整形（范围是0-254），那个参数将会被作为结束状态使用。

&lt;?php
for($i = 1000;$i &gt;= 1 ; $i– ){
if( sqrt($i) &gt;= 29){
echo”$i n&lt;br/&gt;”;
}
else{
exit;
}
}
echo”本行将不会被输出”;
?&gt;

上面这个例子中直接在从循环里结束了代码的运行，这样会导致后面的代码都不会被执行，如果是在一个php web 页面里面，甚至连exit后面的html代码都不会被输出。
<h3>return</h3>
return 语句是用来结束一段代码，并返回一个参数的。可以从一个函数里调用，也可以从一个include()或者require()语句包含的文件里来调用，也可 以是在主程序里调用，如果是在函数里调用程序将会马上结束运行并返回参数，如果是include()或者require()语句包含的文件中被调用，程序 执行将会马上返回到调用该文件的程序，而返回值将作为include()或者require()的返回值。而如果是在主程序中调用，那么主程序将会马上停 止执行

&lt;?php
for($i = 1000;$i &gt;= 1 ; $i– ){
if( sqrt($i) &gt;= 29){
echo”$i n&lt;br/&gt;”;
}
else{
return;
}
}
echo”本行将不会被输出”;
?&gt;

这里的例子和上面使用exit的效果是一样的。
<h3>在循环结束条件，自然跳出</h3>
这个当然是最好理解了，当循环满足循环临界条件时就是自己退出。

以上是PHP中跳出循环的几种方式的简单总结。

注本文原地址：http://blog.sina.com.cn/s/blog_5f0d5bd90100mjyu.html

</div>