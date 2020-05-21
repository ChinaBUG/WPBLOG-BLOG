---
ID: 4124
post_title: 'ECStore二次开发日记之2.<{include}>加载处理你的view视图模板文件及$env全局变量的应用判断是否是后台管理'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-2-loaded-with-your-view-of-the-view-template-file-and-the-env-application-to-determine-whether-a-global-variable-is-admin.html
published: true
post_date: 2015-08-04 17:55:03
---
话说运营部的同事忽然希望在商品详情页，那些品牌类型下面增加一个广告区域，然后来放统一广告，就是详情上方的位置，怎么办呢？

<a href="http://blog.ipodmp.com/wp-content/uploads/2015/07/ecstore_201508051627.png"><img class="alignnone size-full wp-image-4141" src="http://blog.ipodmp.com/wp-content/uploads/2015/07/ecstore_201508051627.png" alt="ecstore_201508051627" width="1012" height="261" /></a>

这个位置的代码是在\app\b2c\view\site\product\tab\intro.html里面，我们要在这边增加一个区域来放广告的话，其实方法很多，比如：
<ol>
	<li>直接加上去；
简略操作：直接找到代码所在，直接将需要的内容代码写进去，保存，就可以了。</li>
	<li>使用一个PHP文件，然后包含进来；
简略操作：因为视图文件是HTML文件，没办法直接用PHP的require跟include方法，所以只能在控制器中包含，设置变量名，然后变量输出就可以了。</li>
	<li>增加一个模板文件来调用；
简略操作：在主题文件夹内新建文件，然后按照ECStore的语法来载入，这边两个分支，具体下面会说到。</li>
	<li>其他</li>
</ol>
当然，方法这么多，但是！如果我说一个需求的话，也许，大概，也就剩下一种方法了，最起码我是只知道一种，你懂得话可以分享一下。

运营部小妹要求这个地方还能可视化编辑。1、2方法都是不可用的，应该怎么做呢？

好吧，我们下面来解决这个问题，请随我一步步的来分析吧。

~

既然，要可以可视化编辑，那么必须这个模板文件要在themes主题文件夹内，要在当前模板文件夹内，要不是不可能编辑的（只要在主题文件夹里面是不行的，在ShopEX4.85系列是可以的）。

我们就在那边新建一个文件，这个文件请使用“页面管理，添加新页面，商品详情页”文件名为“adv”，文件为“商品详情页广告区域”，文件名会自动设置为“product-adv.html”，在编辑的时候我们会看到这个文件名字就叫做“商品详情页广告区域”。

然后，请在这个文件里面写一段话，什么内容随你喜欢，反正我是写着：“您好，这个是商品详情页广告区域”

然后，如果你的经验丰富的话也许就想起，ECStore有一个&lt;{include}&gt;标签，于是，你就在\app\b2c\view\site\product\tab\intro.html文件中的合适位置增加了如下代码：

&lt;div class="product-adv-area"&gt;
&lt;{include file='product-adv.html'}&gt;
&lt;/div&gt;

然后你会发现居然找不到模板文件，然后你是不是瞎眼了？你会去翻看调用的方法，你都会发现一个问题就是这个模板文件都是如下形式：
<ul>
	<li>&lt;{include file="finder/time_header.html" app="archive"}&gt;</li>
	<li>&lt;{include file="admin/goods/batch/batchEditDifferencePriceList.html"}&gt;</li>
	<li>&lt;{include file="product/category/view_row.html"}&gt;</li>
	<li>&lt;{include file="../view/admin/goods/detail/product_row.html"}&gt;</li>
	<li>&lt;{include file='site/gallery/type/grid.html'}&gt;</li>
	<li>......</li>
</ul>
瞎眼了吧，好像这些都是基于view文件夹目录结构的......不信就可以一个个找过去，不过那个很费劲，还不如直接查看源码呢，你觉得呢，经过我事先的查阅源码发现这个调用确实是系统自动从视图文件夹内调用文件，如果有定义二次开发目录的话就从二次开发目录中获取这个文件，如果存在的话。

是不是瞎眼了呢~难道这个标签不能用了？哪还有什么办法呢？我也不知道除了这个办法还有什么办法呢，怎么办呢？

