---
ID: 172
post_title: >
  WordPress域名更换后日志内链接替换
author: ChinaBUG
post_excerpt: >
  上星期把www.fobg.cn换成现在的域名www.iPodMp.com，结果发现后台进不去，不仅如此，连帖子都看不到，哎，上网找了半天才知道要修改一下数据库，收藏一下吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/wordpress-domain-link-replacement.html
published: true
post_date: 2009-08-17 15:36:39
---
　　上星期把<a href="http://www.fobg.cn">www.fobg.cn</a>换成现在的域名<a href="http://www.iPodMp.com">www.iPodMp.com</a>，结果发现后台进不去，不仅如此，连帖子都看不到，哎，上网找了半天才知道要修改一下数据库，收藏一下吧。

　　解决方法：

　　登陆phpmyadmin，打开WordPress的数据库，点击SQL标签，输入以下代码：

　　1、解决wordpress后台登陆问题：

UPDATE 'wp_options' SET 'option_value' = replace('option_value','www.fobg.cn','www.iPodMP.com') WHERE 'option_value' LIKE '%www.fobg.cn%'

　　PS：请将代码内的网址改为你的。

　　2、解决wordpress帖子打不开问题：

UPDATE 'wp_posts' SET 'post_content' = replace('post_content','www.fobg.cn','www.iPodMP.com') WHERE 'post_content' LIKE '%www.fobg.cn%'

　　PS：请将代码内的网址改为你的。

----------

【修订】- 2010.01.20

　　前几天我自己启用Blog的二级域名，再次使用这些代码，发现老是出现错误，错误信息如下：

错误
SQL 查询:

UPDATE 'wp_posts' SET 'post_content' = replace( 'post_content', 'www.iPodMP.com', 'blog.iPodMP.com' ) WHERE 'post_content' LIKE '%www.iPodMP.com%'

MySQL 返回：

<span style="color: #ff0000;">#1064 - You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near ''wp_posts' SET 'post_content' = replace('post_content','www.iPodMP.com','blog.iP' at line 1</span>

　　经过我的测试，终于解决这个问题，只需要将代码中的字段名两边的引号去除即可~~！即

UPDATE wp_posts SET post_content = replace(post_content, 'www.iPodMP.com', 'blog.iPodMP.com' ) WHERE post_content LIKE '%www.iPodMP.com%'