---
ID: 754
post_title: WordPress常用函数列表
author: ChinaBUG
post_excerpt: |
  取得时间：the_time
  分页：wp_list_pages
  文章列表：wp_list_categories
  文章代码：post_class
  显示目前文章的类别：the_category
  文章标题：the_title
  文章网址：the_permalink
  文章内容：the_content
  文章摘要：the_excerpt
  文章id：the_ID
  文章标签：the_tags
  文章作者：the_author
  文章昵称：the_author_nickname
  文章引用：trackback_url
  显示文章作者的描述：the_author_description
  显示文章作者的登录名：the_author_login
  显示文章作者的firstname（名）：the_author_firstname
  显示文章作者的（姓）：the_author_lastname
  显示文章作者的昵称：the_author_nickname
  显示文章作者的ID号：the_author_ID
  显示文章作者的电子邮箱：the_author_email
  显示文章作者的网站地址：the_author_url
  显示文章作者的网站地址：the_author_url
  以文章作者名为链接名：the_author_link
  显示文章作者的aim：the_author_aim
  显示文章作者的yim：the_author_yim
  显示文章作者已发表文章的篇数：the_author_posts
  显示一个链接到文章作者已发表文章列表的链接：the_author_posts_link
  显示blog作者列表：wp_list_authors("arguments")
  订阅rss：post_comments_feed_link("RSS 2.0")
  留言板：comments_template
  日历：get_calendar
  会员注册：wp_register
  会员登入注销：wp_loginout
  分页：<!–next page–>
  文章中断：<!–more–>
  相关联结：wp_list_bookmarks
  文章搜寻：get_search_form
  回首页：bloginfo("url")
  图片连结：bloginfo("template_directory")
  显示blog的标题：bloginfo("name")
  显示blog的网址：bloginfo("url")
  显示目前blog的网址：bloginfo("description")
  comments.php文件(评论)：comments_template
layout: post
permalink: >
  http://blog.ipodmp.com/2010/08/wordpress-list-of-commonly-used-functions.html
published: true
post_date: 2010-08-27 10:43:46
---
取得时间：
2009/05/8 17:06 &lt;?php the_time(’ Y/m/j G:i’) ?&gt;&lt;?php the_time(’Y m/j’) ?&gt;

分页：
&lt;?php wp_list_pages(); ?&gt;
&lt;?php wp_list_pages(’title_li=&lt;h2&gt;Page&lt;/h2&gt;’); ?&gt;

文章列表：
&lt;?php wp_list_categories(); ?&gt;

文章代码：
&lt;?php post_class() ?&gt;

显示目前文章的类别，以,分隔：
&lt;?php the_category(’,') ?&gt;
&lt;?php the_category(’separator’, ‘parents’ ); ?&gt;
’separator’分隔符  ，‘parents’ 阶层：‘multiple’ 分开类别 ’single’  链接类别(预设)
网络知识库(blog) &gt;wordpress 视觉大师     ： &lt;?php the_category(’ &amp;gt;’, ‘multiple’ ); ?&gt;
网络知识库(blog) &gt;wordpress 视觉大师 ： &lt;?php the_category(’ &amp;gt;’, ’single’ ); ?&gt;

文章标题：
&lt;?php the_title(); ?&gt;

文章网址：
&lt;?php the_permalink(); ?&gt;

文章内容：
&lt;?php the_content(); ?&gt;

文章摘要：
&lt;?php the_excerpt(); ?&gt;

文章id：
&lt;?php the_ID(); ?&gt;

文章标签：
&lt;?php the_tags( ”, ‘, ‘, ”); ?&gt;

文章作者：
&lt;?php the_author(); ?&gt;
&lt;a href=”&lt;?php the_author_url(); ?&gt;”&gt;&lt;?php the_author_nickname(); ?&gt;&lt;/a&gt;

文章昵称：
&lt;?php the_author_nickname(); ?&gt;

文章引用：
&lt;?php trackback_url(); ?&gt;

显示文章作者的描述（作者个人资料中的描述）：
&lt;?php the_author_description(); ?&gt;

显示文章作者的登录名：
&lt;?php the_author_login(); ?&gt;

显示文章作者的firstname（名）：
&lt;?php the_author_firstname(); ?&gt;

显示文章作者的（姓）：
&lt;?php the_author_lastname(); ?&gt;

显示文章作者的昵称：
&lt;?php the_author_nickname(); ?&gt;

显示文章作者的ID号：
&lt;?php the_author_ID(); ?&gt;

显示文章作者的电子邮箱：
&lt;?php the_author_email(); ?&gt;
&lt;a href=”mailto:&lt;?php echo antispambot(get_the_author_email()); ?&gt;”&gt;email&lt;/a&gt;

显示文章作者的网站地址：
&lt;?php the_author_url(); ?&gt;

显示文章作者的网站地址：
&lt;?php the_author_url(); ?&gt;
&lt;a href=”&lt;?php the_author_url(); ?&gt;”&gt;&lt;?php the_author_url(); ?&gt;&lt;/a&gt;
&lt;?php if (get_the_author_url()) { ?&gt;&lt;a href=”&lt;?php the_author_url(); ?&gt;”&gt;&lt;?php the_author(); ?&gt;&lt;/a&gt;&lt;?php } else { the_author(); } ?&gt;

以文章作者名为链接名：
&lt;?php the_author_link(); ?&gt;

显示文章作者的aim：
&lt;?php the_author_aim(); ?&gt;

显示文章作者的yim：
&lt;?php the_author_yim(); ?&gt;

显示文章作者已发表文章的篇数：
&lt;?php the_author_posts(); ?&gt;

显示一个链接到文章作者已发表文章列表的链接：
&lt;?php the_author_posts_link(); ?&gt;

显示blog作者列表：
&lt;?php wp_list_authors(’arguments’); ?&gt;

订阅rss：
&lt;?php post_comments_feed_link(’RSS 2.0′); ?&gt;

留言板：
&lt;?php comments_template(); ?&gt;

日历：
&lt;?php get_calendar(); ?&gt;

会员注册：
&lt;?php wp_register(); ?&gt;

会员登入注销：
&lt;?php wp_loginout(); ?&gt;

分页：
&lt;!–next page–&gt;

文章中断：
&lt;!–more–&gt;

相关联结：
&lt;?php wp_list_bookmarks(); ?&gt;

文章搜寻：
&lt;?php get_search_form(); ?&gt;

回首页：
&lt;a href=”&lt;?php bloginfo(’url’); ?&gt; ” title=”回首页”&gt;回首页&lt;/a&gt;

图片连结：
&lt;img src=”&lt;?php bloginfo(’template_directory’); ?&gt;/images/logo.jpg” /&gt;

显示blog的标题：
&lt;?php bloginfo(’name’); ?&gt;

显示blog的网址：
&lt;?php bloginfo(’url’); ?&gt;

显示目前blog的网址：
&lt;?php bloginfo(’description’); ?&gt;

comments.php文件(评论)：
&lt;?php comments_template(); ?&gt;