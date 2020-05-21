---
ID: 1590
post_title: >
  PHP-error_reporting(E_ALL ^
  E_NOTICE)与error_reporting(0);
author: ChinaBUG
post_excerpt: |
  error_reporting() 设置 PHP 的报错级别并返回当前级别。
  ; 错误报告是按位的。或者将数字加起来得到想要的错误报告等级。
  ; E_ALL - 所有的错误和警告
  ; E_ERROR - 致命性运行时错
  ; E_WARNING - 运行时警告（非致命性错）
  ; E_PARSE - 编译时解析错误
  ; E_NOTICE - 运行时提醒(这些经常是是你的代码的bug引起的，
  ;也可能是有意的行为造成的。(如：基于未初始化的变量自动初始化为一个;空字符串的事实而使用一个未初始化的变量)
  ; E_CORE_ERROR - 发生于PHP启动时初始化过程中的致命错误
  ; E_CORE_WARNING - 发生于PHP启动时初始化过程中的警告(非致命性错)
  ; E_COMPILE_ERROR - 编译时致命性错
  ; E_COMPILE_WARNING - 编译时警告(非致命性错)
  ; E_USER_ERROR - 用户产生的出错消息
  ; E_USER_WARNING - 用户产生的警告消息
  ; E_USER_NOTICE - 用户产生的提醒消息
  
  使用方法：
  error_reporting(0);//禁用错误报告
  error_reporting(E_ALL ^ E_NOTICE);//显示除去 E_NOTICE 之外的所有错误信息
  error_reporting(E_ERROR | E_WARNING | E_PARSE);//显示运行时错误，与error_reporting(E_ALL ^ E_NOTICE);效果相同。error_reporting(E_ALL);//显示所有错误
  error_reporting(0)
  error_reporting(255);
  是列出所有提示error_reporting(0);
  是不显示所有提示建议使用error_reporting(7);
  只显示严重错误
  1 E_ERROR 致命的运行时错误
  2 E_WARNING 运行时警告(非致命性错误)
  4 E_PARSE 编译时解析错误
  8 E_NOTICE 运行时提醒(经常是bug，也可能是有意的)
  16 E_CORE_ERROR PHP启动时初始化过程中的致命错误
  32 E_CORE_WARNING PHP启动时初始化过程中的警告(非致命性错)
  64 E_COMPILE_ERROR 编译时致命性错
  128 E_COMPILE_WARNING 编译时警告(非致命性错)
  256 E_USER_ERROR 用户自定义的致命错误
  512 E_USER_WARNING 用户自定义的警告(非致命性错误)
  1024 E_USER_NOTICE 用户自定义的提醒(经常是bug，也可能是有意的)
  2048 E_STRICT 编码标准化警告(建议如何修改以向前兼容)
  4096 E_RECOVERABLE_ERROR 接近致命的运行时错误，若未被捕获则视同E_ERROR
  6143 E_ALL 除E_STRICT外的所有错误(PHP6中为8191,即包含所有)
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/php-error_reportinge_all-e_notice-error_reporting.html
published: true
post_date: 2011-07-13 10:26:26
---
虫神曰：

那个“^”符号是表示排除的意思哈，就是提示全部的信息，除了E_NOTICE类型的错误。

~

在看帝国cms的connect.php是发现第一句是error_reporting(E_ALL ^ E_NOTICE);

以前也没注意过这个语句，知道是设置错误提示的，但不清楚具体怎样设置使用。下面从网上摘抄了些东西，总结了一下。

举例说明：

在Windows环境下：原本在php4.3.0中运行正常的程序，在4.3.1中为何多处报错，大体提示为：Notice:Undefined varialbe:变量名称.
例如有如下的代码：
if (!$tmp_i) {
$tmp_i=10;
}
在4.3.0中运行正常，在4.3.1中运行会提示Notice:Undefined varialbe:tmp_i
问题下下：
1.问题出在哪里？
2.应如何修改这段代码？
3.不改段代码，如何修改php.ini中的设置使原来在4.3.0中的程序在4.3.1的环境下运行正常?而不出现这个错误提示.

解决办法：

在程序开头加一句：
error_reporting(E_ALL &amp; ~E_NOTICE); 或error_reporting(E_ALL ^ E_NOTICE);

或者
修改php.ini
error_reporting  =  E_ALL &amp; ~E_NOTICE

有关error_reporting()函数：

error_reporting() 设置 PHP 的报错级别并返回当前级别。

; 错误报告是按位的。或者将数字加起来得到想要的错误报告等级。
; E_ALL - 所有的错误和警告
; E_ERROR - 致命性运行时错
; E_WARNING - 运行时警告（非致命性错）
; E_PARSE - 编译时解析错误
; E_NOTICE - 运行时提醒(这些经常是是你的代码的bug引起的，

;也可能是有意的行为造成的。(如：基于未初始化的变量自动初始化为一个空字符串的事实而使用一个未初始化的变量)

; E_CORE_ERROR - 发生于PHP启动时初始化过程中的致命错误
; E_CORE_WARNING - 发生于PHP启动时初始化过程中的警告(非致命性错)
; E_COMPILE_ERROR - 编译时致命性错
; E_COMPILE_WARNING - 编译时警告(非致命性错)
; E_USER_ERROR - 用户产生的出错消息
; E_USER_WARNING - 用户产生的警告消息
; E_USER_NOTICE - 用户产生的提醒消息

使用方法：

error_reporting(0);//禁用错误报告
error_reporting(E_ALL ^ E_NOTICE);//显示除去 E_NOTICE 之外的所有错误信息
error_reporting(E_ERROR | E_WARNING | E_PARSE);//显示运行时错误，与error_reporting(E_ALL ^ E_NOTICE);效果相同。error_reporting(E_ALL);//显示所有错误

本文来自CSDN博客，转载请标明出处：

http://blog.csdn.net/boboxiaopangde/archive/2010/11/14/6008293.aspx
error_reporting(0)

error_reporting(255);

是列出所有提示

error_reporting(0);

是不显示所有提示

建议使用

error_reporting(7);

只显示严重错误

1 E_ERROR 致命的运行时错误

2 E_WARNING 运行时警告(非致命性错误)

4 E_PARSE 编译时解析错误

8 E_NOTICE 运行时提醒(经常是bug，也可能是有意的)

16 E_CORE_ERROR PHP启动时初始化过程中的致命错误

32 E_CORE_WARNING PHP启动时初始化过程中的警告(非致命性错)

64 E_COMPILE_ERROR 编译时致命性错

128 E_COMPILE_WARNING 编译时警告(非致命性错)

256 E_USER_ERROR 用户自定义的致命错误

512 E_USER_WARNING 用户自定义的警告(非致命性错误)

1024 E_USER_NOTICE 用户自定义的提醒(经常是bug，也可能是有意的)

2048 E_STRICT 编码标准化警告(建议如何修改以向前兼容)

4096 E_RECOVERABLE_ERROR 接近致命的运行时错误，若未被捕获则视同E_ERROR

6143 E_ALL 除E_STRICT外的所有错误(PHP6中为8191,即包含所有)