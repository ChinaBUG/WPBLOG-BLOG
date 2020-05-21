---
ID: 2872
post_title: >
  ShopEX二次开发DIY日记13之模板主题设计制作:单独页面的创建及单独页模板的配置设计维护提高篇
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  在二次开发DIY日记9《ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇》一文中，已经讲了单独主题页的新建及维护的基本方法，今天来一个提高，顺便填一下基础篇里面挖的那个坑：如果要实现单独主题页的布局、页头、页尾的不一致怎么办？
  标题写的是提高篇，其实，也仅仅是增加了代码的修改，要求大家或多或少懂一点CSS与DIV的基础知识。
  借用上一篇的一张图片：
  图01：单独页面的代码编辑界面
  在基础篇里，我们遇上这个步骤的时候是建议大家直接点击保存的，并没有说明这个文档的作用及怎么修改噢，本文就是来解答这个问题。
  这个编辑器里面的代码都是page-welcome.html文件里面的内容。
  作用就是渲染出那些“坑”位还有“系统区域”，定义这些东西的位置，样式等信息的。
  比如我们看到的有一个系统区域，那么如果要把系统区域取消怎么办呢？
  很简单，直接将<{main}>这样的字符串给删除或者注释掉，然后保存，然后可视化编辑的时候你将看不到系统区域了~~不过一般没人无聊到这个地步给他取消的哈~建议保留。
  请看下图，一般我们都是把暂时不要的给注释掉，就是在花括号上加上*号，然后我们在图04上看到，原先有的图02上的系统功能区块在图04中已经消失了噢。
  另外，在代码中我们还看到很多用<{...}>括起来的内容，这些什么意思呢？下面说明一下模板标签噢：
  1、<{require file="block/header.html"}>引入我们的页头：这是为了重复利用而设计的，当然你也可以没有这个。
  2、<{require file="block/footer.html"}>引入我们的页尾：这是为了重复利用而设计的，当然你也可以没有这个。
  3、<{widgets id="abc"}>这是一个名字叫做abc的挂件坑位。你可以有无数个坑位，唯一差别就是id的名字要不一样！
  4、<{header}>：引入系统必须的资源，这个资源是放在头部。必须存在，如果没有在上面1中，请自行在页面内包含。
  5、<{footer}>：引入系统必须的资源，这个资源是放在尾部。必须存在，如果没有在上面2中，请自行在页面内包含。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-13-create-a-separate-page-and-a-separate-page-template-configuration-design-advanced.html
published: true
post_date: 2013-11-22 14:08:13
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-
<blockquote><em>附注：
本文为进阶内容，另外有基础版本，敬请阅读《</em><a title="ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-9-create-a-separate-page-and-a-separate-page-template-configuration-design-basic/">ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇</a>》</blockquote>
在二次开发DIY日记9《<a title="ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-9-create-a-separate-page-and-a-separate-page-template-configuration-design-basic/">ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇</a>》一文中，已经讲了单独主题页的新建及维护的基本方法，今天来一个提高，顺便填一下基础篇里面挖的那个坑：如果要实现单独主题页的布局、页头、页尾的不一致怎么办？

标题写的是提高篇，其实，也仅仅是增加了代码的修改，要求大家或多或少懂一点CSS与DIV的基础知识。

借用上一篇的一张图片：

图01：单独页面的代码编辑界面
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/04.png"><img class="alignnone size-full wp-image-2855" alt="04" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/04.png" width="582" height="274" /></a>

在基础篇里，我们遇上这个步骤的时候是建议大家直接点击保存的，并没有说明这个文档的作用及怎么修改噢，本文就是来解答这个问题。

这个编辑器里面的代码都是page-welcome.html文件里面的内容。

作用就是渲染出那些“坑”位还有“系统区域”，定义这些东西的位置，样式等信息的。

比如我们看到的有一个系统区域，那么如果要把系统区域取消怎么办呢？

