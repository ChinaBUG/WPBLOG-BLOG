---
ID: 2761
post_title: >
  ShopEX二次开发DIY日记9之单独页面的创建及单独页模板的配置设计维护基础篇
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  在ShopEX-怎么新建单独专题页与怎么让单独专题页的链接是英文的(单页面独立页)一文之中我们谈了怎么新建一个单独专题页与怎么让单独专题页的链接是英文的，大家学的还不错吧，是不是觉得链接格外美丽呢？
  很多朋友在这点上做的不好，结果使得连接上带中文，这个对SEO有多大影响呢？这个我不知道，不过肯定影响美观了噢。
  今天我们继续讲怎么让ShopEX单独页的专题能够更好的，完美的展示我们的信息，又很方便的维护我们的信息噢，直白点讲，怎么让我们的客户未来自己维护方便了。
  ShopEX本身的模板机制挺不错的，可以可视化编辑，可以使用挂件调用，维护我们的数据，那么单独页专题页面能够使用可视化编辑吗？可以用挂件吗？
  答案是可以的！
  登陆后台之后点击：“页面管理>网站内容管理>站点栏目”然后“添加顶级栏目”，剩下的就是看你想要做什么样子的效果决定了噢。
  通过这样子的方式新建的单独页默认就已经有提供头部跟尾部的引用，而我们只需要设置中间部分的内容了。开始增加挂件吧~~
  简单吧？当然，有些着急的小伙伴肯定会急着发问，那我要是不要那种全站都一样的头部跟尾部怎么操作噢。不要着急，下文会说到的啦。
  在上面的方式极可能会出现你编辑好了，但是前台却显示不出来的情况，或者前台能显示出来，而后台可视化编辑时你却看不到挂件的坑位，所以我个人来说一般不推荐这种方式，这种方式比较不省事！继续看下面省事的办法吧~
  现在我们来说说怎么指定“单独页模板”！
  使用单独页模板我们可以做的东西就多了，比如指定不同的头部样式？不同的页面风格等等，让我们的单独页有别于其他的页面，但是却跟系统的功能又是一体的噢。
  那么如何使用单独页模板呢？请按照下面步骤来操作噢~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-9-create-a-separate-page-and-a-separate-page-template-configuration-design-basic.html
published: true
post_date: 2013-11-10 10:34:35
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-
<blockquote><em>附注：
本文为基础内容，另外有进阶版本，敬请阅读《</em><a title="ShopEX二次开发DIY日记13之模板主题设计制作:单独页面的创建及单独页模板的配置设计维护提高篇" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-13-create-a-separate-page-and-a-separate-page-template-configuration-design-advanced/">ShopEX二次开发DIY日记13之模板主题设计制作:单独页面的创建及单独页模板的配置设计维护提高篇</a>》</blockquote>
在<a title="ShopEX-怎么新建单独专题页与怎么让单独专题页的链接是英文的(单页面独立页)" href="http://blog.ipodmp.com/archives/shopex-how-new-separate-topic-page-with-links-to-how-to-make-a-separate-topic-page-english/">ShopEX-怎么新建单独专题页与怎么让单独专题页的链接是英文的(单页面独立页)</a>一文之中我们谈了怎么新建一个单独专题页与怎么让单独专题页的链接是英文的，大家学的还不错吧，是不是觉得链接格外美丽呢？

很多朋友在这点上做的不好，结果使得连接上带中文，这个对SEO有多大影响呢？这个我不知道，不过肯定影响美观了噢。

今天我们继续讲怎么让ShopEX单独页的专题能够更好的，完美的展示我们的信息，又很方便的维护我们的信息噢，直白点讲，怎么让我们的客户未来自己维护方便了。

ShopEX本身的模板机制挺不错的，可以可视化编辑，可以使用挂件调用，维护我们的数据，那么单独页专题页面能够使用可视化编辑吗？可以用挂件吗？

答案是可以的！

登陆后台之后点击：“页面管理&gt;网站内容管理&gt;站点栏目”然后“添加顶级栏目”，剩下的就是看你想要做什么样子的效果决定了噢。

通过这样子的方式新建的单独页默认就已经有提供头部跟尾部的引用，而我们只需要设置中间部分的内容了。开始增加挂件吧~~

简单吧？当然，有些着急的小伙伴肯定会急着发问，那我要是不要那种全站都一样的头部跟尾部怎么操作噢。不要着急，下文会说到的啦。

在上面的方式极可能会出现你编辑好了，但是前台却显示不出来的情况，或者前台能显示出来，而后台可视化编辑时你却看不到挂件的坑位（有的人这个界面能够编辑的了，偶很佩服，不过我就是从来就没有成功过），所以我个人来说一般不推荐这种方式，这种方式比较不省事！~~非要在这边添加的话，我一般是直接操作数据库添加，但是你也知道这样子的话，实在是糟糕透顶！

