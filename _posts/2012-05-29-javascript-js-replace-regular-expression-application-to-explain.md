---
ID: 2071
post_title: >
  JavaScript-JS的replace方法与正则表达式结合应用讲解
author: ChinaBUG
post_excerpt: >
  replace方法的语法是：stringObj.replace(rgExp,
  replaceText)
  其中stringObj是字符串(string)，reExp可以是正则表达式对象(RegExp)也可以是字符串(string)，replaceText是替代查找到的字符串。。为了帮助大家更好的理解，下面举个简单例子说明一下
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/javascript-js-replace-regular-expression-application-to-explain.html
published: true
post_date: 2012-05-29 15:10:45
---
hezhiwu5#163.com    时间：2007-3-22

大家好！！今晚在华软G43*宿舍没什么事做，把javascript中replace方法讲解一下，如果讲得不对或不合理是情理之中的事，因为我不是老鸟，也不是菜鸟，我也不知道我当底是什么鸟？？呵~~

replace方法的语法是：stringObj.replace(rgExp, replaceText) 其中stringObj是字符串(string)，reExp可以是正则表达式对象(RegExp)也可以是字符串(string)，replaceText是替代查找到的字符串。。为了帮助大家更好的理解，下面举个简单例子说明一下

&lt;script language="javascript"&gt;
var stringObj="终古人民共和国，终古人民";

//替换错别字“终古”为“中国”
//并返回替换后的新字符
//原字符串stringObj的值没有改变
var newstr=stringObj.replace("终古","中国");
alert(newstr);
&lt;/script&gt;

比我聪明的你，看完上面的例子之后，会发现第二个错别字“终古”并没有被替换成“中国”，我们可以执行二次replace方法把第二个错别字“终古”也替换掉，程序经过改进之后如下：

&lt;script language="javascript"&gt;
var stringObj="终古人民共和国，终古人民";

//替换错别字“终古”为“中国”
//并返回替换后的新字符
//原字符串stringObj的值没有改变
var newstr=stringObj.replace("终古","中国");

newstr=newstr.replace("终古","中国");
alert(newstr);
&lt;/script&gt;

我们可以仔细的想一下，如果有N的N次方个错别字，是不是也要执行N的N次方replace方法来替换掉错别字呢？？呵，不用怕，有了正则表达式之后不用一个错别字要执行一次replace方法。。程序经过改进之后的代码如下

&lt;script language="javascript"&gt;
var reg=new RegExp("终古","g"); //创建正则RegExp对象
var stringObj="终古人民共和国，终古人民";
var newstr=stringObj.replace(reg,"中国");
alert(newstr);
&lt;/script&gt;

上面讲的是replace方法最简单的应用，不知道大家有没有看懂？？下面开始讲稍微复杂一点的应用。。

大家在一些网站上搜索文章的时候，会发现这么一个现象，就是搜索的关键字会高亮改变颜色显示出来？？这是怎么实现的呢？？其实我们可以用正则表达式来实现，具体怎么样实现呢？简单的原理请看下面的代码

&lt;script language="javascript"&gt;
var str="中华人民共和国，中华人民共和国";
var newstr=str.replace(/(人)/g,"&lt;font color=red&gt;$1&lt;/font&gt;");
document.write(newstr);
&lt;/script&gt;

上面的程序缺少互动性，我们再改进一下程序，实现可以自主输入要查找的字符

&lt;script language="javascript"&gt;
var s=prompt("请输入在查找的字符","人");
var reg=new RegExp("("+s+")","g");
var str="中华人民共和国，中华人民共和国";
var newstr=str.replace(reg,"&lt;font color=red&gt;$1&lt;/font&gt;");
document.write(newstr);
&lt;/script&gt;

可能大家都会对$1这个特殊字符表示什么意思不是很理解，其实$1表示的就是左边表达式中括号内的字符，即第一个子匹配，同理可得$2表示第二个子匹配。。什么是子匹配呢？？通俗点讲，就是左边每一个括号是第一个字匹配，第二个括号是第二个子匹配。。

