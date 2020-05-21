---
ID: 1880
post_title: >
  ShopEX-Fatal error:Call to a member
  function actions() on a non-object
author: ChinaBUG
post_excerpt: |
  文件目录：\plugins\actions
  二次开发：可
  特别注意：不可以在这个文件夹内包含无效的文件，即如果有备份文件习惯的请不要把备份文件放在这个文件夹内，否则将出现如下错误：
  Fatal error: Call to a member function actions() on a non-object in
  data/httpd/lcqymall.com/core/admin/controller/system/ctl.trigger.php on line 69
  目录作用：
  网店机器人的三种类型处理程序。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/01/shopex-fatal-errorcall-to-a-member-function-actions-on-a-non-object.html
published: true
post_date: 2012-01-04 12:52:02
---
最近在操作工具箱网店机器人的时候，不知道为啥，就是不能添加老是提示这个信息，不让打开设置界面。

Fatal error: Call to a member function actions() on a non-object in /data/httpd/lcqymall.com/core/admin/controller/system/ctl.trigger.php on line 69

回想一下，这个程序一直以后都是好的，能设置的，之前是好的，为什么现在就变成不行了呢？

不管了，一步步调试吧。

结果本地服务器却正常！晕死~~

一步步的输出~一次次上传到服务器，终于有结果了，得出结果是这个函数调用的内容为空！就是没东西嘛~~为啥？

不知道，调试到此就找不到问题了。

没事，我就打开插件管理界面，一看，奇怪了，里面出现好多奇怪的插件噢，文件名是偶的插件名的备份名字，去目录里面一看，还真的存在这么多文件。

难道是这个问题？

删除一试，问题解决！汗~~~

最终得出结论，ShopEX的一些文件夹比较特殊的，千万不要随便在里面随意的备份，且吧备份文件放在目录里面要不会被当做有效的文件给使用了。

~~~~~~~~~~~~~~~~

文件目录：\plugins\actions

二次开发：可

特别注意：不可以在这个文件夹内包含无效的文件，即如果有备份文件习惯的请不要把备份文件放在这个文件夹内，否则将出现如下错误：

<em>Fatal error: Call to a member function actions() on a non-object in</em>

<em>data/httpd/lcqymall.com/core/admin/controller/system/ctl.trigger.php on line 69</em>

目录作用：

网店机器人的三种类型处理程序。

<a href="http://blog.ipodmp.com/wp-content/uploads/2012/01/err01.png"><img class="alignnone size-medium wp-image-1883" title="err01" src="http://blog.ipodmp.com/wp-content/uploads/2012/01/err01-300x112.png" alt="" width="300" height="112" /></a>

<a href="http://blog.ipodmp.com/wp-content/uploads/2012/01/err02.png"><img class="alignnone size-medium wp-image-1884" title="err02" src="http://blog.ipodmp.com/wp-content/uploads/2012/01/err02-300x143.png" alt="" width="300" height="143" /></a>