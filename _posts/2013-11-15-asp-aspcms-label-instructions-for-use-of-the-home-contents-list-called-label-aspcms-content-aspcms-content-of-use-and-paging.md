---
ID: 2801
post_title: 'ASP-ASPCMS标签使用说明之首页内容列表调用标签{aspcms:content&#8230;}..{/aspcms:content}的使用及分页'
author: ChinaBUG
post_excerpt: |
  今天看到有人在问“{aspcms:content 怎么分页”这个问题，乘机写一下吧。
  我们从ASPcms手册上已经很明确，明显，还很清晰的看到aspcms:content标签部分的主题叫做“首页内容列表调用标签(无分页)”，已经告诉你了，这个标签使用在首页而且是无分页的。那么还想要分页？那么就只有两个答案了：要嘛就是表达错误，其实是在说内容详情页要分页；要嘛你自己开发让他支持分页。
  第一种情况：表达错误，其实是在说内容详情页要分页
  那么根据官方的手册其实很简单，就是在编辑内容的时候将{aspcms:page}这个标签增加到需要分页的地方，即可实现内容的分页。比如我们经常看到的一些美女站点，明明一套图直接放出来多好，还非要分页，就是用这个标签实现的。
  第二种情况：自己开发支持首页内容列表调用标签的分页的思路
  一说到自己开发就头痛，你觉得呢，其实也不难，我们主要要先找到控制这个标签所在的代码，然后参考一下其他的方法就可以自己修改出来呢。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/asp-aspcms-label-instructions-for-use-of-the-home-contents-list-called-label-aspcms-content-aspcms-content-of-use-and-paging.html
published: true
post_date: 2013-11-15 13:50:33
---
今天看到有人在问“{aspcms:content 怎么分页”这个问题，乘机写一下吧。

我们从ASPcms手册上已经很明确，明显，还很清晰的看到aspcms:content标签部分的主题叫做“首页内容列表调用标签(无分页)”，已经告诉你了，这个标签使用在首页而且是无分页的。那么还想要分页？那么就只有两个答案了：要嘛就是表达错误，其实是在说内容详情页要分页；要嘛你自己开发让他支持分页。

<strong><span style="text-decoration: underline;">第一种情况：表达错误，其实是在说内容详情页要分页</span></strong>

那么根据官方的手册其实很简单，就是在编辑内容的时候将{aspcms:page}这个标签增加到需要分页的地方，即可实现内容的分页。比如我们经常看到的一些美女站点，明明一套图直接放出来多好，还非要分页，就是用这个标签实现的。

具体使用：

在编辑器内假设我们放一张图片：

&lt;img src="a.jpg"/&gt;
&lt;img src="b.jpg"/&gt;
&lt;img src="c.jpg"/&gt;

上面只是一页里面全部显示，那么要分页的话就要改写为：

&lt;img src="a.jpg"/&gt;
{aspcms:page}
&lt;img src="b.jpg"/&gt;
{aspcms:page}
&lt;img src="c.jpg"/&gt;

这样子就实现了内容分页了，很简单的。

<strong><span style="text-decoration: underline;">第二种情况：自己开发支持首页内容列表调用标签的分页的思路</span></strong>

一说到自己开发就头痛，你觉得呢，其实也不难，我们主要要先找到控制这个标签所在的代码，然后参考一下其他的方法就可以自己修改出来呢。

#2.1

其实，怎么说呢，我个人觉得这个是鸡肋，你觉得呢？主页本身同一类的内容指定几篇，其他的都是直接导向列表页了，为什么还要在主页分页呢？当然，我能想到到的唯一应用就是如果你真的要在主页使用tab效果就是用标签显示同一类内容的话，其实可以用{if:}标签来操作而不一定要使用分页功能，个人感觉分页功能增加上去了很耗资源的。

如
<pre>{aspcms:content sort=2 num=4 order=order star=1}
    {if:[content:i]&lt;5}
        ......
    {else}
        ......
    {end if}
    '  or
    {if:[content:i]&lt;5}......{end if}
    {if:5&lt;[content:i]&lt;10}......{end if}
{/aspcms:content}</pre>
#2.2

使用上面取巧的办法你要是不乐意，那就只能自己努力的找代码修改代码了呗。下面就分析一下怎么找到相关位置的过程，具体的修改，这边不想涉及到，因为没多大必要的噢，有需要的朋友就根据思路自己做点修改，放心，有提点的。

最简单的想法就是那里调用，入口就是那里！所以我们要分出这个控制的代码在哪里很简单的就是要从根目录的index.asp文件入手！如果，你不懂这个文件做什么的话，那就不用继续了，因为下面说的基本上都是云里来雾里去的。
<pre>......
with templateObj
    .content=loadFile(templatePath) 'A---载入模板文件
    .parseHtml()                    'B---分析基本的功能
    .parseCommon                    'C---分析公共部分的功能
    echo .content                   'D---分析之后输出这个最终的内容
end with
......</pre>
下面是各个部分的说明：

A：

载入主页的模板，如果你觉得这个模板文件名字不好记的话你可以修改呗，但是，一般是不用改，而这个方法一般都不用理它的。

B：

这个是网站模板进行替换的前站，处理页头跟页尾的模板文件合并（parseTopAndFoot），文件子模板的合并（parseAuxiliaryTemplate）及网站内部的自定义标签的内容替换（parseLabel）。

C：

这个就是我们需要重视的地方，因为里面可以说大部分的功能都是在这里面完成的，包括我们今天需要修改的内容也是从这里面发现的。

D：

这个是模板经过上面几道流程处理过后得到的HTML代码，输出就是我们的网页内容了。在这个地方我们可以操作的不多，不过一般要做的是在这边替换内容，比如你发现一个文字要去掉，但是一时找不到在哪里怎么办？在这边做临时的规律，替换掉即可。

从上面我们知道重点要找parseCommon()方法（在ASP中称之为函数），我们查看它的代码（代码被我简化了，位于inc/AspCms_MainClass.asp内的parseCommon()方法内）：
<pre>Public Function parseCommon()
    ......
    parseLinkList()
    ......
    parseLoop("type")
    parseLoop("about")
    <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">parseLoop("content")</span></span>
    parseLoop("tag")
    ......
End Function</pre>
看到什么了呢？好多的parseLoop函数的调用，然后，好神奇的有一个参数正好是content的，上面代码中，使用红色标出来的地方。好明显，这不是要让你一定要查看一下这个方法的内容吗，我们查看一下parseLoop函数的作用。

查看到吗找到parseLoop函数的时候我们能看到他的注释上写着：替换循环标签，原来你在这里哈~这个函数就是专门处理{aspcms:content...}..{/aspcms:content}这样子标签的关键所在。

PS：当然，你也许从这边也看到其他的内容，比如about，tag等内容，那你是不是想起来，你也需要某个专门的标签呢？是的，请在这边参加调用你的专门标签处理函数即可。

等你完整的阅读完parseLoop函数之后你就知道他就是一个获取标签参数构建SQL语句并执行之后获取需要的数据再获取标签的专门调用字段做替换，再输出最后的结果的。那么只要在输出的时候针对数据的获取采取一点点改变，在获取一下分页信息，并且输出不就可以了呢？

ok，思路完成。