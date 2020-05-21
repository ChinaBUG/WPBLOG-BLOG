---
ID: 1472
post_title: 'ShopEX-Parse error: syntax error, unexpected &#8216;}&#8217;'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-parse-error-syntax-error-unexpected.html
published: true
post_date: 2011-07-03 21:02:52
---
今天，在为ShopEX做二次开发时，在做一个数据库输出时，莫名其妙出现下面的提示：

出错提醒：

Parse error: syntax error, unexpected '}' in C:\PHPnow\htdocs\core\include_v5\pageFactory.php(487) : eval()'d code on line 1

好吧，多了一个“}”，我明白，我死命的把刚才修改过的PHP代码，一段段的去掉，一行行的检验，结果发现，都给删了，问题还在？！神奇哈~

好像还修改了HTML文件哈~去找看看~~~~结果招了数十遍，终于找到了偶的HTML文件中有单独的&lt;{/foreach}&gt;出现，开头呢？~

补上，这个该死的循环，问题解决~~

问题原因：可能在操作时，输入完代码，鼠标乱飘，给删了吧~~~

-----...-----

出错提醒：

Parse error: syntax error, unexpected ';' in /data/httpd/www.lcqymall.com/core/include_v5/pageFactory.php(487) : eval()'d code on line 1

问题原因：

主要是视图文件中的变量后面的“|”多写了一个，写成“||”了，改掉就好了~~