很简单，直接将&lt;{main}&gt;这样的字符串给删除或者注释掉，然后保存，然后可视化编辑的时候你将看不到系统区域了~~不过一般没人无聊到这个地步给他取消的哈~建议保留。

请看下图，一般我们都是把暂时不要的给注释掉，就是在花括号上加上*号，然后我们在图04上看到，原先有的图02上的系统功能区块在图04中已经消失了噢。

图02：没有隐藏系统功能区域的效果
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/06.jpg"><img alt="图02：没有隐藏系统功能区域的效果" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/06.jpg" width="701" height="309" /></a>
图03：隐藏系统功能区域
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/b01.png"><img class="alignnone size-full wp-image-2889" alt="图03：隐藏系统功能区域" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/b01.png" width="577" height="214" /></a>
图04：隐藏系统功能区域的效果
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/b02.jpg"><img class="alignnone size-full wp-image-2890" alt="图04：隐藏系统功能区域的效果" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/b02.jpg" width="633" height="245" /></a>

另外，在代码中我们还看到很多用&lt;{...}&gt;括起来的内容，这些什么意思呢？下面说明一下<em><strong><span style="text-decoration: underline;">模板标签</span></strong></em>噢：
<blockquote><em>1、&lt;{require file="block/header.html"}&gt;引入我们的页头：这是为了重复利用而设计的，当然你也可以没有这个。</em>

<em>2、&lt;{require file="block/footer.html"}&gt;引入我们的页尾：这是为了重复利用而设计的，当然你也可以没有这个。</em>

<em>3、&lt;{widgets id="abc"}&gt;这是一个名字叫做abc的挂件坑位。你可以有无数个坑位，唯一差别就是id的名字要不一样！</em>

<em>4、&lt;{header}&gt;：引入系统必须的资源，这个资源是放在头部。必须存在，如果没有在上面1中，请自行在页面内包含。</em>

<em>5、&lt;{footer}&gt;：引入系统必须的资源，这个资源是放在尾部。必须存在，如果没有在上面2中，请自行在页面内包含。</em></blockquote>
简单吧，其实这边的需要记住的东西不多，更多的是DIV+CSS的应用！顺便说一下，很多人觉得做ShopEX的主题模板很复杂，其实一点也不复杂，起码比wordpress、ShopNC、ECShop等商城系统来的简单。

现在进入我们的提高篇，在这边我们将分享怎么修改模板的结构，怎么增加新的头部，尾部。另外，我们将分享一下挂件的一些资讯。

<em><strong><span style="text-decoration: underline;">修改模板的结构</span></strong></em>

在图02：没有隐藏系统功能区域的效果一图中我们看到左边有一个空板块区域，右边也有一个，下面还有一个系统功能区域，系统功能区域我们在图04时隐藏了。

现在我们想想，除了隐藏我们还能做什么呢？对了，你是不是觉得左边跟右边的“坑”位才两个，很少不够用？

来吧，增加几个坑让需要的挂件去躺吧~

请进入page-welcome.html模板编辑页面，如图01所示，你要是不懂的怎么进入的话，请查看基础篇。
<blockquote><em>提示：页面管理》模板列表》编辑模板》找到模板页面page-welcome.html 点击可视化编辑按钮</em></blockquote>
怎么增加呢？请往回看看模板标签的说明吧。
<blockquote><em>提示：什么是模板标签？就是上面那个&lt;{...}&gt;五大点的说明。</em></blockquote>
看到第三点了吗？

<em>&lt;{widgets id="abc"}&gt;这是一个名字叫做abc的挂件坑位。你可以有无数个坑位，唯一差别就是id的名字要不一样！</em>

说的很清楚了吧？我们要增加一个的话，只需要修改id即可！

来，我们增加一个,在编辑界面中的

&lt;{widgets id="nav"}&gt;

后面增加

