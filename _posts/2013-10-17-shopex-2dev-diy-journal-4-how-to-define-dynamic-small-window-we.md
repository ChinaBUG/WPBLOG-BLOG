---
ID: 2624
post_title: >
  ShopEX二次开发DIY日记4之如何定义我们的前台动态小对话框窗口（MessageBox）？
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  在《二次开发DIY日记1之商品添加到收藏夹的提示改为动态提醒》中我们修改了提示收藏夹的提示方式，要求不高的同学可能不会有什么意见，对于部分有追求的小伙伴们来说，那个动态对话框好丑呀，怎么美化一下？
  勤劳的小伙伴们可能已经动手做了，将之前的代码修改了一下，具体的代码不懂得可以去看上文噢。
  原先的代码为：MessageBox.show(‘已加入收藏’);
  修改之后为：MessageBox.show(‘&lt;img src="http://www.ipodmp.com/sp/1.jpg"/&gt;’);
  保存，运行一下，你会发现，原先的文字变了~变成一张图片噢~~当然，可能有的人会发现意外：图很大张的朋友会看到图跳出那个动态框了噢！好难看噢。
  怎么办呢？看了一下MessageBox与MessageBox.show的定义你会发现（位于statics\script_src\jstools.js），只有一个样式控制项，就是你做模板时可以指定这个框的大小，样式名为success与error两个，你直接定义之后想要的效果就出现了。
  效果还是不错的吧，当然，像我们这么有追求的人，这点小成就是不会满足的，很快，你会发现，整站到处都是一样的大小，图片小的时候没办法自动变小，图片更加大的时候，也不懂得自动放大？！
  对吧，这样子设计真是糟糕，为什么就不弄好点呢？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-4-how-to-define-dynamic-small-window-we.html
published: true
post_date: 2013-10-17 16:39:28
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

在<a title="ShopEX-二次开发DIY日记1之商品添加到收藏夹的提示改为动态提醒" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-1-goods-add-to-favorites-that-changes-the-dynamic-remind/">《二次开发DIY日记1之商品添加到收藏夹的提示改为动态提醒》</a>中我们修改了提示收藏夹的提示方式，要求不高的同学可能不会有什么意见，对于部分有追求的小伙伴们来说，那个动态对话框好丑呀，怎么美化一下？

勤劳的小伙伴们可能已经动手做了，将之前的代码修改了一下，具体的代码不懂得可以去看<a title="ShopEX-二次开发DIY日记1之商品添加到收藏夹的提示改为动态提醒" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-1-goods-add-to-favorites-that-changes-the-dynamic-remind/">上文</a>噢。

原先的代码为：MessageBox.show(‘已加入收藏’);

修改之后为：MessageBox.show(‘&lt;img src="http://www.ipodmp.com/sp/1.jpg"/&gt;’);

保存，运行一下，你会发现，原先的文字变了~变成一张图片噢~~当然，可能有的人会发现意外：图很大张的朋友会看到图跳出那个动态框了噢！好难看噢。

怎么办呢？看了一下MessageBox与MessageBox.show的定义你会发现（位于statics\script_src\jstools.js），只有一个样式控制项，就是你做模板时可以指定这个框的大小，样式名为success与error两个，你直接定义之后想要的效果就出现了。

效果还是不错的吧，当然，像我们这么有追求的人，这点小成就是不会满足的，很快，你会发现，整站到处都是一样的大小，图片小的时候没办法自动变小，图片更加大的时候，也不懂得自动放大？！

对吧，这样子设计真是糟糕，为什么就不弄好点呢？

没人理你的抱怨，只能靠我们自己啦，DIY就是告诉你怎么靠自己吃饱喝足哈~~

今天要说的是mootool的类的继承与扩展，具体可以查阅 <a href="http://mootools.net/docs/core/Class/Class#Class">Class</a> 一文。

太专业的偶也不懂哈，我就按照两种方式，偶自己试验的代码来说明吧~

我们的目标是：让动态信息框可以指定大小尺寸，现在先来分析一下思路哈~

要做到我们需要的自定义动态对话框的话，那么我们需要先查看MessageBox类的定义，该定义在statics\script_src\jstools.js文件中的216行左右。

我们通读下来会发现，createBox方法对width有默认定义，而对height却没有定义。

