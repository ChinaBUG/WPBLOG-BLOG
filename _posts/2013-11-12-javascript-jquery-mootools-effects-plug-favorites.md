---
ID: 2664
post_title: >
  Javascript/JQuery/Mootools效果插件收藏
author: ChinaBUG
post_excerpt: |
  Mootools
  1、遮罩层插件：
  2、让Element取得索引值的方法,类似jquery的Element.index()
  3、如何让div或者其他对象居中显示呢？
  ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~
  JQuery
  1、遮罩层插件
  2、掩码格式化插件（自动格式化输入的电话号码邮政编码电话号码手机号码银行卡信用卡）
  3、如何让div或者其他对象居中显示呢？
  4、日期时间选择器（为jQuery UI Datepicker日期插件增加Timepicker 时间插件功能）
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/javascript-jquery-mootools-effects-plug-favorites.html
published: true
post_date: 2013-11-12 14:00:53
---
<span style="text-decoration: underline;"><strong>Javascript</strong></span>

1、文件上传：javascript-image-upload-master

网址：https://github.com/joelvardy/Javascript-image-upload

点评：很不错的原生的一个上传插件。

2、一键文件上传插件：upclick

网址：https://code.google.com/p/upload-at-click/

点评：原生，无污染，定制也方便，原生插件最值得推荐的一款上传插件了。。主要是可以定制好多东西。最关键是支持ie6哈

<span style="text-decoration: underline;"><strong>Mootools</strong> </span>

1、遮罩层插件

网址：http://davidwalsh.name/mootools-overlay

点评：不知道是不是我不会用，使用的时候，明显不像演示之中的那样子，还需要自己自定义~~~留待后面再测试吧

2、让Element取得索引值的方法,类似jquery的Element.index()

[code lang="javascript"]//http://www.cnblogs.com/see7di/archive/2011/11/24/2261103.html&lt;br /&gt;
Element.implement({&lt;br /&gt;
index:function(){&lt;br /&gt;
var sib=this.getParent().getChildren();&lt;br /&gt;
for(var i=0;i&amp;lt;sib.length;i++){&lt;br /&gt;
if(sib[i]==this){&lt;br /&gt;
sib=null;&lt;br /&gt;
return i;&lt;br /&gt;
}&lt;br /&gt;
}&lt;br /&gt;
}&lt;br /&gt;
});[/code]

3、如何让div或者其他对象居中显示呢？

[code lang="javascript"]Element.implement ({&lt;br /&gt;
/**&lt;br /&gt;
* Centers vertically this Element in its parent element.&lt;br /&gt;
* Makes this element absolute and its parent at least relative.&lt;br /&gt;
* Might wreak havoc in the element's horizontal centering,&lt;br /&gt;
* because the `left` property will be 0. (the default)&lt;br /&gt;
*&lt;br /&gt;
* @param  Element parent Optional. The parent element, defaults to the first parent.&lt;br /&gt;
* @return Element This Element&lt;br /&gt;
*/&lt;br /&gt;
centerVertically: function(parent){&lt;br /&gt;
parent = document.id(parent); if (!parent) parent = this.getParent();&lt;br /&gt;
var top = parent.getSize().y / 2 - this.getSize().y / 2;&lt;br /&gt;
if (parent.getStyle('position') == 'static')&lt;br /&gt;
parent.setStyle('position', 'relative');&lt;br /&gt;
if (this.getStyle('position') == 'static')&lt;br /&gt;
this.setStyle('position', 'absolute');&lt;br /&gt;
this.setStyle('top', top.toInt());&lt;br /&gt;
return this;&lt;br /&gt;
}&lt;/p&gt;
&lt;p&gt;});[/code]

上面是定位垂直居中，再来个全套的吧

