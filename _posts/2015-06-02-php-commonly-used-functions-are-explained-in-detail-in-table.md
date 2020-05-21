---
ID: 2838
post_title: PHP-常用函数方法详细说明表
author: ChinaBUG
post_excerpt: |
  判断指定的文件及文件夹是否可写。
  读文件，获取网络的文件
  写文件，保存网络的文件到本地
  怎么获取一个连接地址的文件名、目录名、文件扩展名
  怎么读取CSV文件，解析 CSV 字段
  除了replace之外还有什么可以替换字符的方法呢？使用strtr就可以。
layout: post
permalink: >
  http://blog.ipodmp.com/2015/06/php-commonly-used-functions-are-explained-in-detail-in-table.html
published: true
post_date: 2015-06-02 10:24:07
---
<ol>
	<li>判断指定的文件及文件夹是否可写。
语法：is_writable(file)     <a href="http://www.w3school.com.cn/php/func_filesystem_is_writable.asp">参考</a>
说明：
<ul>
	<li>如果文件存在并且可写则返回 true。</li>
	<li><i>file</i> 参数可以是一个允许进行是否可写检查的目录名。</li>
</ul>
<strong><span style="color: #ff0000;">注意：</span></strong><span style="text-decoration: underline;">本函数的结果会被缓存。请使用 clearstatcache() 来清除缓存。</span></li>
	<li>读文件，获取网络的文件
语法：file_get_contents(<i>path</i>,<i>include_path</i>,<i>context</i>,<i>start</i>,<i>max_length</i>)     <a href="http://www.w3school.com.cn/php/func_filesystem_file_get_contents.asp">参考</a>
说明：
<ul>
	<li>file_get_contents() 把文件读入一个字符串。</li>
	<li>max_length规定读取的字节数。</li>
</ul>
</li>
	<li>写文件，保存网络的文件到本地
语法：file_put_contents(file,data,mode,context)     <a href="http://www.w3school.com.cn/php/func_filesystem_file_put_contents.asp">参考</a>
说明：
<ul>
	<li>mode一般为空要不就是FILE_APPEND表示附加。</li>
	<li>参数 data 可以是数组但不能是多维数组。</li>
	<li>该函数将返回写入到文件内数据的字节数。</li>
</ul>
</li>
	<li>怎么获取一个连接地址的文件名、目录名、文件扩展名
语法：pathinfo(path,options)
说明：
<ul>
	<li>返回dirname、basename、extension的值</li>
	<li>options可以指定返回那个值：PATHINFO_DIRNAME只返回dirname,PATHINFO_BASENAME只返回basename,PATHINFO_EXTENSION只返回extension。</li>
</ul>
</li>
	<li>怎么读取CSV文件，解析 CSV 字段
语法：fgetcsv(file,length,separator,enclosure)      <a href="http://www.w3school.com.cn/php/func_filesystem_fgetcsv.asp">参考</a>
说明：
<ul>
	<li>fgetcsv() 解析读入的行并找出 CSV 格式的字段，然后返回一个包含这些字段的数组。</li>
	<li>CSV 文件中的空行将被返回为一个包含有单个 null 字段的数组，不会被当成错误。</li>
</ul>
</li>
	<li>除了replace之外还有什么可以替换字符的方法呢？使用strtr就可以。
语法：strtr(string,from,to) <a href="http://www.w3school.com.cn/php/func_string_strtr.asp">参考</a>
说明：
<ul>
	<li>strtr() 函数转换字符串中特定的字符。</li>
	<li>如果 from 和 to 的长度不同，则格式化为最短的长度。</li>
</ul>
</li>
	<li>怎么获取定义的方法清单？
语法：get_defined_functions()
说明：
<ul>
	<li>获取当前页定义的方法及系统定义的方法清单</li>
</ul>
</li>
</ol>