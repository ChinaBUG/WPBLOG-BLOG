---
ID: 4299
post_title: >
  ECStore-Limited-user licenses are not
  supported due to concurrent users module
  start
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/11/ecstore-limited-user-licenses-are-not-supported-due-to-concurrent-users-module-start.html
published: true
post_date: 2015-11-19 14:30:46
---
上星期帮同事重装系统，不小心将自己的操作系统一起给格式化了，重装了一下系统，然后phpstudy没有重装直接使用，结果发现运行ECStore的时候居然会提示如下错误信息：

Limited-user licenses are not supported due to concurrent users module start...

好吧~不懂为啥，总的意思就是授权证书不支持在当前模块中使用，问了一下度娘，结果没有结果。

怎么办呢？

检查了一下授权证书，没错，重启了服务器也没错，一直这个错误，最后没办法，直接重装phpstudy，然后问题就好了，汗死了~^_^

记录一下呗~