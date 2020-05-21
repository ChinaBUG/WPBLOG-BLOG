---
ID: 4195
post_title: >
  ECStore二次开发日记之6.为什么后台模板编辑挂件没办法选择模板呢？
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-6-why-back-end-template-editing-pendant-cant-choose-template.html
published: true
post_date: 2015-08-20 11:46:19
---
<p>
	记得在ShopEX4.85系列，每一个挂件都是可以选择使用的模板视图的，但是，目前所用的这套ECStore系统居然！！！挂件不能选择模板文件！！
</p>

<p>
	怎么办？！
</p>

<p>
	打开app &gt; site &gt; view &gt; admin &gt; theme &gt; widget &gt; widget_header.html文件，然后我们就会看到一个莫名其妙的代码：
</p>

<pre>
&lt;{if $basic_config==&#39;on&#39;}&gt;
&lt;div class=&quot;tableform&quot; style=&quot;display:none;&quot;&gt;
&nbsp;&nbsp;&nbsp; &lt;div class=&quot;division&quot;&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;table width=&quot;100%&quot; cellspacing=&quot;0&quot; cellpadding=&quot;0&quot;&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;tr&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;th&gt;&lt;label for=&quot;x-wcinput-01&quot;&gt;&lt;{t}&gt;版块标题：&lt;{/t}&gt;&lt;/label&gt;&lt;/th&gt;&lt;td&gt;&lt;input id=&quot;x-wcinput-01&quot; name=&quot;__wg[title]&quot; value=&quot;&lt;{$widgets_title}&gt;&quot; /&gt;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;th&gt;&lt;label&gt;&lt;{t}&gt;版块模板：&lt;{/t}&gt;&lt;/label&gt;&lt;/th&gt;&lt;td&gt; &lt;select name=&quot;__wg[tpl]&quot;&gt;
&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;{html_options options=$widget_editor.tpls selected=$widgets_tpl}&gt;
&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&lt;/select&gt;&lt;/td&gt;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &lt;/tr&gt;</pre>

<p>
	看到display:none;了没~这个证明不管是否开启基础设置，都是不显示的，不知道是我这个版本的人改的还是商派那些傻孩子改的。把这个去掉，然后，就发现可以选择模板了。
</p>

<p>
	好简单~却让人烦躁，越来越发现商派在这个人性化方面做得越来越差了~不知道还能撑多久呢~
</p>

<p>
	&nbsp;
</p>