当我们要把查找到的字符进行运算的时候，怎么样实现呢？？在实现之前，我们先讲一下怎么样获取某一个函数的参数。。在函数Function的内部，有一个arguments集合，这个集合存储了当前函数的所有参数，通过arguments可以获取到函数的所有参数，为了大家理解，请看下面的代码

&lt;script language="javascript"&gt;
function test()
{
alert("参数个数："+arguments.length);
alert("每一个参数的值："+arguments[0]);
alert("第二个参数的值"+arguments[1]);
//可以用for循环读取所有的参数
}

test("aa","bb","cc");
&lt;/script&gt;

看懂上面的程序之后，我们再来看下面一个有趣的程序

&lt;script language="javascript"&gt;
var reg=new RegExp("\d","g");
var str="abd1afa4sdf";
str.replace(reg,function(){alert(arguments.length);});
&lt;/script&gt;

我们惊奇的发现，匿名函数竟然被执行了二次，并且在函数里还带有三个参数，为什么会执行二次呢？？这个很容易想到，因为我们写的正则表达式是匹配单个数字的，而被检测的字符串刚好也有二个数字，故匿名函数被执行了二次。。在匿名函数内部的那三个参数到底是什么内容呢？？为了弄清这个问题，我们看下面的代码。

&lt;script language="javascript"&gt;
function test()
{
for(var i=0;i&lt;arguments.length;i++)
{
alert("第"+(i+1)+"个参数的值："+arguments[i]);
}

}
var reg=new RegExp("\d","g");
var str="abd1afa4sdf";
str.replace(reg,test);
&lt;/script&gt;

经过观察我们发现，第一个参数表示匹配到的字符，第二个参数表示匹配时的字符最小索引位置(RegExp.index)，第三个参数表示被匹配的字符串(RegExp.input)。其实这些参数的个数，还会随着子匹配的变多而变多的。弄清这些问题之后，我们可以用另外的一种写法

&lt;script language="javascript"&gt;
function test($1)
{
return "&lt;font color='red'&gt;"+$1+"&lt;/font&gt;"
}
var s=prompt("请输入在查找的字符","人");
var reg=new RegExp("("+s+")","g");
var str="中华人民共和国，中华人民共和国";
var newstr=str.replace(reg,test);
document.write(newstr);
&lt;/script&gt;

看了上面的程序，原来可以对匹配到的字符为所欲为。下面简单举一个应用的例子

&lt;script language="javascript"&gt;
var str="他今年22岁，她今年20岁，他的爸爸今年45岁，她的爸爸今年44岁，一共有4人"
function test($1)
{
var gyear=(new Date()).getYear()-parseInt($1)+1;
return $1+"("+gyear+"年出生)";
}
var reg=new RegExp("(\d+)岁","g");
var newstr=str.replace(reg,test);
alert(str);
alert(newstr);
&lt;/script&gt;

好了，乱写了这么多，写得有点乱，如果你没有看懂是很正常的，因为我都不知道自己当底写了什么东西，我没事做，练练打字而已的。。呵~~如果有疑问，欢迎发E-Mail问我。886

===================================

两种定义正则表达式对象（RegExp）的方法：
1) var pattern = /s$/;
2) var pattern = new RegExp("s$");

