---
ID: 4260
post_title: >
  ECStore二次开发日记之9.头部的系统级别模板到底有几个？
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/10/ecstore-development-diary-9-how-many-head-of-system-level-template.html
published: true
post_date: 2015-10-19 19:55:18
---
我们在做ECStore模板的时候都是直接调用&lt;{header}&gt;标签调用头部的系统模板文件，但是，你知道这个header标签代表几个文件吗？！

以前，我认为是调用同一个文件，现在，今天晚上，我确定了，原来是两个，主页一个，其他页面一个。

今天有一个需求需要在头部增加一串代码，因为模板是以前人设计的，都是没有分模块调用的，要是增加个代码，全部的模板都是要修改的，累死了，偷懒一下，就直接修改：appb2cviewsitecommonheader.html这个文件。

提示：这个文件夹内的模板都是系统需要调用的公用文件，header.html是头部，footer.html是尾部，其他的自己研究吧。

修改之后刷新就会发现其他的页面都修改了，就是主页没有修改，因为主页使用CDN了，还以为是CDN系统出现错误，所以就一下午在折腾，最后发现不是，一查源码，才发现，原来我错了。

控制&lt;{header}&gt;的内容的程序位于/app/b2c/lib/site/view/helper.php内，所以我们查看就会发现，这边做了特别处理，首页加载的是b2c应用中的site/common/resources.html文件而其他页面调用的则是site/common/header.html，然后，修改一下，终于收工回家了~~^_^