&lt;{widgets id="nav2013"}&gt;

保存一下，然后我们可视化编辑，你会发现，噢~~真的增加一个坑啦！

图05：增加新的挂件坑位
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/b03.png"><img class="alignnone size-full wp-image-2892" alt="图05：增加新的挂件坑位" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/b03.png" width="679" height="148" /></a>
图06：增加新的挂件坑位效果
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/b04.png"><img class="alignnone size-full wp-image-2893" alt="图06：增加新的挂件坑位效果" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/b04.png" width="989" height="144" /></a>

你操作完了吗？还没得话赶紧去试试看吧，你绝对有成就感~~很简单的就设计好一个模板了噢。

也许你会很疑问：为什么要增加坑？！

其实，一个坑是可以放多个挂件的！但是这样子是不提倡的，因为出现问题你会不好处理，另外就是在编辑的时候会出现互相干扰的情况，所以没必要都是不建议一个坑蹲多个挂件的！

有个朋友问过我后台那么多的挂件，大部分都是用不到的吧？！其实，那些挂件不是多，而是基本的，根据不同的需要搭配的，有时候你会发现根本不够用，还需要重新定制！

现在你会增加挂件坑了吧，那么你会添加挂件吗？不懂得话请查阅基础篇噢。

我们再来做点高级一点的吧，在图06上，2号的坑在右边，你看的很不爽，希望他在左边怎么办？

聪明的小伙伴一定会说，移动到右边呗！

没错，移动！怎么移动呢？坑位是一个代码控制的，要移动坑位，就是要移动代码呗，请将图05上的代码：

&lt;{widgets id="sideritems"}&gt;

剪切黏贴到

&lt;{widgets id="nav2013"}&gt;

后面吧~保存，可视化编辑，你会发现什么呢？！

图06：移动挂件坑位的效果
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/b05.png"><img class="alignnone size-full wp-image-2895" alt="图06：移动挂件坑位的效果" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/b05.png" width="374" height="112" /></a>

是不是坑位都移动到左边来了呢？

这个机制告诉我们：<em><span style="text-decoration: underline;">不管你的挂件是什么，只要移动了挂件的坑或者说是位置，那么挂件也跟着换位置了，所以，导航栏一般是放在顶部，如果你不喜欢，可以将坑位移动到尾部也是可以的。</span></em>

所以，你看到的东西只要能找到坑的位置，那么就可以移动！

现在三个挂件都在左边，那么整体看起来好奇怪你不觉得吗？右边一个地方是空白的！！！

为了完美我们是不是要取消掉这个空白，让我们的左边的区域占领全屏呢？

来吧~这个时候就是涉及到CSS的知识了，你要是不懂那就去学吧~~<a href="http://www.w3school.com.cn/css/index.asp">CSS基础教程</a> 点击即可立即学习^_^

我们在图05上将

&lt;div class="sideColumn pageSide"&gt; &lt;/div&gt;

删除或者注释掉，这个位置就是右边空白地方的占位符。

保存之后查看会发现位置还是被占着......

怎么回事呢？！

如果有开发经验的同学肯定知道那是因为右边指定了宽度啦，所以没办法占满全屏噢。

来修改一下吧~~

经过排查我们发现是坑位所在的外框的div限定了宽度，那么很自然的我们就是修改掉不就行了？！

这边不建议贸然的直接修改已经存在的样式，我们直接用内嵌的方式指定吧，将代码修改为：

&lt;div class="mainColumn pageMain" <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">style="width: 100%;"</span></span>&gt;&lt;{widgets id="nav"}&gt; &lt;{widgets id="nav2013"}&gt;&lt;{widgets id="sideritems"}&gt; &lt;{*main*}&gt; &lt;/div&gt;

请注意代码中红色部分，这是增加了style属性。