系统学习正则表达式的两本参考：
1）Programming Perl by Larry Wall et al. (O'Reilly).
2）Mastering Regular Expressions by Jeffrey E.F. Friedl (O'Reilly)

转义字符(backslash)：\
字母和数字不需要转义。其他字符如果记不住就用吧。
这些字符有特殊含义，需要转义：^ $ . * + ? = ! : | \ / ( ) [ ] { }
其它特殊字符表示方法：
\0 NULL（unicode十六进制表示法为\u0000，下同)
\t Tab (\u0009)
\n NewLine (\u000A)
\v Vertical tab (\u000B)
\f Form feed (\u000C)
\r Carriage return (\u000D)
\xnn The Latin character specified by the hexadecimal number nn; for example, \x0A is the same as \n
\uxxxx The Unicode character specified by the hexadecimal number xxxx; for example, \u0009 is the same as \t
\cX The control character ^X; for example, \cJ is equivalent to the newline character \n

一类字符的表示方法：
[...] 括号内的任意一个字符
[^...] 除括号内字符之外的任意一个字符
. 除换行符（or another Unicode line terminator）外的任意一个字符
\w 字母、数字或下划线，等价于[a-zA-Z0-9_]
\W 除字母、数字或下划线外的任意一个字符，等价于[^a-zA-Z0-9_].
\s Unicode whitespace
\S 除Unicode whitespace之外的其他字符。须注意\w和\S是不一样的。
\d 数字，等价于[0-9]
\D 非数字，等价于[^0-9].
[\b] backspace (方括号之间的\b指的是键盘上Backspace键对应的字符).
\b   \w和\W之间的位置(锚点)，请注意第二个W是大写字母。

重复匹配模式：
{n,m} 匹配至少n次，但是不超过m次
{n,}    匹配n次或n次以上
{n} 匹配恰好n次
? 匹配0次或1次，等价于{0,1}
+ 匹配1次或1次以上，等价于{1,}
* 匹配0次或0次以上，等价于{0,}

举例：
/\d{2,4}/     2-4个数字
/\w{3}\d?/    1-3位是个字母、数字或下划线，第四位是一个可选的数字
/\s+java\s+/  匹配"java"单词,前后都要有空格，1个或多个空格都行
/[^"]*/       不含双引号的字符串

提醒：使用*和?时要小心。例如/a*/不要求a必须出现，所以"bbb"也会被匹配的。
贪心匹配和不贪心匹配：
之前提到的重复匹配模式会匹配尽可能多的字符，用贪心一词形容很贴切。在重复模式后面跟个问号，则匹配尽可能少的字符。
例如，"aaabbbb"匹配/a*b*/的结果是aaabbbb，匹配/a*b*?/的结果是aaa，匹配/a*?b*/的结果是什么都没有。跟你想的一样么？

选择(alternation)、分组(grouping)和引用(references)
|这个符号从左向右选择第一个可以匹配的模式，例如"aaabbb"匹配/a*|a*b*|b*/的结果是aaa。
()可以构造表达式与|,*,+,?等组合使用。例如/java(script)?/可以匹配"java"或"javascript"(优先）。
()还可以帮助抽取子模式。例如/[a-z]+\d+/可以匹配字母加数字，但是如果你真正关心的是匹配成功后的数字部分，那么，/[a-z]+(\d+)/可以帮到你。
\加一个数字可以引用前面的表达式，无论是否嵌套，总是数左括号的位置。例如/['"][^'"]*['"]/的本意是匹配一对双引号或一对单引号界定的字符串，这个公式还不够严谨。正确的写法是/['"][^'"]*\1/
(?:开头意味着不让引用，例如/([Jj]ava(?:[Ss]cript)?)\sis\s(fun\w*)/这个模式如果有\2，那么引用的就是(fun\w*)了。

提醒：引用提取的是匹配结果(字符串)而不是公式。
锚点（位置匹配）：
锚点本质上是对匹配条件的强化。
最基本的锚点就是^和$，表示字符串的开始位置和结束位置。
例如，\s可以表示空格，用它提取单词/\sJava\s/，会连着空格一起提取出来" Java "，如果不需要空格则可以用/\bJava\b/，提取出来的就是"Java"。
\b就是边界的意思。\B表示无边界。例如/\Bscript\b/可以匹配"javascript"提取"script",/\bscript\b/不能，它返回null。
(?=和(?!分别规定字符串为边界。例如/Java(?!Script)/可以匹配"Java is powerful"，不能匹配"JavaScript is powerful"。

标记(Flags)：
在正则表达式结尾处。i表示不区分大小写。（已验证）
g表示找到全部匹配结果。(有待验证)
m表示多行匹配。(有待验证)