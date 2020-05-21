---
ID: 3742
post_title: >
  PHP-10个不太为人所知的，但实用的PHP函数
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/12/php-10-is-not-known-but-useful-php-functions.html
published: true
post_date: 2014-12-17 17:29:57
---
PHP拥有非常丰富的内置函数，并且大多数我们是知道的。有许多功能，这些功能不是很出名，但真的非常有用。在这篇文章中，我已经列出并解释了一些鲜为人知的，但真正有用的PHP函数。

php_check_syntax

这是一个非常有用的功能，用于检查一个指定文件的语法。

用法:

[php]&lt;?php
$error_message = &quot;&quot;;
$filename = &quot;./php_script.php&quot;;
if(!php_check_syntax($filename, &amp;$error_message)) {
echo &quot;Errors were found in the file $filename: $error_message&quot;;
} else {
echo &quot;The file $filename contained no syntax errors&quot;;
}
?&gt;[/php]

来源: http://www.php.net/manual/en/function.php-check-syntax.php

highlight_string

该highlight_string()函数可以让我们在Web页面上展示语法高亮的PHP代码。这个函数利用内置的语法高亮功能对给定的PHP代码进行语法着色，并返回结果。

用法:

[php]&lt;?php
highlight_string(' &lt;?php phpinfo(); ?&gt;');
?&gt;[/php]

来源: http://php.net/manual/en/function.highlight-string.php

show_source

show_source() 函数的功能与上面介绍的 highlight_file () 相似。可以对一个给定的PHP文件进行语法着色。语法高亮使用HTML标记。运行成功返回TRUE，失败返回FALSE。

用法:

[php]&lt;?php
show_source(&quot;php_script.php&quot;);
?&gt;[/php]

来源: http://www.php.net/manual/en/function.show-source.php

php_strip_whitespace

如前所述，与show_source（）函数类似。此函数也是返回特定文件源代码。但是删除了PHP注释和空白的源代码。

用法:

[php]&lt;?php
 echo php_strip_whitespace(&quot;php_script.php&quot;);
 ?&gt;[/php]

来源: http://www.php.net/manual/en/function.php-strip-whitespace.php

__halt_compiler

此函数用于停止编译器的执行。这对于在PHP脚本中嵌入数据很有用，如安装文件。

用法:

[php]&lt;?php
 $fp = fopen(__FILE__, 'r');
 fseek($fp, __COMPILER_HALT_OFFSET__);
 var_dump(stream_get_contents($fp));
 // the end of the script execution
 __halt_compiler();
 ?&gt;[/php]

来源: http://www.php.net/manual/en/function.halt-compiler.php

highlight_file

这是一个非常的PHP函数返回带PHP语法高亮显示特定PHP文件。

用法:

[php]&lt;?php
 highlight_file(&quot;php_script.php&quot;);
 ?&gt;[/php]

来源: http://www.php.net/manual/en/function.highlight-file.php

ignore_user_abort

此功能可用于客户端ABOT脚本。客户端将中止导致脚本停止运行。
用法:

[php]&lt;?php
ignore_user_abort();
?&gt;[/php]

来源: http://www.php.net/manual/en/function.ignore-user-abort.php

str_word_count

这个函数是用来计算在字符串中找到词的数量。

用法:

[php]&lt;?php
echo str_word_count(&quot;Hello How Are You!&quot;);
?&gt;[/php]

来源: http://php.net/manual/en/function.str-word-count.php

get_defined_vars

这是一个方便的功能，调试时。该函数能够返回一个包含所有定义的变量列表的多维数组。

用法:

[php]&lt;?php
print_r(get_defined_vars());
?&gt;[/php]

来源: http://php.net/manual/en/function.get-defined-vars.php

get_browser

这个函数会查找查找browscap.ini文件并返回浏览器的性能。

用法:

[php]&lt;?php
echo $_SERVER['HTTP_USER_AGENT'];
$browser = get_browser();
print_r($browser);
?&gt;[/php]

来源: http://www.php.net/manual/en/function.get-browser.php

&nbsp;

附录：

鉴于我经常忘记方法，所以这边放几个我常用又容易忘记的方法吧。

1、HTML实体符号转换的方法

htmlspecialchars(string,quotestyle,character-set)

htmlentities(string,quotestyle,character-set)

htmlspecialchars_decode()

2、输出打印调用的来源方法，调用栈

function print_stack_trace()
{
$array =debug_backtrace();
//print_r($array);//信息很齐全
unset($array[0]);
foreach($array as $row)
{
$html .=$row['file'].':'.$row['line'].'行,调用方法:'.$row['function']."&lt;p&gt;";
}
return$html;
}

&nbsp;