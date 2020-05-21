---
ID: 2302
post_title: >
  ShopEX-同时使用jQuery与Mootools两种Javascript框架的兼容
author: ChinaBUG
post_excerpt: >
  众所周知，ShopEX使用Mootools脚本框架开发的，那个悲剧呀，很多优秀的JQuery的插件不能用噢，怎么办？看得到吃不到？肯定不可能的嘛~下面就是提供同时使用jQuery与Mootools两种Javascript框架的兼容方法，具体看例子噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/shopex-javascript-simultaneous-use-of-the-jquery-with-mootools-two-kinds-of-javascript-framework-compatible.html
published: true
post_date: 2013-01-29 11:58:14
---
众所周知，ShopEX使用Mootools脚本框架开发的，那个悲剧呀，很多优秀的JQuery的插件不能用噢，怎么办？看得到吃不到？肯定不可能的嘛~下面就是提供同时使用jQuery与Mootools两种Javascript框架的兼容方法，具体看例子噢。

更多的信息及例子请参考：<a href="http://www.w3school.com.cn/jquery/core_noconflict.asp">jQuery 核心 - noConflict() 方法</a>

例子来源于<a href="http://davidwalsh.name/jquery-mootools">Using jQuery and MooTools Together</a>

[html]
  &lt;script type=&quot;text/javascript&quot; src=&quot;jquery-1.3.js&quot;&gt;&lt;/script&gt;
  &lt;script type=&quot;text/javascript&quot;&gt;
    //no conflict jquery
    jQuery.noConflict();
    //jquery stuff
    (function($) {
      $('p').css('color','#ff0000');
    })(jQuery);
  &lt;/script&gt;
  &lt;script type=&quot;text/javascript&quot; src=&quot;moo1.2.js&quot;&gt;&lt;/script&gt;
  &lt;script type=&quot;text/javascript&quot;&gt;
    //moo stuff
    window.addEvent('domready',function() {
      $$('p').setStyle('border','1px solid #fc0');
    });
  &lt;/script&gt;
[/html]

按照上面的例子的做法肯定是可以执行的，然后你就会发现世界是很美好的。但是，当你使用在SHopEX里面时你就会发现，一会儿晴天一会儿阴天！为什么呢？你可能会发现按照上面的代码做，可能照成原来系统功能的不正常运作。比如客服系统没办法打开？或者其他问题。

你是不是也跟我一样找了又找，想了又想，却不得其解呢？

让我来告诉你吧，我的思考成果！~~其实是同事发现的哈~~

会出现问题的原因就在于脚本库加载的顺序！

嗯，就是 JQuery这个外来户的加载要在Mootools之前！如下面的例子：

[html]&lt;script type=&quot;text/javascript&quot; src=&quot;images/jquery.min.js&quot;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot;&gt;
var jq=$.noConflict();
&lt;/script&gt;
&lt;script src=&quot;statics/miao/js/slides.min.jquery.js&quot;&gt;&lt;/script&gt;
&lt;{header}&gt;[/html]

这样加载就是保证了JQuery在Mootools之前加载完毕。一般的功能都可以正常使用了！