哈~其实，通过查阅源码会发现这个方法本身是有参数的，而且这个参数是可以指定视图文件的来源，是视图还是模板文件？

将我们上面的代码换成下面的文件就会发现问题解决了：

&lt;div class="product-adv-area"&gt;
&lt;{include file='product-adv.html' is_theme=true}&gt;
&lt;/div&gt;

没错！增加 is_theme 这个参数，请记住这个值一定要不包含引号的，要不，没有效果不要问我，因为我一开始也被误导了，最后还是分析源码后才知道，这个参数一定不能带引号。如果你运气好的话，那当我没说。

现在你是不是想调用哪个就是哪个呢？

冲吧，少年~~

等等，话说解决了模板导入问题，但是，说好的可视化编辑呢？

来吧，聪明的小伙伴也许直接对“product-adv.html”文件，点击可视化编辑，然后，除了之前写的文字之外，其他都没有~

原因是，因为里面没有写挂件的坑，肯定是没效果的嘛~

然后，我们来添加一下挂件坑位：

&lt;{widgets id="ProductAdv" }&gt;

然后，你会发现还是没有啥反应，

有制作模板主题经验的朋友肯定骂我的，因为这个是自然的事情，我没有加载脚本库呗！

请记住！！！做模板需要可视化编辑的话，请一定要加载文件头跟文件尾，不加载肯定是没有办法可视化编辑的。

好了~我们修改一下代码，最后的代码如下：

&lt;{header}&gt;
&lt;{widgets id="ProductAdv" }&gt;
&lt;{footer}&gt;

保存，赶紧去体验一下可视化编辑吧~~

然后前台一看，瞎眼了吧~~要是这么简单的话，还需要我给你写日志嘛~~

是不是发现重复的两个页头，两个页尾呢？

痛苦吧~

原因就在于我们加载的页头跟页尾造成的。

有苦逼的人会说，没事，需要编辑的话，我增加上去页头跟页尾，然后显示的时候我去掉不就行了？

可以！这个确实能解决这个问题，难道我们没有更好的办法了吗？！

本来我也是这样子想的，想要偷懒一下嘛，后来想，这个对运营的同事有点挑战，如果不小心弄错了。。那完蛋的还是我们技术部呀，怎么办呢？

然后我想，这个肯定要做成能自动识别是前台还是后台，这样根据情况来显示，那么怎么识别是后台还是前台呢？

在ShopEX4.85系列上，因为设计的结构，所以他有一个常量是表明是前台还是后台的，在ECStore中我却找不到这个变量，难道就这么放弃？

不行！

我又找了一下，发现有一个变量是指示后台登陆地址的，使用看看呗，结果还是错误。

......

头痛了...一看表......已经快八点了~

然后眼光飘到地址栏上~看到编辑界面的好多参数，如下：

/index.php?app=site&amp;ctl=admin_theme_widget&amp;act=editor&amp;theme=MAX360buy&amp;file=index.html

好吧~

难道我不能根据这个参数来判断？！

可以！

好在ShopEX4.85跟ECStore都有提供一个超级的全局变量，那就是$env变量。

好吧，我们的代码现在可以改为如下：

&lt;{if $env.get.file == 'product-adv.html'}&gt;
&lt;{header}&gt;
&lt;{/if}&gt;
&lt;{widgets id="ProductAdv" }&gt;
&lt;{if $env.get.file == 'product-adv.html'}&gt;
&lt;{footer}&gt;
&lt;{/if}&gt;

然后，你会发现前台的错误解决了，天空又恢复一片的晴朗了~

你会了吗？
<blockquote><strong><em>重点提示：</em></strong>

<strong><em>如果，你没看到最后那么你没有看到我的提示，这个与我不想干，可能有的人会遇上，比如我这样倒霉的人噢。</em></strong>

<strong><em>请注意你的模板文件的编码，一定要是utf8的，要不显示的都是一片空白，没提示。</em></strong></blockquote>
附录：分析过程用到的代码所在

\ecstore\app\base\lib\component\compiler.php

tpl_compile_include($arguments, &amp;$object)

\ecstore\app\base\lib\render.php

_fetch_compile_include($app_id,$tmpl_file, $vars=null, $is_theme=false)