继续看下面省事的办法吧~

现在我们来说说怎么指定“单独页模板”！

使用单独页模板我们可以做的东西就多了，比如指定不同的头部样式？不同的页面风格等等，让我们的单独页有别于其他的页面，但是却跟系统的功能又是一体的噢。最关键是不会出现上面那种莫名其妙找不到文件的情况！

那么如何使用单独页模板呢？请按照下面步骤来操作噢~下面的单独主题页的创建及配置是很重要的，是未来使用ShopEX维护主题的一个很好地方式，敬请重视！
<blockquote><strong><span style="text-decoration: underline;">教程提示：
</span>         <span style="text-decoration: underline;">请严格按照下面说的步骤去做，在操作的过程之中不要想着要提问，也就是你心里有万分的想法要表达，也敬请不要中断，先跟随教程操作完毕再来发表您的疑问，因为您在操作的过程中的任何想法，极可能是想太多！先做了再提问，谢谢合作。</span></strong></blockquote>
<span style="text-decoration: underline;"><strong>第一部分</strong></span>

<del><span style="text-decoration: underline;">案例中我们以经典的主题《紫气东来(ShopEx4.8)》为例说明，其他的主题请根据实际做一下变通。新建的主题单独页的链接为：http://shopex326.ipodmp.com/page_welcome.html，页面的标题栏显示：欢迎您光临我的教程主页。</span></del>

请点击“页面管理&gt;模板管理&gt;模板列表”然后在右边“正在使用的模板”边上点击“编辑模板”，然后你会看到很多的模板文件噢，有的店的主题模板会有很多的文件（图01），不用怕噢，他是按照分类来罗列的。
<blockquote><em>默认分类：</em>
<em><span style="text-decoration: underline;">首页、商品列表页、商品详细页、商品评论/咨询页、文章列表页、赠品页、捆绑商品页、品牌专区页、品牌商品展示页、购物车页、高级搜索页、注册/登录页、会员中心页、站点栏目单独页、订单详细页、订单确认页、默认页</span></em></blockquote>
图01：模板列表
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/01.jpg"><img class="alignnone size-full wp-image-2852" alt="图01：模板列表" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/01.jpg" width="481" height="250" /></a>

现在我们先来新建一个新的单独页模板文件把~

在右上角您会看到“创建新页面”的文字边上有一个下拉菜单噢，请点击下来菜单吧，看看里面都有什么噢。

请找到“站点栏目单独页”并选择它。

然后在“页面名称”中输入你希望的名字，当然这个名字最好是容易辨别的噢，<strong><span style="text-decoration: underline;">必填项</span></strong>；“文件名”则填入你希望的文件名字，建议最好是使用英文，不要有特殊符号，当然，你可以使用默认选项哈，不过<strong><span style="text-decoration: underline;">建议你还是要填写比较好</span></strong>~
<blockquote><strong>“页面名称”：</strong>
这边实例填写page-welcome，你也可以不填这个，但是原则上填写的要看得懂他是做什么用的，<span style="text-decoration: underline;">一般我习惯性的如果页面的链接是什么，那么模板页的页面名字也就是什么</span>。
<strong>“文件名”：</strong>
这边实例填写welcome，默认的话将自动生成一个文件名（如：page-1385092209098.html），这个名字就是您在主题模板文件夹里面看到的文档名字（格式是：分类类型-xxxxxx，上面实例的就是page-welcome.html文件，我们通过<em><span style="text-decoration: underline;">编辑代码编辑的就是这个文件内的代码</span></em>）</blockquote>
点击创建按钮，会出现一个编辑框，一般刚开始都是直接点击“保存”按钮保存模板代码。
<blockquote><em>提醒：</em>
<em>1.第一次新建完毕请一定要保存，防止出现故障什么造成文档没有保存噢，再接下去的编辑中请根据实际情况编辑一些内容就要保存一下，防止意外的发生。</em>
<em>2.请注意编辑框的最上方有一个加黑的字符串，如实例：purple/page-welcome.html，这个文件名请记住，下面需要找到这个文件才能编辑。</em></blockquote>
图02：编辑模板
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/02.png"><img class="alignnone size-full wp-image-2853" alt="图02：编辑模板" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/02.png" width="709" height="369" /></a>

保存会自动返回模板的文件列表，我们找到刚才新建设的文件（page-welcome.html），点击“文件名”后面的“可视化编辑”按钮，进入可视化编辑操作模式噢。
<blockquote><em>提示：</em>
<em>您是不是忘记您刚才新建的文件名呢？
请直接在“站点栏目单独页”分类下面找，一般没有新建的话这里面只有一个文件的，多出来的一定是您新建的。这种情况多见于使用默认的文件名，所以，请仔细看上面的教程建议！</em></blockquote>
图03：站点栏目单独页
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/03.png"><img class="alignnone size-full wp-image-2854" alt="图03：站点栏目单独页" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/03.png" width="495" height="95" /></a>
图04：站点栏目单独页新建后的代码编辑
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/04.png"><img alt="图04：站点栏目单独页新建后的代码编辑" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/04.png" width="553" height="260" /></a>

