---
ID: 2479
post_title: >
  CSS-记住这几点，DIV+CSS浏览器兼容性问题全搞定(HACK代码实现IE6/IE7/IE8/Firefox浏览器兼容)！
author: ChinaBUG
post_excerpt: |
  CSS对浏览器的兼容性有时让人很头疼,或许当你了解当中的技巧跟原理,就会觉得也不是难事,从网上收集了IE7,6与Fireofx的兼容性处理方法并整理了一下：
  请尽量用xhtml格式写代码，而且DOCTYPE影响 CSS 处理，作为W3C标准，一定要加DOCTYPE声明。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/css-fix-div-css-layout-al.html
published: true
post_date: 2013-06-13 14:59:49
---
条件注释判断浏览器

&lt;!--[if !IE]&gt;&lt;!--&gt; 除IE外都可识别 &lt;!--&lt;![endif]--&gt;
&lt;!--[if IE]&gt; 所有的IE可识别 &lt;![endif]--&gt;
&lt;!--[if IE 6]&gt; 仅IE6可识别 &lt;![endif]--&gt;
&lt;!--[if lt IE 6]&gt; IE6以及IE6以下版本可识别 &lt;![endif]--&gt;
&lt;!--[if gte IE 6]&gt; IE6以及IE6以上版本可识别 &lt;![endif]--&gt;
&lt;!--[if IE 7]&gt; 仅IE7可识别 &lt;![endif]--&gt;
&lt;!--[if lt IE 7]&gt; IE7以及IE7以下版本可识别 &lt;![endif]--&gt;
&lt;!--[if gte IE 7]&gt; IE7以及IE7以上版本可识别 &lt;![endif]--&gt;
&lt;!--[if IE 8]&gt; 仅IE8可识别 &lt;![endif]--&gt;
&lt;!--[if IE 9]&gt; 仅IE9可识别 &lt;![endif]--&gt;

---

作者：望极天涯 发布于：2012-9-14 10:10 分类：DIV+CSS

CSS对浏览器的兼容性有时让人很头疼,或许当你了解当中的技巧跟原理,就会觉得也不是难事,从网上收集了IE7,6与Fireofx的兼容性处理方法并整理了一下：

<span style="color: #ff0000;"><strong><span style="text-decoration: underline;">请尽量用xhtml格式写代码，而且DOCTYPE影响 CSS 处理，作为W3C标准，一定要加DOCTYPE声明。</span></strong></span>

1.div的垂直居中问题

vertical-align:middle; 将行距增加到和整个DIV一样高 line-height:200px; 然后插入文字，就垂直居中了。缺点是要控制内容不要换行

2. margin加倍的问题

设置为float的div在ie下设置的margin会加倍。这是一个ie6都存在的bug。解决方案是在这个div里面加上display:inline;

例如：

&lt;#div id=”imfloat”&gt; 相应的css为 #imfloat{ float:left; margin:5px; display:inline;}

3.浮动ie产生的双倍距离

#box{ float:left; width:100px; margin:0 0 0 100px; //这种情况之下IE会产生200px的距离 display:inline; //使浮动忽略}

这里细说一下block与inline两个元素：block元素的特点是,总是在新行上开始,高度,宽度,行高,边距都可以控制(块元素);Inline元素的特点是,和其他元素在同一行上,不可控制(内嵌元素);

#box{ display:block; //可以为内嵌元素模拟为块元素 display:inline; //实现同一行排列的效果 diplay:table;

4 IE与CSS宽度和CSS高度的问题div css技巧

IE不认得min-这个定义，但实际上它把正常的width和height当作有min的情况来使。这样问题就大了，如果只用宽度和高度，正常的浏览器里这两个值就不会变，如果只用min-width和min-height的话，IE下面根本等于没有设置宽度和高度。

比如要设置背景图片，这个宽度是比较重要的。要解决这个问题，可以这样：

#box{ width: 80px; height: 35px;}html&gt;body #box{ width: auto; height: auto; min-width: 80px; min-height: 35px;}

5.页面的最小宽度

min-width是个非常方便的CSS命令，它可以指定元素最小也不能小于某个宽度，这样就能保证排版一直正确。但IE不认得这个，而它实际上把 width当做最小宽度来使。为了让这一命令在IE上也能用，可以把一个＜div&gt; 放到＜body&gt; 标签下，然后为div指定一个类,然后CSS这样设计：

