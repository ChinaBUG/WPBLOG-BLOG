---
ID: 3982
post_title: Sublime Text 3 自动补全深入定制
author: ChinaBUG
post_excerpt: |
  延伸阅读：关于snippets.json
  延伸阅读：关于.sublime-snippet文件的说明 关于scope的讲解
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/sublime-text-3-customized-auto-completion.html
published: true
post_date: 2015-03-25 17:38:54
---
去年换了Sublime Text 3这个编辑器，非常不错，最近在做ShopNC的模板的时候发现，要写好多的中间代码，累死了，最近手指头有点疼痛，估计有毛病了，需要好好爱护了噢。

如果你有安装Emmet这个插件，你就知道只需要输入关键词，按下Ctrl+E就会相应的给你生成你需要的代码噢。

比如：输入ul&gt;li*5按下Ctrl+E，然后你就会发现编辑器已经自动帮你生成如下代码：

&lt;ul&gt;
&lt;li&gt;&lt;/li&gt;
&lt;li&gt;&lt;/li&gt;
&lt;li&gt;&lt;/li&gt;
&lt;li&gt;&lt;/li&gt;
&lt;li&gt;&lt;/li&gt;
&lt;/ul&gt;

方便吧？

好了，现在我们也来定制一下。

请打开Sublime Text 3安装目录，然后依次进入\Data\Packages\Emmet\emmet\找到snippets.json文件，打开它，你会发现很多的代码。里面就是定制好的格式了~

我们就在开头修改一下，代码如下：

"variables": {
"lang": "en",
"locale": "en-US",
"charset": "UTF-8",
"indentation": "\t",
"newline": "\n"
},
/* 2015.03.25 ChinaBUG */
"php": {
"filters": "html,php",
"snippets": {
'php':'&lt;?php | ?&gt;',
'phpecho':'&lt;?php echo "|";?&gt;',
'phpout':'&lt;?php echo \$output["|"];?&gt;',
'tpl':"Tpl::output('${1:name}|',${2:\\$name});"
}
},

然后保存，关闭Sublime Text再打开一下就可以了，现在我们输入php按下Ctrl+E，看看是不是生成了&lt;?php  ?&gt;标签对呢？

呵呵，简单吧。

具体信息可以参考英文的官方资料自行研究噢。

延伸阅读：<a href="http://docs.emmet.io/customization/snippets/">关于snippets.json</a>

当然，这个是插件，替我们节省更多的时间哈，如果你很懒，懒到连Ctrl+E都不愿意按的话，那么只能使用Sublime Text本身的自动完成了。

具体是新建一个你需要的快捷键的信息文件，然后就可以了。

官方说这个文件要放哪里都可以，不过还是建议放在Sublime Text 3\Data\Packages\User\里面，方便管理，反正我是在这个文件夹里面新建一个我自己的文件夹哈。

请按照下面的格式新建一个文档：

&lt;snippet&gt;
&lt;content&gt;
&lt;![CDATA[&lt;?php echo \$${1:output}[''];?&gt;]]&gt;
&lt;/content&gt;
&lt;!-- Optional: Set a tabTrigger to define how to trigger the snippet --&gt;
&lt;tabTrigger&gt;phpoutput&lt;/tabTrigger&gt;
&lt;!-- Optional: Set a scope to limit where the snippet will trigger --&gt;
&lt;!-- &lt;scope&gt;source.html&lt;/scope&gt; --&gt;
&lt;/snippet&gt;

保存为phpoutput.sublime-snippet文件，文件名你可以随意，但是扩展是不能修改的。然后保存，在Sublime Text里面输入php...就会看到提示框里面出现他的匹配了，点击就可以出现我们设定好的代码了。

噢，一个提示一个文件，不要放在一个文件里面，经试验，只有第一个是有效的，所以有多少的规则就新建多少个文件吧。

延伸阅读：<a href="http://docs.sublimetext.info/en/latest/extensibility/snippets.html">关于.sublime-snippet文件的说明</a> <a href="http://docs.sublimetext.info/en/latest/extensibility/syntaxdefs.html">关于scope的讲解</a>