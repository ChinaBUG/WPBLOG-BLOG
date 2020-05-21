---
ID: 2918
post_title: >
  ShopEX-Mootools框架之Switchable封装Slide卡盘效果Tab标签页效果Accordion手风琴效果Carousel旋转木马效果Popup弹出层效果DataLazyload延迟加载机制Countdown倒计时效果
author: ChinaBUG
post_excerpt: |
  虽然，ShopEX东家商派很糟糕，我是这么觉得的，论坛几百年不见得有什么大的声音，不过偶尔看看他们家的ShopEx模板开发手册(模板开发|模板制作|模板教程)，每每都有收获~这是一个好事~就是里面的交流mail不知道什么时候能开通，能正常接受反馈呢？！
  这不今天来推荐一下他们家的Switchable框架封装吧。
  如果有做过淘宝的模板装修的话，应该知道淘宝提供了好多的widget库：Tabs标签页、Slide卡盘效果、Carousel旋转木马、Accordion手风琴、Popup弹出层、Countdown倒计时、Compatibe兼容性组件等效果是不是很方便，很好用呢？！那你想不想在ShopEX之中应用这类效果呢？让我们的插件、模板开发更加简单，方便又时尚呢？
  参考网站：淘宝网 店铺开发文档 Widget规范
  今天的主角就是ShopEX用来实现这些效果的利器！
  参考网站：Mootools框架
  需要更多详细的内容请点击上面的参考网站。那么来说说怎么使用吧，毕竟上面的参考网站很多的说明都有，我这边就不重复了。
  下载回来的是内核文档跟实例文档，解压之后会发现有如下结构：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-switchable-package-for-mootools-framework-as-slide-tab-accordion-datalazyload-etc.html
published: true
post_date: 2013-11-28 10:10:43
---
虽然，ShopEX东家商派很糟糕，我是这么觉得的，论坛几百年不见得有什么大的声音，不过偶尔看看他们家的<a href="http://wiki.zx.shopex.cn">ShopEx模板开发手册(模板开发|模板制作|模板教程)</a>，每每都有收获~这是一个好事~就是里面的交流mail不知道什么时候能开通，能正常接受反馈呢？！

这不今天来推荐一下他们家的Switchable框架封装吧。

如果有做过淘宝的模板装修的话，应该知道淘宝提供了好多的widget库：Tabs标签页、Slide卡盘效果、Carousel旋转木马、Accordion手风琴、Popup弹出层、Countdown倒计时、Compatibe兼容性组件等效果是不是很方便，很好用呢？！那你想不想在ShopEX之中应用这类效果呢？让我们的插件、模板开发更加简单，方便又时尚呢？
<blockquote><em><a href="http://wiki.zx.taobao.com/Widget%E8%A7%84%E8%8C%83" target="_blank">参考网站：淘宝网 店铺开发文档 Widget规范</a></em></blockquote>
今天的主角就是ShopEX用来实现这些效果的利器！
<blockquote><em><a href="http://wiki.zx.shopex.cn/advance/mootools.html" target="_blank">参考网站：Mootools框架</a></em></blockquote>
需要更多详细的内容请点击上面的参考网站。那么来说说怎么使用吧，毕竟上面的参考网站很多的说明都有，我这边就不重复了。

下载回来的是内核文档跟实例文档，解压之后会发现有如下结构：
<pre><em>[assets]
[source]
accordion1.html
accordion2.html
accordion3.html
carousel1.html
carousel2.html
carousel3.html
countdown1.html
countdown2.html
countdown3.html
......</em></pre>
文件夹assets内包含实例用到的图片资源，而文件夹source里面则是我们的框架内核与我们的Switchable代码了。

正常的直接点击案例文件就可以查看效果了，这边说一下怎么弄到ShopEX里面噢。

其实很简单，只需要再要使用效果的时候增加Switchable的引用即可，而框架文件则不需要载入了，因为ShopEX系统本身就已经存在Mootools框架了。

除了在使用的地方引用之外，还可以直接在头部文件中一次性载入。

代码如下：

1、请将switchable.js拷贝到/statics/script/内，然后使用下面代码载入：

&lt;script src="/statics/script/switchable.js"&gt;&lt;/script&gt;

一般如果全局调用的话我都是会放进这个地方的，这样子容易做迁移等操作而不会遗忘。

2、请将switchable.js拷贝到需要调用效果的挂件文件夹内，假设挂件名字叫做cb_rightpop，那么一般都是放在images或者js文件夹内：

&lt;script src="/plugins/widgets/cb_rightpop/images/switchable.js"&gt;&lt;/script&gt;

然后按照手册的效果来写结构，系统将会自动调用效果的。

很多人可能会问怎么制作基于mootools版的ShopEX商城系统图片延迟加载效果（懒加载）插件，其实使用这个插件之后很容易噢~~具体可以查看参考手册噢。