[code lang="javascript"]&lt;br /&gt;
var ScreenCentre = new Class({&lt;br /&gt;
Implements: Options,&lt;br /&gt;
options: {&lt;br /&gt;
createIfAbsent: true,&lt;br /&gt;
divToCentre: 'import'&lt;br /&gt;
},&lt;br /&gt;
initialize: function(options) {&lt;br /&gt;
this.setOptions(options);&lt;br /&gt;
this.bound = {&lt;br /&gt;
'window': {&lt;br /&gt;
resize: this.centreDiv.bind(this),&lt;br /&gt;
scroll: this.centreDiv.bind(this)&lt;br /&gt;
}&lt;br /&gt;
};&lt;br /&gt;
if (!$(this.options.divToCentre) &amp;amp;&amp;amp; this.options.createIfAbsent) {&lt;br /&gt;
// DIV not in HTML, so create it&lt;br /&gt;
new Element('div', {&lt;br /&gt;
'id': this.options.divToCentre&lt;br /&gt;
}).inject(document.body).setStyle('display','block');&lt;br /&gt;
}&lt;br /&gt;
this.centreDiv(); // centres DIV on screen&lt;br /&gt;
window.addEvents(this.bound.window); // make the window call the centreDiv function when it's resized or scrolls&lt;br /&gt;
},&lt;br /&gt;
centreDiv: function() {&lt;br /&gt;
var div = $(this.options.divToCentre);&lt;br /&gt;
var size = div.getSize();&lt;br /&gt;
div.setStyles({&lt;br /&gt;
'display': 'block',&lt;br /&gt;
'left': ((window.getWidth()-size.x)/2)+'px',&lt;br /&gt;
'position': 'absolute',&lt;br /&gt;
'top': ((window.getHeight()-size.y)/2)+window.getScrollTop()+'px',&lt;br /&gt;
'visibility': 'visible',&lt;br /&gt;
'zIndex': 1000&lt;br /&gt;
});&lt;br /&gt;
}&lt;br /&gt;
});[/code]

要说明的是，上面两端代码我测试均出现错误~~不知道为啥~哎，最后是自己写一个来替代了~
~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~ ~.-.~

<span style="text-decoration: underline;"><strong>JQuery</strong> </span>

1、遮罩层插件

网址：http://code.google.com/p/jquery-loadmask/

点评：不错，简单易用。

用法：

//打开遮罩 $('#maskit').mask("正在努力获取商品中......");

//遮罩关闭 $('#maskit').unmask();

2、掩码格式化插件（自动格式化输入的电话号码邮政编码电话号码手机号码银行卡信用卡）

网址：http://igorescobar.github.io/jQuery-Mask-Plugin/

点评：文档完整，实例完整，而且比较易用的一个 用法：直接查看 <a href="http://igorescobar.github.io/jQuery-Mask-Plugin/">官方网站</a>

3、如何让div或者其他对象居中显示呢？

[code lang="javascript"](function($){&lt;br /&gt;
$.fn.extend({&lt;br /&gt;
center: function () {&lt;br /&gt;
return this.each(function() {&lt;br /&gt;
var top = ($(window).height() - $(this).outerHeight()) / 2;&lt;br /&gt;
var left = ($(window).width() - $(this).outerWidth()) / 2;&lt;br /&gt;
$(this).css({position:'absolute', margin:0, top: (top &amp;gt; 0 ? top : 0)+'px', left: (left &amp;gt; 0 ? left : 0)+'px'});&lt;br /&gt;
});&lt;br /&gt;
}&lt;br /&gt;
});&lt;br /&gt;
})(jQuery);[/code]

4、日期时间选择器（为jQuery UI Datepicker日期插件增加Timepicker 时间插件功能）

网址：http://trentrichardson.com/examples/timepicker/

点评：很棒的扩展，用这个我估计是Jquery日期时间插件中我用过最好的。

5、AJax精简文件上传插件：Mini AJAX File Upload Form

网址：http://tutorialzine.com/2013/05/mini-ajax-file-upload-form/

点评：很不错的一个插件效果，值得推荐

6、dropzone文件上传插件：https://github.com/enyo/dropzone

点评：一般吧，没有上面的好，个人觉得

7、<a href="http://tutorialzine.com/2014/05/jquery-image-slideshow-plugin/">jQuery Image Slideshow Plugin</a>

点评：很不错的一个图片幻灯插件，避免第一张小图也参与轮播。最关键是开发思路非常不错。

8、