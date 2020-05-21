---
ID: 2884
post_title: >
  ShopNC二次开发研究日记1:怎么修改商品分类的个数(解除商品分类8个的限制)
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  前段日子在ShopNC交流群里面有朋友提出《怎么修改商品分类的个数》，现在刚研究到这个地方，就拿来当实例操作一番吧，其实很简单，但是过程是很曲折的，如果不想知道我的研究过程的话，那么请直接移到文章下方的解决方案处直接查看解决方案。
  研究过程啐啐念
  ShopNC的二次开发研究一直没有进展，主要是自己很忙，参加群好久了，一直没动工，早就看到这个朋友的这个问题，但是不懂得怎么回答，因为不懂呗，正好有人共享一份的代码，好吧，研究从此开始~
  一看这个需求，第一个想法就是既然显示的数量受到限制，那么原因如下：1.系统后台存在的商品分类就只有8个；2.系统输出的变量处理过了，只能输出8个；3.其他莫名其妙的问题。
  根据上面推测的原因，我去找一下，结果发现系统后台的商品分类有11个多，也就是1.的原因可以排除了，那么就剩下2.的原因了，根据开发经验，找到控制的地方，也就是代码
  $output['show_goods_class']
  所在的地方，也没详细看代码的逻辑，直接一头砸入$output['show_goods_class']变量获取的来源之中，我找呀找呀，然后发现这个值其实是来源于cache\goods_class.php文件，这下好了，只要数据在这边就能分析出一些东西啦，然后我开始输出这个文件的内容发现，这个不就是后台的分类的数据吗？！还是全部分类项目！
  这个就奇怪了，这个变量输出之后根本就没有做截取，那么为什么前台显示只能显示8个大分类呢？？
  我们的2.的猜测是排除了，就剩下3.的莫名其妙的问题了，既然数据是完整的，那么无非就是前台处理程序输出的时候限制了呗，所以，又绕回来了~我们找到$output['show_goods_class']所在，发现了一个秘密！
  于是我就打开网站，打开firbug然后一看，哇咧~~白跑了~~不是全在上面吗？！
  原来程序是把分类都输出，只是把没用到的给隐藏了！
  我尝试着把display:none改为display:block然后终于看到了我们久违的分类了~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopnc-2dev-study-of-diary-1-how-to-modify-the-number-of-commodity-classification.html
published: true
post_date: 2013-11-26 09:55:56
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

前段日子在ShopNC交流群里面有朋友提出《怎么修改商品分类的个数》，现在刚研究到这个地方，就拿来当实例操作一番吧，其实很简单，但是过程是很曲折的，如果不想知道我的研究过程的话，那么请直接移到文章下方的解决方案处直接查看解决方案。

<span style="text-decoration: underline;"><strong>研究过程啐啐念</strong></span>

ShopNC的二次开发研究一直没有进展，主要是自己很忙，参加群好久了，一直没动工，早就看到这个朋友的这个问题，但是不懂得怎么回答，因为不懂呗，正好有人共享一份的代码，好吧，研究从此开始~

一看这个需求，第一个想法就是既然显示的数量受到限制，那么原因如下：1.系统后台存在的商品分类就只有8个；2.系统输出的变量处理过了，只能输出8个；3.其他莫名其妙的问题。

根据上面推测的原因，我去找一下，结果发现系统后台的商品分类有11个多，也就是1.的原因可以排除了，那么就剩下2.的原因了，根据开发经验，找到控制的地方，也就是代码

$output['show_goods_class']

所在的地方，也没详细看代码的逻辑，直接一头砸入$output['show_goods_class']变量获取的来源之中，我找呀找呀，然后发现这个值其实是来源于cache\goods_class.php文件，这下好了，只要数据在这边就能分析出一些东西啦，然后我开始输出这个文件的内容发现，这个不就是后台的分类的数据吗？！还是全部分类项目！

这个就奇怪了，这个变量输出之后根本就没有做截取，那么为什么前台显示只能显示8个大分类呢？？

我们的2.的猜测是排除了，就剩下3.的莫名其妙的问题了，既然数据是完整的，那么无非就是前台处理程序输出的时候限制了呗，所以，又绕回来了~我们找到$output['show_goods_class']所在，发现了一个秘密！

于是我就打开网站，打开firbug然后一看，哇咧~~白跑了~~不是全在上面吗？！

原来程序是把分类都输出，只是把没用到的给隐藏了！

我尝试着把display:none改为display:block然后终于看到了我们久违的分类了~

<span style="text-decoration: underline;"><strong>研究成果展示</strong></span>

要修改这个<em><span style="text-decoration: underline;">前提条件就是模板要是解密</span></em>的，要不不能做修改噢。

请打开templates\default\layout\home_layout.php文件，查找$output['show_goods_class']关键词，然后你会看到如下放的代码：
<pre>......
  &lt;ul&gt;
    &lt;?php
    if(is_array($output['show_goods_class']) &amp;&amp; count($output['show_goods_class']) != 0){ $sign = 1;
    foreach ($output['show_goods_class'] as $tkey=&gt;$val){
    if ($val['gc_parent_id'] != '0') break;
    ?&gt;
    &lt;li id="cat_&lt;?php echo $sign;?&gt;" &lt;?php if(<span style="color: #ff0000;">$sign&gt;8</span>){?&gt;style="display:none;"&lt;?php }?&gt;&gt;
      &lt;h3&gt;&lt;a href="index.php?act=search&amp;cate_id=&lt;?php echo $val['gc_id'];?&gt;" title="&lt;?php echo $val['gc_name']?&gt;"&gt;&lt;?php echo $val['gc_name']?&gt;&lt;/a&gt;&lt;/h3&gt;
      &lt;div id="cat_&lt;?php echo $sign;?&gt;_menu" &gt;
......</pre>
如果你看到的都是乱码的话，请找一下破解版吧~

查看一下上方代码之中的红色区域$sign&gt;8这个条件，请将8修改为你想要显示的分类数，比如你的总的分类是11个大分类，你希望显示8个，那么轻不需要修改，你要是希望显示10个的话，那么请修改为10即可，如果你希望全部显示的话，那么请将这个数字弄大一点，最起码要大于你的一级分类总数，当然最简单的就是将下面的代码直接删除即可！
<pre>&lt;?php if($sign&gt;8){?&gt;style="display:none;"&lt;?php }?&gt;</pre>
保存，前台刷新一下，哇咧~~~你的分类是不是增加了呢？！