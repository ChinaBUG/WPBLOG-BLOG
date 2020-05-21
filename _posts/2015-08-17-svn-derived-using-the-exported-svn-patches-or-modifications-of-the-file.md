---
ID: 4187
post_title: >
  SVN-利用SVN导出补丁包或者导出修改之后的文件
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/svn-derived-using-the-exported-svn-patches-or-modifications-of-the-file.html
published: true
post_date: 2015-08-17 17:10:06
---
序

自从使用SVN开始，一直有个烦恼的事情在心间：每次一个功能开发完毕，因为修改的文件较多，结果还要把这些文件挑出来，并按照目录结构拷贝到另外一个SVN上，然后才更新到生产环境中。

对SVN一直不熟悉，只懂得用就好啦，但是总不能每次都这么的痛苦吧，找了一下，还好有解决方案的。

文

我一直在找利用SVN导出补丁包的方法，在网上找了好久都没有找到，或者回答的不能让我满意，但是看到一些开源程序的产品发布方式，我觉得一定是存在这种工具或者方法的。

我一开始以为是和.diff的创建和应用有关，但.diff其实只能针对没提交之间的代码有效，经过长时间的探索发现有一个很简单的方法可以实现导出补丁包。

首先,需要的工具是SVN的windows客户端TortoiseSVN，然后需要右键打开版本分支图（TortoiseSVN → Revision Graph...），然后按住ctrl选中2个版本，这样我们可以导出这两个版本之间的补丁包。

<em><strong>注意：一定要先点小版本号，再点大版本号，否则就是降级补丁了。</strong></em>

然后右键点击比较版本差异（<span lang="EN-US">Compare revisions</span>），然后在弹出的对话框中，全选里面的文件列表，然后右键点击导出选中项到一个文件夹，这个文件夹里就是补丁包了。<em> 要注意的是里面的带删除颜色标记的都不可以选择</em>，否则就乱了，或者分开导出，删除标记部分导出为对外宣布的此版本废弃的文件列表。

&nbsp;

<a href="http://blog.csdn.net/alicehyxx/article/details/4310602">利用TortoiseSVN自动对比文件差异</a>

<a href="http://www.cnblogs.com/QLeelulu/archive/2009/12/09/1619927.html">SVN导出两个版本之间的差异文件</a>

<a href="http://blog.sina.com.cn/s/blog_5d514b780100g8vx.html">SVN制作补丁包的方法</a>