然后保存，浏览，你会发现，很好~~占满全屏了噢（<em><span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">注意：这边的全屏是指网站格局的全屏而不是真正的全屏幕，请注意，需要真正的全屏幕还是需要其他的设定</span></span></em>）。

至此，我们的模板的结构被我们修改的差不多了，请自己研究一下并多做练习吧。

<span style="text-decoration: underline;"><em><strong>自定义我们的页头页尾</strong></em></span>

重头戏来了~~在基础篇，我们埋下的坑，希望自己自定义页头页尾，现在到了解决的时候了~直接进入教程

请查看上面的模板标签的说明，这次重点查看页头，至于页尾那就是一样的操作啦~

我们看到：
<blockquote><em>1、&lt;{require file="block/header.html"}&gt;引入我们的页头：这是为了重复利用而设计的，当然你也可以没有这个。</em>

<em>4、&lt;{header}&gt;：引入系统必须的资源，这个资源是放在头部。必须存在，如果没有在上面1中，请自行在页面内包含。</em></blockquote>
这两个的说明，我们知道要制作页头需要这两个标签的帮助噢。那么我们来个简单的。

请进入page-welcome.html模板编辑页面，你要是还是不懂的进入的话，那么请不要继续了，因为特没意思，已经重复很多遍了~难道还要帮你把饭都吃了？

我们可以看到最顶部是

&lt;{require file="block/header.html"}&gt;

这个代码，这个代码干嘛用的噢？上面已经说明了，那么你肯定会问为什么需要这个文件呢？不用可以不？

答案是，不用这个文件可以，但是必须使用<em>&lt;{header}&gt;</em>这个标签，这个标签是系统自动输出的，没有这个那么系统将不能工作！既然是系统自动输出的话。那么你应该能想到了，这个是没办法自己定义内容的了，当然也不是绝对不能定制，还是有办法定制，不过不是我们今天要讲的。所以，如果你不想要定义页头的话，那么就只需要使用&lt;{header}&gt;标签即可。

下面就是两种情况的代码：

1.不能定制页头页尾

<em>&lt;{header}&gt;</em>
&lt;div&gt;......&lt;/div&gt;
<em>&lt;{footer}&gt;</em>

2.可以定制页头页尾

&lt;{require file="block/header.html"}&gt;
&lt;div&gt;......&lt;/div&gt;
&lt;{require file="block/footer.html"}&gt;

3.你可以混合起来使用

<em>&lt;{header}&gt;</em>
&lt;div&gt;......&lt;/div&gt;
&lt;{require file="block/footer.html"}&gt;

或者

&lt;{require file="block/header.html"}&gt;
&lt;div&gt;......&lt;/div&gt;
<em>&lt;{footer}&gt;</em>

简单吧？！基础了解好了~那么我们就来自定义我们的内容了，这边演示怎么自定义页头噢。

请记住，要自定义的话一定要使用

&lt;{require file="block/header.html"}&gt;

这样子的形式，这样子才能写入我们自己的代码噢。这个file的值表示在当前主题模板的根目录下面的block目录里面的header.html文件，我们一般使用下面的方式进行编辑：

<span style="text-decoration: underline;"><em>页面管理》模板列表》正在使用的模板》模板文件管理</em></span>
<blockquote><em>提示：以purple模板为例，则使用ftp登录服务器之后需要进入themes\purple\block目录内，就会看到头部文件了header.html。</em></blockquote>
然后你会看到好多的东西，其中你会看到block的字样，点击进入，你会看到好些文件，请找到header.html，点击就会自动进入编辑界面。下面就是部分代码：
<pre>......
<span style="text-decoration: underline; color: #ff0000;">&lt;{header}&gt;</span>
&lt;link rel="stylesheet" type="text/css" href="images/css.css" /&gt;
...
    &lt;div id="logo"&gt;&lt;{widgets id="logo"}&gt;&lt;/div&gt;
