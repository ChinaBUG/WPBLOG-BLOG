---
ID: 1513
post_title: >
  密码保护：ShopEX-二次开发-商品搜索时-对特价商品显示特价标志及如何设置特价商品
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-product-search-for-specials-show-how-to-set-up-special-signs-and-specials.html
published: true
post_date: 2011-07-07 21:03:27
---
<ul>
	<li>文章内容：ShopEX-商品搜索时-对特价商品显示特价标志及如何设置特价商品</li>
	<li>文章难度：中等</li>
	<li>修改难度：中等</li>
	<li>二次开发：可以</li>
</ul>
在使用ShopEX时，怎么设置一个商品有别与其他商品呢？如特价商品，客户怎么知道这是做特价的呢？特别是我在搜索的时候。

在ShopEX之中有一个标签功能，原先设计的理念是做什么的我不知道，不过我知道，这个功能可以帮我实现我需要的功能。

不多废话了，开始修改了~~

第一步，我们先准备一张图片，比如我做特价的，那么我就做一张图片，上面写着特字。文件名及存放路径：statics\ico_tj.gif，尺寸为20px*20px的。

第二步，我们需要修改主要程序，实现查找标签的功能：

在core\shop\controller\ctl.gallery.php之中找到

$this-&gt;pagedata['setting'] = $setting;
$this-&gt;pagedata['products'] = &amp;$aProduct;

在这两行代码中间添加下面代码（你问我为什么要在这边添加？看代码的相关性吧，不要提前就行）：

//扩展：增加在查询商品时，列表的图片上增加标志
//条件：后台设置商品时，需要设置标签(添加这个标签与为商品添加标签)
$tagM = $this-&gt;system-&gt;loadModel('system/tag');
$tagId = $tagM-&gt;gettagbyname( 'goods', '特价商品[标签]' );
$tagRel = array();
foreach($aProduct as $k =&gt; $v){
//存在标签则1,表示是特价商品
$states = $tagM-&gt;getTagRel( $tagId,$v['goods_id'] );
$aProduct[$k]['tagRel'] = empty($states) ? 0:1;
}
具体含义看注释，我想应该都能看懂吧？

程序修改好了，现在我们需要修改视图文件了，请找到

core\shop\view\gallery\type\grid.html与core\shop\view\gallery\type\list.html并打开

在最两个文件最上方输入：

&lt;style&gt;
/*
时间：2011.07.07
扩展：商品搜索时，需要显示是否为特价商品
来源：http://webdesignerwall.com/tutorials/css-decorative-gallery
*/
.photo {
margin: 0px;
position: relative;
width: 119px;
height: 160px;
float: left;
}
.photo img {
background: #fff;
border: solid 0px #ccc;
padding: 4px;
}
.photo span {
width: 20px;
height: 20px;
display: block;
position: absolute;
top: 12px;
left: 12px;
background: url(statics/ico_tj.gif) no-repeat;
}
&lt;/style&gt;

再在两个文件里面各自找到

&lt;img src="&lt;{$product.thumbnail_pic|default:$env.conf.site.default_thumbnail_pic|storager}&gt;" alt="&lt;{$product.name|escape:html}&gt;"/&gt;

这一行把这一样修改成下面的：

&lt;{if $product.tagRel=='1'}&gt;
&lt;div&gt;
&lt;span&gt;&lt;/span&gt;&lt;img src="&lt;{$product.thumbnail_pic|default:$env.conf.site.default_thumbnail_pic|storager}&gt;" alt="&lt;{$product.name|escape:html}&gt;"/&gt;
&lt;/div&gt;
&lt;{else}&gt;
&lt;img src="&lt;{$product.thumbnail_pic|default:$env.conf.site.default_thumbnail_pic|storager}&gt;" alt="&lt;{$product.name|escape:html}&gt;"/&gt;
&lt;{/if}&gt;

保存吧，这两个文件是可以二次开发的，不用保存在系统的core文件夹里的，把index这个函数整个复制出来保存为cct文件就可以到处发了噢。

清除缓存，然后就能看到效果了。

==

2011.07.08测试补充

在火狐里面浏览是正确的，但是在IE中却不正确，不正确表现在，火狐里面可以点击图片并打开详细页，而在IE中不行，却可以点击文字打开详细页。

检查发现时我把photo这个层定义为图片大小，结果给遮住了，汗，有这样的说法？？好吧，只能说是我的基础不好哈。

具体修改如下：

样式修改为

&lt;style&gt;
/*
时间：2011.07.07
扩展：商品搜索时，需要显示是否为特价商品
*/
.photo {
margin: 0px;
position:absolute;
left:0px;
top:0px;
}
&lt;/style&gt;

然后打开视图文件将上面修改的都换成

&lt;a target="_blank" href='&lt;{link ctl="product" act="index" arg0=$product.goods_id}&gt;'  style='&lt;{if $env.conf.site.thumbnail_pic_width !=0 &amp;&amp; $env.conf.site.thumbnail_pic_height !=0}&gt; width:&lt;{$env.conf.site.thumbnail_pic_width}&gt;px;height:&lt;{$env.conf.site.thumbnail_pic_height}&gt;px;&lt;{/if}&gt;'&gt;
&lt;{if $product.tagRel=='1'}&gt;
&lt;div&gt;&lt;img src="statics/ico_tj.gif" /&gt;&lt;/div&gt;
&lt;img src="&lt;{$product.thumbnail_pic|default:$env.conf.site.default_thumbnail_pic|storager}&gt;" alt="&lt;{$product.name|escape:html}&gt;"/&gt;
&lt;{else}&gt;
&lt;img src="&lt;{$product.thumbnail_pic|default:$env.conf.site.default_thumbnail_pic|storager}&gt;" alt="&lt;{$product.name|escape:html}&gt;"/&gt;
&lt;{/if}&gt;
&lt;/a&gt;

就可以了，在修改list.html时要注意一下，因为他的一个A标签还包含另外一部分内容。