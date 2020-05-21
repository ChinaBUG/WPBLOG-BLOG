---
ID: 2644
post_title: >
  ShopEX二次开发DIY日记5之前台商品列表页筛选保留选中分类,品牌分类,规格及扩展属性值
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  引言
  老早在ShopEX官方论坛看到一则广告噢，说的就是商品列表页筛选保留选中分类,品牌,规格及扩展属性值的问题，原帖地址 [485/易开店]【showker出品】shopex列表页筛选可保留选中分类,品牌,规格及扩展属性值 可打叉去掉选中 请各位小伙伴们自己去看说明哈。
  我们这次DIY日记的目标就是打造这样的一个功能~当然，简单版的哈。
  怎么保留选中的商品分类？商品品牌？商品规格？还有扩展值？
  要实现这个功能我们需要了解的是这些数据是怎么来的，通过分析ctl.gallery.php文件我们可以发现这些数据经过一次次的弯子处理之后汇聚成如下变量：$selector、$SpecFlatList、$searchSelect。
  我们在文件core\shop\view\gallery\selector\default.html之中就可以很明显的看出这些变量的调用情况。
  我们先来查看一下变量$selector的代码噢，你会发现：（下面是原代码，请根据实际代码作分析）
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-5-list-screening-hold-selected-categories-brand-specifications-and-extended-attribute-value.html
published: true
post_date: 2013-10-18 15:24:14
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

引言

老早在ShopEX官方论坛看到一则广告噢，说的就是商品列表页筛选保留选中分类,品牌,规格及扩展属性值的问题，原帖地址 <a href="http://bbs.shopex.cn/read.php?tid-234024-page-e.html">[485/易开店]【showker出品】shopex列表页筛选可保留选中分类,品牌,规格及扩展属性值 可打叉去掉选中</a> 请各位小伙伴们自己去看说明哈。

我们这次DIY日记的目标就是打造这样的一个功能~当然，简单版的哈。

怎么保留选中的商品分类？商品品牌？商品规格？还有扩展值？

要实现这个功能我们需要了解的是这些数据是怎么来的，通过分析ctl.gallery.php文件我们可以发现这些数据经过一次次的弯子处理之后汇聚成如下<a href="#var3">变量</a>：$selector、$SpecFlatList、$searchSelect。

我们在文件core\shop\view\gallery\selector\default.html之中就可以很明显的看出这些变量的调用情况。

我们先来查看一下变量$selector的代码噢，你会发现：（下面是原代码，请根据实际代码作分析）

&lt;{if $selector}&gt;
&lt;div id="selector_contents"&gt;
&lt;table width="100%"&gt;
&lt;{if $selector.ordernum}&gt;
&lt;{foreach from=$selectorExd item="column" key=column_id}&gt;
&lt;{if count($column.options)&gt;0 &amp;&amp; !$column.value}&gt;
&lt;tr&gt;
&lt;td style="padding-right:6px; width:72px; white-space:nowrap"&gt;&lt;{$column.name}&gt;：&lt;/td&gt;
&lt;td style=" border-bottom:1px solid #eee; line-height:22px;"&gt;&lt;{foreach from=$column.options item="item" key=value}&gt;&lt;a href="&lt;{selector key=$column_id value=$value}&gt;"&gt;&lt;{$item}&gt;&lt;/a&gt;&lt;{/foreach}&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;{/if}&gt;
&lt;{/foreach}&gt;
&lt;{foreach from=$selector.ordernum item=pord key=key}&gt;
&lt;{if count($selector.$pord.options)&gt;0 &amp;&amp; !$selector.$pord.value}&gt;
&lt;tr&gt;
&lt;td style="padding-right:6px; width:72px; white-space:nowrap"&gt;&lt;{$selector.$pord.name}&gt;：&lt;/td&gt;
&lt;td style=" border-bottom:1px solid #eee; line-height:22px;"&gt;&lt;{foreach from=$selector.$pord.options item="item" key=value}&gt;&lt;a href="&lt;{selector key=$pord value=$value}&gt;"&gt;&lt;{$item}&gt;&lt;/a&gt;&lt;{/foreach}&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;{/if}&gt;
&lt;{/foreach}&gt;
&lt;{else}&gt;
&lt;{foreach from=$selector item="column" key=column_id}&gt;
&lt;{if count($column.options)&gt;0 &amp;&amp; !$column.value}&gt;
&lt;tr&gt;
&lt;td style="padding-right:6px; width:72px; white-space:nowrap"&gt;&lt;{$column.name}&gt;：&lt;/td&gt;
&lt;td style=" border-bottom:1px solid #eee; line-height:22px;"&gt;&lt;{foreach from=$column.options item="item" key=value}&gt;&lt;a href="&lt;{selector key=$column_id value=$value}&gt;"&gt;&lt;{$item}&gt;&lt;/a&gt;&lt;{/foreach}&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;{/if}&gt;
&lt;{/foreach}&gt;
&lt;{/if}&gt;
&lt;/table&gt;
&lt;/div&gt;
&lt;{/if}&gt;

根据经验我们会发现&lt;{if count($column.options)&gt;0 &amp;&amp; !$column.value}&gt;这样的形式，我们知道这个是条件判断语句，那么是为了判断什么呢？这个问题才是关键对吧！从上面这几段代码的作用来看，就是显示前端的分类、品牌等的，那么判断一个条件才输出，朋友你可能猜到，这个条件是什么？对吧。

来吧，我们试试把条件取消看看。我们将上面的几段代码之中的

&amp;&amp; !$column.value

&amp;&amp; !$selector.$pord.value

给删除了，删除时注意不要删掉&lt;{与}&gt;两个分解符（当然你也可以给注释掉）。保存查看一下吧！

怎样你会发现什么呢？

对了！选中的分类保留着了！

简单吧~其他三个变量就按照这样的思路做一下就会发现，我们的需求都实现了。

进阶提示：

你看到的效果是保留选择的分类，但是，但是都是孤零零的保留着，没有其他同级的分类、品牌噢，怎么办？

开篇之前我就说了噢，我们这边的信息主要是三个变量在控制，那么我们这个时候需要对这三个变量的详细的值做处理了噢。只要不给过滤掉自然就能保留着了，详细的就留给有需要的朋友自己DIY吧。

然后，增加一个打叉的做取消这个功能，其实，从我本身来说觉得保留全部信息跟打叉的这两个功能有点鸡肋！

为什么？因为ShopEX的查询条件是单项的，渐进式的，比如颜色的话，同一时刻是一个颜色的，那么你颜色增加一个叉做啥噢？顶多增加一个全部的意义比较大，红叉的意义比较小，当然，个人之见哈~

全部的功能比较复杂，这边说一下我的思路吧，通过分析我们会看到，每一个点击的选项都是会显示出他的链接！而不同的项目之见是参数值的不同，那么，只要把相应的位置的值设置为空的话，是不是就是全部了呢？是的！怎么做这个修改呢？

通过我的不断尝试，最后结果出来了，这边就不废话了，直接贴出代码吧~~

&lt;a href="&lt;{selector key=$column_id value=''}&gt;"&gt;全部&lt;/a&gt;

&lt;a href="&lt;{selector key=$pord value=''}&gt;"&gt;全部&lt;/a&gt;

&lt;a href="&lt;{selector key=$column_id value=$value}&gt;"&gt;全部&lt;/a&gt;

在上面的代码之后每一个的循环之前增加相应的代码串，然后就可以了。

收拾下班了~~~