......</pre>
请看红色的&lt;{header}&gt;标签，看到没有，这个页面内一定需要加载因为，这个是系统必须的支持文件，没有这个里面的内容你就没办法使用系统了。除此之外你还能发现其他的一些HTMl代码，这些构成了一个头部文档了。

在这边做任何的修改要注意了！

因为"block/header.html"这个文件是被很多的页面加载，公用的一个模块！也就是你要是希望添加一个东西，让全站都一起用的话，那么请在这边直接修改即可反应到全站噢！！！

那么你要是希望这个头部单单一个模板使用，那怎么办？

好简单的问题，难道你不知道？新建一个头部文件呗~！
<blockquote><em><span style="color: #ff0000;">注意：新建一个模块的话都是需要重新设定挂件坑内的挂件调用的！如果你增加了新的挂件坑，没有调用新的挂件，那么也是没任何效果的噢。</span></em></blockquote>
我们来一个自定义的头部文件吧。

例子中，头部文件名字尽量设置的有意义点，比如我取名为"header_welcome.html"那么包含的文件的整体代码应该变成：

&lt;{require file="block/header_welcome.html"}&gt;

怎么新建这个文件？编辑器里面没有新建文件的工具！

答案：请新建一个文件，名字叫"header_welcome.html"，然后用ftp工具上传到网站目录内的相应位置即可。

新建这个文件，里面应该写些什么内容噢，测试一下吧~~请将下面的代码复制到你的"header_welcome.html"文件内：
<pre>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8" /&gt;
<span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">&lt;{header}&gt;</span></span>
&lt;link rel="stylesheet" type="text/css" href="images/css.css" /&gt;
&lt;/head&gt;&lt;body&gt;
<span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">&lt;h1&gt;WELCOME TO MY WEBSITE&lt;/h1&gt;
</span></span></pre>
注意红色区域噢，标签是载入必须的资源，你可以自己事后尝试一下去掉这个标签的效果噢。&lt;h1&gt;..&lt;/h1&gt;这个代码将在模板中显示我们设定的“WELCOME TO MY WEBSITE”这段话。

保存，上传，然后该让这个头部文件起作用了噢。

请进入page-welcome.html模板编辑页面，如图01所示，你要是不懂的怎么进入的话，请查看基础篇。

然后我们将：

&lt;{require file="block/header.html"}&gt;

修改为

&lt;{require file="block/header_welcome.html"}&gt;

然后保存，在进入可视化编辑，你看到什么了呢？！

对了~整个头部没了~就剩下一个“WELCOME TO MY WEBSITE”这句话。

有的着急的朋友会说，不是自定义头部吗？这么难看，还不如不要呢~~哇咧，好吧，这不太着急了~

我们现在来增加一点内容吧，一般头部都是有LOGO的，那么我们也弄一个吧~~

请进入block/header_welcome.html文件编辑界面，不懂得进入的话，请看上文。

将原先的代码修改为：

&lt;h1&gt;WELCOME TO MY WEBSITE...&lt;{widgets id="logo"}&gt;&lt;/h1&gt;

增加一个挂件坑位，保存，然后进入page-welcome.html模板编辑页面我们会看到

WELCOME TO MY WEBSITE...文字边上多了一个“空板块区域”。

这个坑位我们是要给LOGO的，那么现在请让我们来添加一个LOGO挂件吧。

<em><span style="text-decoration: underline;">添加板块》系统相关》商店Logo</span></em>

点击文字后面新出现的“空板块区域”然后保存即可，你会看到那个“空板块区域”已经变成了我们网站的LOGO了噢。

然后~~~没有然后了。

教程到此结束，剩下的你就自己举一反三，然后功夫到家的话，你就会自己设计ShopEX的模板了噢。
<blockquote><em>附注：
本文为进阶内容，另外有基础版本，敬请阅读《</em><a title="ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-9-create-a-separate-page-and-a-separate-page-template-configuration-design-basic/">ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇</a>》</blockquote>