在可视化编辑界面，我们看到好大一块的灰色地带，上面写着“<strong>系统功能区块[无法可视化编辑]</strong>”几个大字，这个是系统本身调用的，就是程序处理完数据就是输出在这个地方的，你是不能修改的！当然，不能修改不是绝对不能修改，你只要懂得开发技术，自然能对这个区域做二次开发，具体的教程可能你需要等待，如果有的话将会补充的。
<blockquote><strong>系统功能区块[无法可视化编辑]</strong>：
<em>这个地方是程序统一处理并输出的，一般客户是不能做修改，而且这个区域是一个整体，比如：商品列表页，那个边上的东西不看单单看那个橱窗的效果，就是系统自动调用程序输出的。如果你希望每一行的商品数量由3个变成4个的话那么使用系统后台那边设置即可，但是如果你希望奇偶行不同色的话怎么办？那个就只能定义系统功能区块了，通过编辑模板是做不到的。</em></blockquote>
我们先看编辑界面最重要的几个按钮：

顶部操作栏的左边的“添加版块、保存修改”，字眼上明显就知道意思啦，保存修改就是如果你做了修改就要保存噢，当然，在你对挂件最修改的时候，挂件的配置信息将会单独保存，这个保存功能是指的是对这个页面的位置等信息的保存，如果分不清楚请一定要养成习惯全部点击。“添加版块”这个功能点击之后就会看到很多可以使用的挂件！这个是shopex模板的灵魂噢，所以要善用这里的工具。
<blockquote><em>提示：</em>
<em>顶部的“保存修改”跟添加挂件与编辑挂件里面的“保存”的功能是不一样的，前者是仅仅保存布局的信息，而挂件编辑界面是保存挂件的参数的。如果编辑了挂件之后点击“保存”而没有点击“保存修改”的话则挂件里面的内容将变化。如果移动了界面的布局，比如将挂件从这个地方移动到另外一个地方的话，不点击“保存修改”是不会改变布局的，执行之后是原来的效果噢。</em></blockquote>
当我们的鼠标随意移动到一些元素上，比如LOGO或者菜单栏什么的，你会发现有的会出现一个蓝色框的东西将你选择的内容框起来，在蓝色框右上角你会发现有两个选项：编辑与删除，这边的编辑就是针对被蓝色框住的内容做修改，被蓝色框住的内容我们称之为挂件，框住的内容就是由挂件程序生成的，框所在的地方我们称之为坑，就是一个茅坑，给挂件这个人蹲的；删除就是将挂件这个人移开，保留下坑。

那么什么叫做“坑”呢？您的模板如果开发规范的话，你肯定会看到一些预留的坑位，在编辑界面上你会看到一些区域是虚线框起来，底色是灰色的，上面还写着“空板块区域”的就是了，这个就是肯定，提供给你需要安排的挂件蹲的地方，而这些坑的位置是由你的设计师设计的噢。

图05：模板列表
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/05.png"><img class="alignnone size-full wp-image-2856" alt="图05：模板列表" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/05.png" width="623" height="75" /></a>

图06：模板编辑界面
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/06.jpg"><img class="alignnone size-full wp-image-2857" alt="图06：模板编辑界面" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/06.jpg" width="628" height="277" /></a>

上面的很简单，听不懂的不要紧，请看下面的实例你就懂了噢。

点击“添加版块”按钮，在显示的弹窗里面你会看到板块中心，左边是分类，右边是详细的挂件内容。

图07：添加模块中心
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/07.jpg"><img class="alignnone size-full wp-image-2858" alt="图07：添加模块中心" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/07.jpg" width="706" height="492" /></a>

请点击右边分类的“自定义版块”，然后在左边显示出一个“用户自定义HTML”的玩意，请把鼠标移上去点击，然后你会发现系统跳回刚才的编辑界面了噢，不要乱点！请用你的眼睛寻找一下界面上的坑位！就是有写着“空板块区域”的灰色框，点击“空板块区域”这几个字所在的地方噢（图08：添加模块的步骤），然后就会出现挂件的配置界面（图09：添加挂件指定参数）。

PS：不是所有的挂件都有一样的配置选项噢，请根据挂件的不同设置不同的参数。

图08：添加模块的步骤
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/08.jpg"><img alt="图08：添加模块的步骤" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/08.jpg" width="670" height="373" /></a>