我们很明显就能想到，最简单的就是直接在这个文件上直接修改一个参数来带入值，让程序按照我们的尺寸来创建窗口了噢。不过这个办法比较麻烦，开发的时候还要把这个文件重新压缩，合并，麻烦到死，所以，肯定不能这么干的，那么怎么办比较好呢？

嗯，聪明的小伙伴们肯定会想到那我就在我需要的地方重新定义这个类，这样不就省却了压缩啦，合并啦，这一大堆的麻烦。

好办法~不过这个也真的很麻烦，我这边有更好的方法噢~请看下文

学过编程语言的人都知道类有继承跟扩展的概念噢，具体自己去看专门的书籍，这边我不说这些专业的东西哈~~

1、implement - 继承

我们第一个想法就是，重新写这个类，既然不用重写类，那么重写方法总可以吧？下面就是重写createBox方法，直接从上面的jstools.js文件中拷贝过来做修改，我们要重点注意的是代码中加粗的地方，那个就是区别所在噢。

MessageBox.implement({
createBox:function(msg,type){
var msgCLIP=/&lt;h4[^&gt;]*&gt;([\s\S]*?)&lt;\/h4&gt;/;
var tempmsg=msg;
if(tempmsg.test(msgCLIP)){
tempmsg.replace(msgCLIP, function(){
msg=arguments[1];
return '';
});
}
var box = new Element('div').setStyles({
'position':'absolute',
'visibility':'hidden',
<strong>'width':this.options.width||200,</strong>
<strong> 'height':this.options.height||200,</strong>
'opacity':this.options.opacity||0,
'zIndex':65535
}).inject(document.body);
var obj=this;
box.addClass(type||'success').setHTML("&lt;h4&gt;",msg,"&lt;/h4&gt;").amongTo(window).effect('opacity').start(1).chain(function(){obj.flee(this.element);});
return box;
}
});

<strong>this.options.width</strong>与<strong><strong>this.options.height</strong></strong>代表我们可以在调用的使用传入<strong>width</strong>与<strong><strong>height</strong></strong>两个参数，也顺便设置了默认值哈~这样，调用时只要不指定都会显示200*200大小的对话框噢。

在需要的地方增加上面的代码，然后调用方式如下：

MessageBox.success('&lt;img src="http://www.ipodmp.com/sp/20130308/1.jpg"/&gt;',{width:750,height:300});

就会显示一张图片，对话框的大小为750*300的了。

2、Extends-扩充

看了上面的代码，你可能觉得，为什么代码那么长噢，我不要这么长的代码，怎么办？好吧，满足你吧~看代码

var <strong>MessageBoxV2</strong> = new Class({
Extends: MessageBox,
createBox:function(msg,type){
<strong>box = this.parent(msg,type);</strong>
box.setStyles({
'width':this.options.width||200,
'height':this.options.height||200,
}).amongTo(window).effect('opacity').start(1);
}
});

这个代码简单吧~~~请注意那个加粗的代码<strong>：</strong>

<strong>box = this.parent(msg,type);</strong>

表示执行父类的createBox方法，返回值赋给box，下面的代码就是获取创建的对话框对象，设置它的大小尺寸，针对窗口居中，特效显示。

<strong>var MessageBoxV2......</strong>

表示我们新创建一个类，从MessageBox扩展出来，当然你也能直接使用原来的变量名~~~~。

在需要的地方添加上面的代码，然后使用如下代码执行

new MessageBoxV2('&lt;img src="http://www.xu.com/sp/20130308/1.jpg"/&gt;','success',{width:750,height:300});

就会显示一张图片，对话框的大小为750*300的了。

总结

没什么说的噢。。。自己尝试一下吧

PS：其实我很纠结一个问题就是为什么implement的不能使用<strong> this.parent</strong>()方法，而Extends还需要另外新建一个类噢，但是网上查找了没有这方面的资料噢，可能是关键词不对吧，希望有知道的人可以留言噢。

2013.10.20

今天在看android教父高焕堂高老师的&lt;&lt;<a href="http://study.163.com/course/courseLearn.htm?courseId=358006#/learn/video?lessonId=477375&amp;courseId=358006">android从程序员到架构师之路</a>&gt;&gt;讲到继承与扩展的概念噢，有需要的同学可以看看。