#container{ min-width: 600px; width:expression_r(document.body.clientWidth ＜ 600? "600px": "auto" );}

第一个min-width是正常的；css制作但第2行的width使用了Javascript，这只有IE才认得，这也会让你的HTML文档不太正规。它实际上通过Javascript的判断来实现最小宽度。

6.DIV浮动IE文本产生3象素的bug

左边对象浮动，右边采用外补丁的左边距来定位，右边对象内的文本会离左边有3px的间距.

#box{ float:left; width:800px;} #left{ float:left; width:50%;} #right{ width:50%;} *html #left{ margin-right:-3px; //这句是关键} &lt;div id="box"&gt; &lt;div id="left"&gt;＜/div&gt; &lt;div id="right"&gt;＜/div&gt; &lt;/div&gt;

7.IE捉迷藏的问题

当div应用复杂的时候每个栏中又有一些链接，DIV等这个时候容易发生捉迷藏的问题。

有些内容显示不出来，当鼠标选择这个区域是发现内容确实在页面。解决办法：对#layout使用line-height属性或者给#layout使用固定高和宽。页面结构尽量简单。

8.float的div闭合;清除浮动;自适应高度

①例如：＜#div id=”floatA” &gt;＜#div id=”floatB” &gt;＜#div id=”NOTfloatC” &gt;这里的NOTfloatC并不希望继续平移，而是希望往下排。(其中floatA、floatB的属性已经设置为float:left;)

这段代码在IE中毫无问题，问题出在FF。原因是NOTfloatC并非float标签，必须将float标签闭合。在 ＜#div class=”floatB”&gt; ＜#div class=”NOTfloatC”&gt;之间加上 ＜#div class=”clear”&gt;这个div一定要注意位置，而且必须与两个具有float属性的div同级，之间不能存在嵌套关系，否则会产生异常。并且将clear这种样式定义为为如下即可： .clear{ clear:both;}

②作为外部 wrapper 的 div 不要定死高度,div css制作为了让高度能自动适应，要在wrapper里面加上overflow:hidden; 当包含float的box的时候，高度自动适应在IE下无效，这时候应该触发IE的layout私有属性(万恶的IE啊！)用zoom:1;可以做到，这样就达到了兼容。 例如某一个wrapper如下定义：

.colwrapper{ overflow:hidden; zoom:1; margin:5px auto;}

③对于排版,我们用得最多的css描述可能就是float:left.有的时候我们需要在n栏的float div后面做一个统一的背景,譬如:

&lt;div id=”page”&gt; &lt;div id=”left”&gt;＜/div&gt; &lt;div id=”center”&gt;＜/div&gt; &lt;div id=”right”&gt;＜/div&gt; &lt;/div&gt;

比如我们要将page的背景设置成蓝色,以达到所有三栏的背景颜色是蓝色的目的,但是我们会发现随着left center right的向下拉长,而page居然保存高度不变,问题来了css 制作,原因在于page不是float属性,而我们的page由于要居中,不能设置成 float,所以我们应该这样解决

&lt;div id=”page”&gt; &lt;div id=”bg” style=”float:left;width:100%”&gt; &lt;div id=”left”&gt;＜/div&gt; &lt;div id=”center”&gt;＜/div&gt; &lt;div id=”right”&gt;＜/div&gt; &lt;/div&gt; &lt;/div&gt;

再嵌入一个float left而宽度是100%的DIV解决之道。

④万能float 闭合(非常重要!)

关于 clear CSS float 的原理可参见 [How To Clear Floats Without Structural Markup],将以下代码加入Global CSS 中,给需要闭合的div加上 即可,屡试不爽.

.clearfix:after { content:"."; display:block; height:0; clear:both; visibility:hidden; } .clearfix { display:inline-block; }

.clearfix {display:block;}

