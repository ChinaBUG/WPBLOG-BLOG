---
ID: 318
post_title: 模板层级
author: ChinaBUG
post_excerpt: >
  WordPress的模板文件像一个机器上的各个零部件，这些零部件共同合作，为WordPress网站生成各种类型的页面。有些模板（例如页眉模板文件和页脚模板文件）会在所有网页上出现，而其它模板只出现在特定条件下。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/01/template-hierarchy.html
published: true
post_date: 2010-01-15 15:15:06
---
<h2>模板层级</h2>
<h3>简介</h3>
WordPress的模板文件像一个机器上的各个零部件，这些零部件共同合作，为WordPress网站生成各种类型的页面。有些模板（例如页眉模板文件和页脚模板文件）会在所有网页上出现，而其它模板只出现在特定条件下。
<h4>本文主要内容</h4>
本文目的在于寻找下面这个问题的答案：
<blockquote>WordPress显示某一类型页面时，使用的是哪个（哪些）模板文件？</blockquote>
<h4>本文目标读者</h4>
自WordPress 1.5版引入主题功能后，模板的配置也越来越灵活。为了能够更好地开发WordPress的主题，我们有必要了解WordPress在显示网站不同页面时如何调用模板文件。如果需要定制已有的WordPress主题，本文也可以告诉你哪些主题模板文件需要修改。
<blockquote>WordPress提供了多种方式匹配模板与查询类型。WordPress主题开发人员也可以使用条件标签来决定生成特定页面时所使用的模板。有些WordPress主题可能只执行本文提到的部分模板文件，有些主题使用条件标签来加载其它模板文件。更多信息请阅读<a href="http://www.wordpress.la/codex-%E6%9D%A1%E4%BB%B6%E6%A0%87%E7%AD%BE.html">条件标签</a> 与<a href="http://www.wordpress.la/codex-WordPress%E4%B8%BB%E9%A2%98%E5%BC%80%E5%8F%91.html">主题开发</a>中的“<a href="http://www.wordpress.la/codex-WordPress%E4%B8%BB%E9%A2%98%E5%BC%80%E5%8F%91.html#query_based">基于查询的模板</a>”。</blockquote>
<h3>模板文件层级</h3>
<h4>基本理论</h4>
WordPress用查询字符串——网站上每个链接中所包含的信息——来决定显示页面时应使用的模板。

首先WordPress会将每个查询字符串与查询类型相匹配——例如，判断访问者所请求的页面类型（搜索页面，类别页面，主页等）。

然后按照WordPress模板层级规定的顺序以及主题中的模板文件是否可用选择模板并生成页面内容。

WordPress在当前主题文件中查找特定名称的模板文件，然后在相应查询区域下方列出的模板文件中选择首个匹配的模板文件。

除了最基本的index.php模板文件外，主题开发人员可以决定他们是否需要执行某个特定模板文件。如果WordPress不能找到与当前名称相匹配的模板文件，会自动跳转到模板层级的下一个文件名称进行查找。如果WordPress查找不到任何匹配模板文件，系统默认使用index.php文件（主题的主页模板文件）代替。
<h4>示例</h4>
假设某博客地址是http://example.com/wp/，并且有个访问者点击了网站上的一个类别页面，页面地址为http://example.com/wp/category/your-cat/，此时WordPress的任务是在当前主题文件中查找与类别ID相匹配的模板文件。如果类别ID为4，WordPress会查找名称为category-4.php的模板文件。如果该文件丢失，WordPress继续查找通用类别模板文件category.php。如果category.php文件也不存在，WordPress会查找通用存档模板archive.php。如果archive.php文件仍然不存在，WordPress会用主题的主模板文件index.php来代替所查找的文件。

如果访问者点击的是网站主页 http://example.com/wp/，WordPress首先判断网站是否存在静态首页，如果存在WordPress会根据页面模板层级加载静态首页；如果不存在静态首页，WordPress会查找home.php模板文件并用该文件生成用户所请求页面。若home.php文件丢失，WordPress会在当前主题文件中查找index.php文件并用该文件生成用户请求页面。
<h4>图示</h4>
WordPress根据模板层级调用各种模板文件以生成网站的不同页面。

<img src="http://www.wordpress.la/sites/default/files/600px-Template_Hierarchy.png" alt="" />
<h4>模板调用顺序</h4>
WordPress为不同查询类型调用模板文件，顺序如下：

<strong>查询主页时</strong>

1. home.php

2. index.php

<strong>查询某篇日志时</strong>

1. single.php

2. index.php

<strong>查询页面</strong><strong>时</strong>

WordPress页面：

1. pagetemplate.php——pagetemplate.php是页面模板分配给页面的模板文件

2. page.php

3. index.php

<strong>查询类别时</strong>

类别模板：

1. category-id.php——例如，若类别ID为6，WordPress查找category-6.php文件

2. category.php

3. archive.php

4. index.php

<strong>查询标签</strong><strong>时</strong>

标签模板：

1. tag-slug.php——例如，若标签的别名为sometag，WordPress会查找tag-sometag.php文件

2. tag.php

3. archive.php

4. index.php

<strong>查询作者时</strong>

作者模板：

1. author.php

2. archive.php

3. index.php

<strong>查询日期时</strong>

存档（日期）模板

1. date.php

2. archive.php

3. index.php

<strong>查询搜索结果时</strong>

创建搜索页面：

1. search.php

2. index.php

<strong>404页面未找到</strong>

生成404错误页面：

1. 404.php

2. index.php

<strong>查询附件时</strong>

附件模板：

1. image.php, video.php, audio.php, application.php以及其它MIME类型的第一部分

2. attachment.php

3. single.php

4. index.php