挂件的配置界面一般的分为两部分：上一部分为基础配置选项，这个是全部的挂件都一样的，里面的属性参与全局的程序调用。下一部分则为各个挂件不同的配置选项（图09：添加挂件指定参数）。

“用户自定义HTML”挂件提供一个编辑器给我们，让我们可以书写html代码噢，现在我们先来写一些文字吧，请点击输入框，不点击是没办法输入内容的噢。点击完毕，请写几个字，我一般都是写“ChinaBUG”你可以写其他的噢，然后我们可以设置一下粗体，下划线什么的。然后，<span style="text-decoration: underline;">请记得点击最下面的新建按钮</span>！这是重点！很多人会忘记这个然后说看不到效果。

图09：添加挂件指定参数
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/09.png"><img alt="图09：添加挂件指定参数" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/09.png" width="615" height="401" /></a>

然后，请点击编辑界面的最上方的“<span style="text-decoration: underline;"><strong>保存修改</strong></span>”按钮，请记住这个也是一定要点的（图10：保存设置）。

图10：保存设置
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/10.jpg"><img class="alignnone size-full wp-image-2861" alt="图10：保存设置" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/10.jpg" width="604" height="403" /></a>

<strong>第二部分</strong>

模板这样子就完成了，我们接着来新建我们的主题页噢，不懂得新建？请看下面吧，再重复一次噢~

点击“页面管理&gt;网站内容管理&gt;站点栏目”然后“添加顶级栏目”然后“栏目类型”选择“单独页面”，然后“栏目标题”填你喜欢的（实例这边请填写：page-welcome），“单独页模板”这个项很重要！一定要注意，因为填写完毕之后是不能修改的！这个选项是一个下拉菜单，我们点击一下看看，你有没有在里面找到刚才你新建的模板呢？！没有？那就是你没有按照我上面说的去做，请重新回答第一部分去。这边实例我们选择刚才上面新建的page-welcome即可。
<blockquote><em>提示：如果你忘记怎么设置友好链接的话，请查阅<a title="ShopEX-怎么新建单独专题页与怎么让单独专题页的链接是英文的(单页面独立页)" href="http://blog.ipodmp.com/archives/shopex-how-new-separate-topic-page-with-links-to-how-to-make-a-separate-topic-page-english/">ShopEX-怎么新建单独专题页与怎么让单独专题页的链接是英文的(单页面独立页)</a></em></blockquote>
图11：站点栏目列表
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/11.png"><img class="alignnone size-full wp-image-2862" alt="图11：站点栏目列表" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/11.png" width="499" height="196" /></a>

图12：添加顶级栏目界面
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/12.png"><img class="alignnone size-full wp-image-2863" alt="图12：添加顶级栏目界面" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/12.png" width="491" height="280" /></a>

图14：站点栏目列表及编辑功能选项
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/14.png"><img class="alignnone size-full wp-image-2865" alt="图14：站点栏目列表及编辑功能选项" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/14.png" width="719" height="209" /></a>

选择完我们的模板，点击“添加栏目”，然后会进入编辑界面，请直接点击“<span style="text-decoration: underline;"><strong>保存修改</strong></span>”即可，保存后点击“退出编辑”。

图13：保存我们的顶级栏目设置
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/13.png"><img alt="图13：保存我们的顶级栏目设置" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/13.png" width="689" height="205" /></a>

我们点击“站点栏目”找到我们刚才新建的栏目，点击后面的放大镜查看，你会看到我们刚才写的文字了吧~

图16：查看最后的单独主题页的效果
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/16.jpg"><img alt="图16：查看最后的单独主题页的效果" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/16.jpg" width="325" height="309" /></a>

细心的朋友可能会发现新建的主题页的标题上出现“page-welcome”等字样，对吧，怎么去掉噢？查看上面的链接吧，你会发现，事情很简单的~

图15：怎么设置单独主题页的标题
<a href="http://blog.ipodmp.com/wp-content/uploads/2013/11/15.png"><img alt="图15：怎么设置单独主题页的标题" src="http://blog.ipodmp.com/wp-content/uploads/2013/11/15.png" width="791" height="222" /></a>

然后以后我们需要更新内容的话，就直接编辑模板文件即可，而模板文件就有很多高级的应用，比如怎么设置不同的文件头，文件尾部......这些就留给大家去自己DIY吧~

PS：跟这个相似的还有商品分类模板，商品详情页模板等等，够大家研究一段时间了。
<blockquote><em>附注：
本文为基础内容，另外有进阶版本，敬请阅读《</em><a title="ShopEX二次开发DIY日记13之模板主题设计制作:单独页面的创建及单独页模板的配置设计维护提高篇" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-13-create-a-separate-page-and-a-separate-page-template-configuration-design-advanced/">ShopEX二次开发DIY日记13之模板主题设计制作:单独页面的创建及单独页模板的配置设计维护提高篇</a>》</blockquote>