---
ID: 4291
post_title: >
  ECStore二次开发日记之10.后台模板编辑按钮边增加挂件坑的ID名
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/11/ecstore-development-diary-10-back-end-template-editing-button-pendant-id-additional.html
published: true
post_date: 2015-11-05 17:10:51
---
昨天，编辑同事说在后台编辑模板需要增加一个显示挂件坑ID的功能，如下图画圈圈的地方，这个功能确实很贴心，方便编辑人员定位源码所在位置，非常棒，今天抽空修改了一下，分享一下修改的地方吧。

<a href="http://blog.ipodmp.com/wp-content/uploads/2015/11/ecstore_templete.jpg"><img class="alignnone size-full wp-image-4292" src="http://blog.ipodmp.com/wp-content/uploads/2015/11/ecstore_templete.jpg" alt="ecstore_templete" width="248" height="328" /></a>

首先，我们打开
<ol>
	<li>/public/app/site/statics/js_mini/shopwidgets.min.js</li>
	<li>/public/app/wap/statics/js_mini/shopwidgets.min.js</li>
</ol>
随便打开一个就好，1是PC端的，2是手机端的。我们拿2来讲解吧，1的修改其实都一样，可能有一些变量名不一样，而实际的程序是一样的。

因为这个脚本文件是压缩的噢，我们需要先解压“美化”一下，我经常用“<a href="http://js.clicki.cc/">Javascript压缩、美化</a>”，推荐使用噢。

然后请找到大约40行checkEmptyDropPanel方法定义处：

在方法内我们可以看到

var b = (new Element("div.empty_drop_box")).set("html", '&amp;nbsp;&lt;button type="button" class="btn btn-add-widgets"&gt;&lt;span&gt;&lt;span&gt;&lt;i class="icon"&gt;&lt;/i&gt;添加挂件&lt;/span&gt;&lt;/span&gt;&lt;/button&gt;').inject(a);

这一行代码，只要修改为：

var b = (new Element("div.empty_drop_box")).set("html", '&amp;nbsp;&lt;button type="button" class="btn btn-add-widgets"&gt;&lt;span&gt;&lt;span&gt;&lt;i class="icon"&gt;&lt;/i&gt;添加挂件' + ( (a.get('base_id')&amp;&amp;a.get('base_id').length&gt;0)?' [ ' + a.get('base_id') + ' ] ':'') + '&lt;/span&gt;&lt;/span&gt;&lt;/button&gt;').inject(a);

保存，然后就可以实现我们上图中的下面的效果了~

再往下120行左右找到initDrags方法定义处，然后你会看到

h = $(h.target);

只需要在下面另起一行增加如下代码

if((WidgetEdit = h.getParent('.shopWidgets_panel')) &amp;&amp; WidgetEdit.get('base_id') &amp;&amp; WidgetEdit.get('base_id').length &gt; 0) c.getElement('.btn-edit-widgets').innerHTML = "编辑 [ " + WidgetEdit.get('base_id') + ' ]';

然后保存，效果实现了。

简单吧~~

&nbsp;