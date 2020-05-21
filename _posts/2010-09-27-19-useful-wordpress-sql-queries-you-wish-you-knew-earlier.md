---
ID: 810
post_title: 19条WordPress SQL查询语句
author: ChinaBUG
post_excerpt: >
  不要轻易折腾你的SQL。但有的时候
  ，使用SQL能大大提高你的办事效率，或者有的时候，你不得不用SQL来改变一些东西，比如把让你老是觉得不安全的admin这几个字换成其它的，比如你想收集所有留言者的邮箱地址来实现你的垃圾营销目的，比如我想把帕兰映像站内所有含链接的留言完全删掉！
layout: post
permalink: >
  http://blog.ipodmp.com/2010/09/19-useful-wordpress-sql-queries-you-wish-you-knew-earlier.html
published: true
post_date: 2010-09-27 10:13:53
---
不要轻易折腾你的SQL。但有的时候 ，使用SQL能大大提高你的办事效率，或者有的时候，你不得不用SQL来改变一些东西，比如把让你老是觉得不安全的admin这几个字换成其它的，比如你想收集所有留言者的邮箱地址来实现你的垃圾营销目的，比如我想把帕兰映像站内所有含链接的留言完全删掉！

本文为大家介绍<strong>19条<a href="http://paranimage.com/category/apps/wordpress/">wordpress</a> SQL查询</strong>，你可能啥时候就会需要到。

<strong>使用方法：</strong>

进入你主机的phpmyadmin，选择你的WordPress数据，点击SQL选项卡，在文本框中输入SQL查询语句，执行！

<strong>高度注意：</strong>

在每次执行SQL语句前，请<strong><span style="color: #ff0000;"><span style="text-decoration: line-through;">勿必</span>（务必）</span>备份你的WordPress数据库</strong>。
<h3>1. 删除所有未使用的标签</h3>
<pre><code>DELETE a,b,c
FROM wp_terms AS a
LEFT JOIN wp_term_taxonomy AS c ON a.term_id = c.term_id
LEFT JOIN wp_term_relationships AS b ON b.term_taxonomy_id = c.term_taxonomy_id
WHERE c.taxonomy = 'post_tag' AND c.count = 0</code></pre>
<h3>2. 删除所有文章修订版本(Revisions)以及它们的Meta数据</h3>
<pre><code>DELETE a,b,c
FROM wp_posts a
LEFT JOIN wp_term_relationships b ON (a.ID = b.object_id)
LEFT JOIN wp_postmeta c ON (a.ID = c.post_id)
WHERE a.post_type = 'revision'</code></pre>
<h3>3. 更改WordPress地址和首页地址</h3>
<pre><code>UPDATE wp_options
SET option_value = replace(option_value, 'http://www.旧网址.com', 'http://www.新网址.com')
WHERE option_name = 'home' OR option_name = 'siteurl'</code></pre>
<h3>4. 更改文章的GUID</h3>
<pre><code>UPDATE wp_posts
SET guid = REPLACE (guid, 'http://www.旧网址.com', 'http://www.新网址.com')</code></pre>
 
<h3>5. 更改正文中的链接地址</h3>
<pre><code>UPDATE wp_posts
SET post_content = REPLACE (post_content, 'http://www.旧网址.com', 'http://www.新网址.com')</code></pre>
<h3>6. 更新文章的Meta值</h3>
<pre><code>UPDATE wp_postmeta
SET meta_value = REPLACE (meta_value, 'http://www.旧网址.com', 'http://www.新网址.com'</code></pre>
<h3>7. 重设Admin密码</h3>
<pre><code>UPDATE wp_users
SET user_pass = MD5( 'new_password' )
WHERE user_login = 'admin'</code></pre>
<h3>8. 重设admin的用户名</h3>
<pre><code>UPDATE wp_users
SET user_login = 'newname'
WHERE user_login = 'admin'</code></pre>
<h3>9. 将作者a的文章全部转移到作者b</h3>
<pre><code>UPDATE wp_posts
SET post_author = 'b'
WHERE post_author = 'a'</code></pre>
<h3>10. 删除文章的meta标签</h3>
<pre><code>DELETE FROM wp_postmeta
WHERE meta_key = 'your-meta-key'
</code></pre>
<h3>11. 导出所有评论中的邮件地址</h3>
<pre><code>SELECT DISTINCT comment_author_email
FROM wp_comments</code></pre>
<h3>12. 删除所有的Pingback</h3>
<pre><code>DELETE FROM wp_comments
WHERE comment_type = 'pingback'</code></pre>
<h3>13. 删除所有的垃圾评论</h3>
<pre><code>DELETE FROM wp_comments
WHERE comment_approved = 'spam'</code></pre>
<h3>14. 禁用所有激活的插件</h3>
<pre><code>UPDATE wp_options
SET option_value = ''
WHERE option_name = 'active_plugins'</code></pre>
<h3>15. 罗列所有未使用的Meta标签</h3>
<pre><code>SELECT *
FROM wp_postmeta pm
LEFT JOIN wp_posts wp ON wp.ID = pm.post_id
WHERE  wp.ID IS NULL</code></pre>
<h3>16. 关闭旧文章的留言</h3>
<pre><code>UPDATE wp_posts
SET comment_status = 'closed'
WHERE post_date &lt; '2009-01-01' AND post_status = 'publish'</code></pre>
<h3>17. 更新留言者的网址</h3>
<pre><code>UPDATE wp_comments
SET comment_author_url = REPLACE( comment_author_url, 'http://旧网址.com', 'http://新网址.com' )</code></pre>
<h3>18. 更新正文内所有的’target=”_blank”‘为’rel=”nofollow”‘</h3>
<pre><code>UPDATE wp_posts
SET post_content = REPLACE (post_content, 'target="_blank',  'rel="nofollow')</code></pre>
<h3><del datetime="2010-05-12T07:47:16+00:00">19. 删除所有含链接的留言(勿用)</del></h3>
<pre><code><del datetime="2010-05-12T07:47:16+00:00">DELETE FROM wp_comments
WHERE comment_content LIKE "%&lt;a href=%" AND comment_type = ''</del></code></pre>
<pre><code><del datetime="2010-05-12T07:47:16+00:00"></del></code></pre>
<pre><code><del datetime="2010-05-12T07:47:16+00:00"></del></code></pre>
<pre><code><del datetime="2010-05-12T07:47:16+00:00"></del></code></pre>
<h4>参考文章:</h4>
<ul>
	<li><a href="http://www.onextrapixel.com/2010/01/30/13-useful-wordpress-sql-queries-you-wish-you-knew-earlier/" target="_blank">13 Useful WordPress SQL Queries You Wish You Knew Earlier</a></li>
	<li><a href="http://www.catswhocode.com/blog/wordpress-10-life-saving-sql-queries" target="_blank">WordPress : 10+ life saving SQL queries</a></li>
</ul>