或者这样设置：.hackbox{ display:table; //将对象作为块元素级的表格显示}

9．高度不能自适应

高度不能自适应是当内层对象的高度发生变化时外层高度不能自动进行调节，特别是当内层对象使用margin 或paddign 时。

例：

#box {background-color:#eee; } #box p {margin-top: 20px;margin-bottom: 20px; text-align:center; } &lt;div id="box"&gt; &lt;p&gt;p对象中的内容＜/p&gt; &lt;/div&gt;

解决技巧：在P对象上下各加2个空的div对象CSS代码：.1{height:0px;overflow:hidden;}或者为DIV加上border属性。

10 .div+css之IE6下为什么图片下有空隙产生

解决这个BUG的技巧也有很多,可以是改变html的排版,或者设置img 为display:block 或者设置vertical-align 属性为vertical-align:top

bottom 　middle 　text-bottom 都可以解决.

11.如何对齐文本与文本输入框

加上 vertical-align:middle;

&lt;style type="text/css"&gt; &lt;!-- input { width:200px; height:30px; border:1px solid red; vertical-align:middle; } --&gt; &lt;/style&gt;

12.web标准中定义id与class区别吗

(1).web标准中是不容许重复ID的,比如 div id="aa" 不容许重复2次,而CSS class定义的是类,理论上可以无限重复, 这样需要多次引用的定义便可以使用他.

(2).属性的优先级问题

CSS ID的优先级要高于class,看上面的例子

(3).方便JS等客户端脚本,如果在页面中要对某个对象进行脚本操作,那么可以给他定义一个ID,否则只能利用遍历页面元素加上指定特定属性来找到它,这是相对浪费时间资源,远远不如一个ID来得简单。

13. LI中内容超过长度后以省略号显示的技巧

此技巧适用与IE与OP浏览器

&lt;style type="text/css"&gt; &lt;!-- li { width:200px; white-space:nowrap; text-overflow:ellipsis; -o-text-overflow:ellipsis; overflow: hidden; }

--&gt; &lt;/style&gt;

14.为什么web标准中IE无法设置滚动条颜色了

解决办法是将body换成html

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"<a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd</a>&gt; &lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt;

&lt;style type="text/css"&gt; &lt;!--

html { scrollbar-face-color:#f6f6f6; scrollbar-highlight-color:#fff; scrollbar-shadow-color:#eeeeee; scrollbar-3dlight-color:#eeeeee; scrollbar-arrow-color:#000; scrollbar-track-color:#fff; scrollbar-darkshadow-color:#fff; } --&gt; ＜/style&gt;

15.为什么无法定义1px左右高度的容器

IE6下这个问题是因为默认的行高造成的,解决的技巧也有很多,例如: overflow:hidden 　 zoom:0.08 　 line-height:1px

16.怎么样才能让层显示在FLASH之上呢

解决的办法是给FLASH设置透明

&lt;param name="wmode" value="transparent" /&gt;

17.怎样使一个层垂直居中于浏览器中

这里我们使用百分比绝对定位,与外补丁负值的技巧,负值的大小为其自身宽度高度除以二

&lt;style type="text/css"&gt; &lt;!-- div { position:absolute; top:50%; lef:50%; margin:-100px 0 0 -100px; width:200px; height:200px; border:1px solid red; } --&gt; &lt;/style&gt;

Firefox与IE的CSS兼容CSS HACK技巧

1. Div居中问题

div设置 margin-left, margin-right 为 auto 时已经居中，IE 不行，IE需要设定body居中，首先在父级元素定义text-algin: center;这个的意思就是在父级元素内的内容居中。

2.CSS 链接(a标签)的边框与背景

a链接加边框和背景色，需设置 display: block, 同时设置 float: left 保证不换行。参照 menubar, 给 a 和 menubar 设置高度是为了避免底边显示错位, 若不设 height, 可以在 menubar 中插入一个空格。

3.超链接访问过后hover样式就不出现的问题

被点击访问过的超链接样式不在具有hover和active了,很多人应该都遇到过这个问题,解决技巧是改变CSS属性的排列顺序: L-V-H-A

Code:

&lt;style type="text/css"&gt; &lt;!-- a:link {} a:visited {} a:hover {} a:active {} --&gt; &lt;/style&gt;

4. 游标手指cursor

cursor: pointer 可以同时在 IE FF 中显示游标手指状， hand 仅 IE 可以

5.UL的padding与margin

ul标签在FF中默认是有padding值的,而在IE中只有margin默认有值,所以先定义 ul{margin:0;padding:0;}就能解决大部分问题

6. FORM标签

这个标签在IE中,将会自动margin一些边距,而在FF中margin则是0,因此,如果想显示一致,所以最好在css中指定margin和 padding,针对上面两个问题,我的css中一般首先都使用这样的样式ul,form{margin:0;padding:0;}给定义死了,所以后面就不会为这个头疼了.

7. BOX模型解释不一致问题

在FF和IE中的BOX模型解释不一致导致相差2px解决技巧：div{margin:30px!important;margin:28px;} 注意这两个margin的顺序一定不能写反， important这个属性IE不能识别，但别的浏览器可以识别。所以在IE下其实解释成这样：

div{maring:30px;margin:28px}重复定义的话按照最后一个来执行，所以不可以只写margin:xx px!important;#box{ width:600px; //for ie6.0- w\idth:500px; //for ff+ie6.0} #box{ width:600px!important //for ff width:600px; //for ff+ie6.0 width :500px; //for ie6.0-}

8.属性选择器(这个不能算是兼容,是隐藏css的一个bug)

p[id]{}div[id]{}

这个对于IE6.0和IE6.0以下的版本都隐藏,FF和OPera作用.属性选择器和子选择器还是有区别的,子选择器的范围从形式来说缩小了,属性选择器的范围比较大,如p[id]中,所有p标签中有id的都是同样式的.

9.最狠的手段 - !important

如果实在没有办法解决一些细节问题,可以用这个技巧.FF对于”!important”会自动优先解析,然而IE则会忽略.如下

.tabd1{ background:url(/res/images/up/tab1.gif) no-repeat 0px 0px !important; background:url( /res/images/up/tab1.gif) no-repeat 1px 0px; }

值得注意的是，一定要将xxxx !important 这句放置在另一句之上。

10.IE,FF的默认值问题

或许你一直在抱怨为什么要专门为IE和FF写不同的CSS，为什么IE这样让人头疼，然后一边写css，一边咒骂那个可恶的M$ IE.其实对于css的标准支持方面，IE并没有我们想象的那么可恶，关键在于IE和FF的默认值不一样而已，掌握了这个技巧，你会发现写出兼容FF和 IE的css并不是那么困难，或许对于简单的css，你完全可以不用”!important”这个东西了。

我们都知道，浏览器在显示网页的时候，都会根据网页的css样式表来决定如何显示，但是我们在样式表中未必会将所有的元素都进行了具体的描述，当然也没有必要那么做，所以对于那些没有描述的属性，浏览器将采用内置默认的方式来进行显示，譬如文字，如果你没有在css中指定颜色，那么浏览器将采用黑色或者系统颜色来显示，div或者其他元素的背景，如果在css中没有被指定，浏览器则将其设置为白色或者透明，等等其他未定义的样式均如此。所以有很多东西出现 FF和IE显示不一样的根本原因在于它们的默认显示不一样，而这个默认样式该如何显示我知道在w3中有没有对应的标准来进行规定，因此对于这点也就别去怪罪IE了。

11.为什么FF下文本无法撑开容器的高度

标准浏览器中固定高度值的容器是不会象IE6里那样被撑开的,那我又想固定高度,又想能被撑开需要怎样设置呢？办法就是去掉height设置min- height:200px; 这里为了照顾不认识min-height的IE6 可以这样定义:

{ height:auto!important; height:200px; min-height:200px; }

12.FireFox下如何使连续长字段自动换行

众所周知IE中直接使用 word-wrap:break-word 就可以了, FF中我们使用JS插入 的技巧来解决

&lt;style type="text/css"&gt; &lt;!-- div { width:300px; word-wrap:break-word; border:1px solid red; } --&gt; &lt;/style&gt;

&lt;div id="ff"&gt;aaaaaaaaaaaaaaaaaaaaaaaaaaaa＜/div&gt;

&lt;scrīpt type="text/javascrīpt"&gt;

function toBreakWord(el, intLen){ var ōbj=document.getElementByIdx_x_xx_x_x(el); var strContent=obj.innerHTML; var strTemp=""; while(strContent.length&gt;intLen){ strTemp+=strContent.substr(0,intLen)+" "; strContent=strContent.substr(intLen,strContent.length); } strTemp+=" "+strContent; obj.innerHTML=strTemp; } if(document.getElementByIdx_x_xx_x_x &amp;&amp; !document.all) toBreakWord("ff", 37);

&lt;/scrīpt&gt;

13.为什么IE6下容器的宽度和FF解释不同？

问题的差别在于容器的整体宽度有没有将边框（border）的宽度算在其内,这里IE6解释为200PX ,而FF则解释为220PX,那究竟是怎么导致的问题呢？大家把容器顶部的xml去掉就会发现原来问题出在这,顶部的申明触发了IE的qurks mode。

&lt;?xml version="1.0" encoding="gb2312"?&gt; &lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" <a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd</a>&gt; &lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt; &lt;style type="text/css"&gt; &lt;!-- div { cursor:pointer; width:200px; height:200px; border:10px solid red } --&gt; &lt;/style&gt; &lt;div ōnclick="alert(this.offsetWidth)"&gt;让FireFox与IE兼容＜/div&gt;

IE7.0对CSS的支持又有新问题，解决如下。

第一种，CSS HACK

Bpx; _height:20px;

注意顺序。

下面这样也属于div CSS HACK，不过没有上面这样简洁。

#example { color: #333; } * html #example { color: #666; } *+html #example { color: #999; }

第二种，是使用IE专用的条件CSS注释

&lt;!--其他浏览器 --&gt; &lt;link rel="stylesheet" type="text/css" href="css.css" /&gt;

&lt;!--[if IE 7]&gt; &lt;!-- 适合于IE7 --&gt; &lt;link rel="stylesheet" type="text/css" href="ie7.css" /&gt; &lt;![endif]--&gt;

&lt;!--[if lte IE 6]&gt; &lt;!-- 适合于IE6及一下 --&gt; &lt;link rel="stylesheet" type="text/css" href="ie.css" /&gt; &lt;![endif]--&gt;

第三种，css filter的办法，以下是从国外网站翻译过来的。

新建一个css样式如下：

#item { width: 200px; height: 200px; background: red; }

新建一个div,并使用前面定义的css的样式：

&lt;div id="item"&gt;some text here＜/div&gt;

在body表现这里加入lang属性,中文为zh：

&lt;body lang="en"&gt;

现在对div元素再定义一个样式：

*:lang(en) #item{ background:green !important; }

++++++++

++++++++

做主题或程序并不能保证能兼容所有浏览器，但也不能为每个浏览器专门再设计一套。但是为了能更好的实现兼容只能在CSS里做文章了，这篇<strong>CSS HACK代码实现IE6/IE7/IE8/Firefox浏览器兼容</strong>以供参考。

1.区别IE和非IE浏览器CSS HACK代码：
<blockquote>
<pre>#divcss5{
background:blue; /*非IE 背景藍色*/
background:red \9; /*IE6、IE7、IE8背景紅色*/
}</pre>
</blockquote>
2.区别IE6,IE7,IE8,FF CSS HACK
【区别符号】：「\9」、「*」、「_」
【示例】：
<blockquote>
<pre>#divcss5{
background:blue; /*Firefox 背景变蓝色*/
background:red \9; /*IE8 背景变红色*/
*background:black; /*IE7 背景变黑色*/
_background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：因为IE系列浏览器可读「\9」，而IE6和IE7可读「*」(米字号)，另外IE6可辨识「_」(底线)，因此可以依照顺序写下来，就 会让浏览器正确的读取到自己看得懂得CSS语法，所以就可以有效区分IE各版本和非IE浏览器(像是Firefox、Opera、Google Chrome、Safari等)。

3.区别IE6、IE7、Firefox (EXP 1)
【区别符号】：「*」、「_」
【示例】：
<blockquote>
<pre>#divcss5{
background:blue; /*Firefox背景变蓝色*/
*background:black; /*IE7 背景变黑色*/
_background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：IE7和IE6可读「*」(米字号)，IE6又可以读「_」(底线)，但是IE7却无法读取「_」，至于Firefox(非IE浏览器)则完全无法辨识「*」和「_」，因此就可以透过这样的差异性来区分IE6、IE7、Firefox。

4.区别IE6、IE7、Firefox (EXP 2)
【区别符号】：「*」、「!important」
【示例】：
<blockquote>
<pre>#divcss5{
background:blue; /*Firefox 背景变蓝色*/
*background:green !important; /*IE7 背景变绿色*/
*background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：IE7可以辨识「*」和「!important」，但是IE6只可以辨识「*」，却无法辨识「!important」，至于Firefox可以读取「!important」但不能辨识「*」因此可以透过这样的差异来有效区隔IE6、IE7、Firefox。
5.区别IE7、Firefox
【区别符号】：「*」、「!important」
【示例】：
<blockquote>
<pre>#divcss5{
background:blue; /*Firefox 背景变蓝色*/
*background:green !important; /*IE7 背景变绿色*/
}</pre>
</blockquote>
【说明】：因为Firefox可以辨识「!important」但却无法辨识「*」，而IE7则可以同时看懂「*」、「!important」，因此可以两个辨识符号来区隔IE7和Firefox。
6.区别IE6、IE7 (EXP 1)
【区别符号】：「*」、「_」
【示例】：
<blockquote>
<pre>#tip {
*background:black; /*IE7 背景变黑色*/
_background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：IE7和IE6都可以辨识「*」(米字号)，但IE6可以辨识「_」(底线)，IE7却无法辨识，透过IE7无法读取「_」的特性就能轻鬆区隔IE6和IE7之间的差异。
7.区别IE6、IE7 (EXP 2)
【区别符号】：「!important」
【示例】：
<blockquote>
<pre>#divcss5{
background:black !important; /*IE7 背景变黑色*/
background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：因为IE7可读取「!important;」但IE6却不行，而CSS的读取步骤是从上到下，因此IE6读取时因无法辨识「!important」而直接跳到下一行读取CSS，所以背景色会呈现橘色。
8.区别IE6、Firefox
【区别符号】：「_」
【示例】：
<blockquote>
<pre>#divcss5{
background:black; /*Firefox 背景变黑色*/
_background:orange; /*IE6 背景变橘色*/
}</pre>
</blockquote>
【说明】：因为IE6可以辨识「_」(底线)，但是Firefox却不行，因此可以透过这样的差异来区隔Firefox和IE6，有效达成CSS hack。

