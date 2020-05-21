---
ID: 785
post_title: >
  为WordPress的常规选项添加一个选项
author: ChinaBUG
post_excerpt: >
  　　在WordPress当做CMS来使用时，难免需要增加一些设置选项，本文将分享如何在常规选项里面增加一个“关于企业”这一选项，恩，不用写代码噢，只要你会复制，其他都是WP自动完成的。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/09/wp-add-an-option-for-the-wordpress.html
published: true
post_date: 2010-09-09 17:37:43
---
　　在WordPress当做CMS来使用时，难免需要增加一些设置选项，本文将分享如何在常规选项里面增加一个“关于企业”这一选项，恩，不用写代码噢，只要你会复制，其他都是WP自动完成的。

　　首先，我们打开<strong>admin/options-general.php</strong>文件，在里面我们找到

&lt;tr valign="top"&gt;
&lt;th scope="row"&gt;&lt;label for="blogname"&gt;&lt;?php _e('Site Title') ?&gt;&lt;/label&gt;&lt;/th&gt;
&lt;td&gt;&lt;input name="blogname" type="text" id="blogname" value="&lt;?php form_option('blogname'); ?&gt;" /&gt;&lt;/td&gt;
&lt;/tr&gt;

　　然后复制（注意：这句代码是<span style="text-decoration: underline;">站点标题</span>的设置代码噢），黏贴到你需要添加新的选项的地方噢，我是直接黏贴到他的上方，即在站点标题上方增加一个选项。

　　然后把代码修改为

&lt;tr valign="top"&gt;
&lt;th scope="row"&gt;&lt;label for="<strong>aboutQY</strong>"&gt;<strong>关于企业</strong>&lt;/label&gt;&lt;/th&gt;
&lt;td&gt;&lt;textarea name="<strong>aboutQY</strong>" rows="10" cols="50" id="<strong>aboutQY</strong>"&gt;&lt;?php form_option(<strong>'aboutQY'</strong>); ?&gt;&lt;/textarea&gt;&lt;/td&gt;
&lt;/tr&gt;

　　也就改了记得选项，上面的加粗的地方时必改的地方噢。

　　然后我们再打开<strong>admin/options.php</strong>文件，在里面找到

$whitelist_options = array(
 'general' =&gt; array( 'blogname',

　　修改为

$whitelist_options = array(
 'general' =&gt; array( <strong>'aboutQY'</strong>, 'blogname',

　　即在原来基础上加上<strong>'aboutQY'</strong>这个<strong>ID</strong>噢，然后你就可以看到你的常规选项里面出现了一个关于企业的选项了。

　　当然，第一次使用时，你<span style="text-decoration: underline;">看到的是空的文本框</span>噢，因为这个选项本来就是没有数据的，现在你只需要填写并保存更改，你就能看到该选项自动保存数据啦。