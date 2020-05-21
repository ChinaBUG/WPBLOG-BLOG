---
ID: 2685
post_title: >
  jQuery-prettyPhoto非常棒的一个图片浏览弹窗插件(类似Lightbox)
author: ChinaBUG
post_excerpt: |
  prettyPhoto是jQuery的一个图片浏览弹窗插件，类似于Lightbox插件，他不仅支持图片，还支持视频、FLASH影片、YouTube影片、内置页框和AJAX等。非常棒的插件，关键是简单实用。
  对我来说比较好的是浏览器兼容方面的，他支持的版本很低很低：
  “Firefox 3.0+ Google Chrome 10.0+ Internet Explorer 6.0+ Safari 3.1.1+ Opera 9+”
  最值得推荐的是他的使用真的很简单实用！最简单的调用如下：
  <script type="text/javascript" charset="utf-8">
  $(document).ready(function(){
  $("a[rel^='prettyPhoto']").prettyPhoto();
  });
  </script>
  然后再需要的链接上增加rel=”prettyPhoto”这句代码即可，当然，相册集的话则是rel=”prettyPhoto[....]”这样子的形式哈，具体请看“prettyPhoto documentation”
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/jquery-prettyphoto-great-a-picture-browser-popup-plugin-like-lightbox.html
published: true
post_date: 2013-10-31 14:32:31
---
prettyPhoto是jQuery的一个图片浏览弹窗插件，类似于Lightbox插件，他不仅支持图片，还支持视频、FLASH影片、YouTube影片、内置页框和AJAX等。非常棒的插件，关键是简单实用。

对我来说比较好的是浏览器兼容方面的，他支持的版本很低很低：

“Firefox 3.0+ Google Chrome 10.0+ Internet Explorer 6.0+ Safari 3.1.1+ Opera 9+”

最值得推荐的是他的使用真的很简单实用！最简单的调用如下：
<pre>&lt;script type="text/javascript" charset="utf-8"&gt;
  $(document).ready(function(){
    $("a[rel^='prettyPhoto']").prettyPhoto();
  });
&lt;/script&gt;</pre>
然后再需要的链接上增加rel=”prettyPhoto”这句代码即可，当然，相册集的话则是rel=”prettyPhoto[....]”这样子的形式哈，具体请看“<a href="www.no-margin-for-errors.com/projects/prettyphoto-jquery-lightbox-clone/documentation">prettyPhoto documentation</a>”

市面上的很多同类的插件，不是只能统一调用就是可定制性不强......

如果有使用过这类插件的朋友都知道的，真的有时候客户的需求很奇怪，CMS系统的莫名其妙，常常让我们需要对插件的效果做一定的定制，但是实际上却不得不一个个的找到功能差不多的来替换。

也许你不理解我在说什么，这边说说我在使用过程中的一个场景吧，如果你有同样的需求的话，请直接使用吧^_^
<blockquote>在使用aspcms这套CMS系统时，客户站内的经典案例栏目是使用prettyPhoto插件的。

客户站网址：www.xmlongkai.com/lkzs

程序设计：

输出相册的缩略图然后通过点击缩略图触发整个相册集，但是，会出现缩略图这一张图片噢，客户不希望点击之后会出现缩略图。想的我的头发都白了，没办法呀~原因就是使用prettyPhoto的时候就一句代码：

$("a[rel^='prettyPhoto']").prettyPhoto();

然后点击任何一个链接都会自动处理成相册集，里面没有针对改变起始展示图片顺序的参数。</blockquote>
看了上面实例场景的描述不知道您明白我的意思？像这样子的场景相信经常性的出现，但是其他的一些插件都是不能满足的，可能是我不懂得这些吧，下载了好多测试一下都是不能达到这个效果。有谁知道的话请留言，谢谢。

那么怎么解决上面的问题呢？

答案是：prettyPhoto的API接口调用！

以前，要解决这个问题只能修改比较符合需求的插件，然后实现我们需要的这个效果，现在嘛就不用了。

下面是prettyPhoto的API接口调用实例：

$.prettyPhoto.open('images/fullscreen/image.jpg','Title','Description');
$.prettyPhoto.changePage('next');
$.prettyPhoto.changePage('previous');
$.prettyPhoto.close();

你还可以这样子调用：
<pre>api_images = ['images/fullscreen/image1.jpg','images/fullscreen/image2.jpg','images/fullscreen/image3.jpg'];
api_titles = ['Title 1','Title 2','Title 3'];
api_descriptions = ['Description 1','Description 2','Description 3']
$.prettyPhoto.open(api_images,api_titles,api_descriptions);</pre>
从这边我们可以将我们需要的图片按照我们需要的规范来分类调用了，虽然这样子看起来很复杂哈~

当然，如果有的人想知道说，我使用API之后还想要定制起始的展示图片顺序怎么办？

通过分析prettyPhoto的源码我们会发现，其实作者都考虑到了~

你只需要用下面的代码即可定义起始的展示图片了：

$.prettyPhoto.changePage(<span style="color: #ff0000;">N</span>);

注意括号内的N噢，这个值是您需要开始播放的图片的序列，比如从第二张开始展示，则设置为：

$.prettyPhoto.changePage(2);

好了~~需要查看具体的应用的同学请移驾我的客户站观看吧