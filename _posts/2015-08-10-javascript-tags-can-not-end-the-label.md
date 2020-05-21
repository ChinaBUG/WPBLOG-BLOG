---
ID: 4164
post_title: 'javascript-&lt;script&gt;标签可以没有结束标签？'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/javascript-tags-can-not-end-the-label.html
published: true
post_date: 2015-08-10 17:46:39
---
上周同事在开发ECStore晒单功能的时候，因为他的脚本使用的是jQuery脚本库，所以自然需要加载库了~按照求稳的办法，就是在页头最先加载jQuery库，然后noConflict一下，交出$美元符号的占用给下面加载的Mootools脚本库，经过测试这个是最保险的加载方式，但是同事居然加载不成功，错误一大堆~

至于ShopEX4.85系列的加载方法请参考之前的文章：<a href="http://blog.ipodmp.com/archives/shopex-javascript-simultaneous-use-of-the-jquery-with-mootools-two-kinds-of-javascript-framework-compatible/">ShopEX-同时使用jQuery与Mootools两种Javascript框架的兼容</a>

我同事使用代码如下
<pre>&lt;script type="text/javascript" src="/public/app/b2c/statics/js_mini/jquery-1.11.3.min.js"/&gt;
&lt;script type="text/javascript"&gt;
var jq=$.noConflict();
&lt;/script&gt;
...
&lt;script src="http://192.168.1.12:86/public/app/site/statics/js_mini/moo.min.js"&gt;&lt;/script&gt;</pre>
一般调试，我喜欢加上一个alert方法哈，第一步思考的是不是冲突了，然后在分别的代码里面写上代码，改为如下：
<pre>&lt;script type="text/javascript" src="/public/app/b2c/statics/js_mini/jquery-1.11.3.min.js"/&gt;
&lt;script type="text/javascript"&gt;
alert('jQuery....');
var jq=$.noConflict();
&lt;/script&gt;
...
&lt;script src="http://192.168.1.12:86/public/app/site/statics/js_mini/moo.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript"&gt;
alert('Mootools....');
&lt;/script&gt;</pre>
然后执行之后发现一个神奇的事情：那就是输出
<pre>Mootools....</pre>
而jQ那边的代码完全忽略了~一般这个问题是不会出现的，因为不管JS库在有没有载入，这个alert总是会执行的，除非遇上错误干扰了，但是我们就只是加载，库是没有问题的，检查一下加载的库文件也是完整，加载成功的。

再来改一下：
<pre>&lt;script type="text/javascript" src="/public/app/b2c/statics/js_mini/jquery-1.11.3.min.js"/&gt;
&lt;script type="text/javascript"&gt;
jQuery(function(){
    alert('jQuery....');
});
var jq=$.noConflict();
&lt;/script&gt;
...
&lt;script src="http://192.168.1.12:86/public/app/site/statics/js_mini/moo.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript"&gt;
window.addEvent('domready',function(){
     alert('Mootools....');
});
&lt;/script&gt;</pre>
然后再来试试看呗，结果还是一样的，在jQuery这段直接跳过去了，为啥呢？

......

陪着同事在他的电脑上调试半天，疯了，他是Mac电脑，那个操作，那个不习惯，算了，会自己的机子上慢慢调试吧。

然后，回来了，问题解决了。

原因就在于我重新开了一下代码，发现一个莫名其妙的问题！！！

居然，他在jQuery库加载的时候居然没有写结束标签，而是直接给反斜杠处理了，这个语法，有点特别，我怎么没见过呢？！

给他纠正一下，然后，脚本终于正常了~~~^_^

最后问了一下才知道他们以前工作的时候都是这么写的，都没见过这个错误。

好吧，我猜了一下，把下面的代码给转移一个地方，不让他紧靠上面的标签了，然后问题解决了。

请不要随便使用莫名其妙的语法了~~其他浏览器没有测试不知道会不会出现这个问题，反正，火狐还有Mac上的Chrome是不能使用的。