~.~.~.~.~.~.~.~.~.~

efox 浏览器
<ol class="dp-j">
	<li class="alt">@-moz-document url-prefix() {</li>
	<li>  .selector {</li>
	<li class="alt">    property: value;</li>
	<li>  }</li>
	<li class="alt">}</li>
</ol>
支持所有Gecko内核的浏览器 (包括Firefox)
<ol class="dp-j">
	<li class="alt">*&gt;.selector { property: value; }</li>
	<li></li>
	<li class="alt">Webkit 内核浏览器</li>
	<li></li>
	<li class="alt"><span class="annotation">@media</span> screen and (-webkit-min-device-pixel-ratio: <span class="number">0</span>) {</li>
	<li>  Selector {</li>
	<li class="alt">    property: value;</li>
	<li>  }</li>
	<li class="alt">}</li>
</ol>
Opera 浏览器
<ol class="dp-j">
	<li class="alt">html:first-child&gt;b\ody Selector {property:value;}</li>
</ol>
IE 浏览器

IE 浏览器针对不同的版本有不同个Hack方式。

IE 9
<ol class="dp-j">
	<li class="alt">:root Selector {property: value\<span class="number">9</span>;}</li>
</ol>
IE 9-
<ol class="dp-j">
	<li class="alt">Selector {property: value\<span class="number">9</span>;}</li>
</ol>
IE 8
<ol class="dp-j">
	<li class="alt">Selector {property: value/;}</li>
	<li>或：</li>
	<li class="alt"><span class="annotation">@media</span> \0screen {</li>
	<li>    Selector {property: value;}</li>
	<li class="alt">}</li>
</ol>
IE 8+
<ol class="dp-j">
	<li class="alt">Selector {property: value\<span class="number">0</span>;}</li>
</ol>
IE 7
<ol class="dp-j">
	<li class="alt">*+html Selector{property:value;}</li>
	<li class="alt">或：</li>
	<li>*:first-child+html Selector {property:value;}</li>
</ol>
IE 7-
<ol class="dp-j">
	<li class="alt">Selector {*property: value;}</li>
</ol>
IE6
<ol class="dp-j">
	<li class="alt">Selector {</li>
	<li>  _property: value;</li>
	<li class="alt">}</li>
	<li></li>
	<li class="alt">或者：</li>
	<li>*html Selector {</li>
	<li class="alt">  property: value;</li>
	<li>}</li>
</ol>