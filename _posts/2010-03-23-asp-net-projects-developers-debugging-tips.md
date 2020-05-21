---
ID: 365
post_title: ASP.NET项目懒人开发-调试技巧
author: ChinaBUG
post_excerpt: |
  　　网站刚上线，但是架不住测试的人多，自己要修改文件被占用，比如数据库被锁定怎么办噢？
  　　虚拟主机，好像不给这个权限，停止整台服务器，好像太复杂了噢，有简单的吗？
  　　有！就是使用 app_offline.htm 文件，我们只需要新建一个文件名字叫app_offline.htm的，并放在根目录下，你就会发现，服务器不能用了！
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/asp-net-projects-developers-debugging-tips.html
published: true
post_date: 2010-03-23 12:00:11
---
　　网站刚上线，但是架不住测试的人多，自己要修改文件被占用，比如数据库被锁定怎么办噢？

　　难不成停止IIS吗？

　　虚拟主机，好像不给这个权限，停止整台服务器，好像太复杂了噢，有简单的吗？

　　有！就是使用 <strong>app_offline.htm</strong> 文件，我们只需要新建一个文件名字叫app_offline.htm的，并放在根目录下，你就会发现，服务器不能用了！

　　当然，这个文件可以空的也可以写进一些提示语言，比如：本站正在维护中等等信息。

　　要恢复运作，只需要删除这个文件或者改名即可恢复，简单，实用噢。