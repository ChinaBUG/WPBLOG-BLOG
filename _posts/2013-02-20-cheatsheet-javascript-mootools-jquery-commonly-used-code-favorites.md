---
ID: 2358
post_title: >
  CheatSheet-Javascript/Mootools/JQuery常用代码收藏
author: ChinaBUG
post_excerpt: |
  1、Javascript:object转string
  2、Mootools:indexOf方法
  3、Mootools:Element.Delegation - 想要监控新增加之后的元素？请使用relay方法吧
  4、Mootools怎么定时执行一个方法函数
  5、JS检查浏览器版本
  6、怎么判断JS库是否加载完成或者怎么判断是否存在JS库框架
  7、获取按下的按键的值
  8、Mootools对象聚焦focus操作
  9、Mootools自动触发对象事件（像JS的fireEvent方法）
  10、JQuery自动触发对象事件（像JS的fireEvent方法）
  11、函数的参数序列，不固定的参数数量
  12、Mootools 怎么停止事件的触发，或者说怎么停止事件
  13、怎么检测一个Javascript的类有没有加载创建
  14、表单提交验证
  15、如何获取select下拉列表的值的索引与怎么设置下拉列表的值
  16、JS中怎么解析JSON字符串呢？
  17、Mootools怎么提交表单(form)内的全部表单内容？
  18、jQuery如何获取一个URL的完整地址或者获取如何获取一个URL的完整形式
  19、如何实现选择排除自身之外的对象？
  20、Mootools监视“后面”变动的事件
layout: post
permalink: >
  http://blog.ipodmp.com/2013/02/cheatsheet-javascript-mootools-jquery-commonly-used-code-favorites.html
published: true
post_date: 2013-02-20 17:46:02
---
自学的基础就是差呀，经常记得这个忘记那个，需要用到时才会发现，原来那个点自己没学到。记录一下吧。

++++
<pre>1、Javascript:object转string

var obj=$('some...');

String(obj).test('some...')

2、Mootools:indexOf方法

Mootools是检查数组内是否包含某个字符串，要是需要检查字符串是否存在则需要使用.test或者转为字符串类型。如上。

3、Mootools:Element.Delegation - 想要监控新增加之后的元素？请使用relay方法吧

http://mootools.net/demos/?demo=Element.Delegation

注：Jquery使用<code>delegate</code>就可以监听新增加的元素

4、Mootools怎么定时延迟延时执行一个方法函数

var timer;
var endact = function(){clearInterval(timer); };
//3秒
timer = endact.periodical(3000);

5、JS检查浏览器版本

var getOs = function(){
if(navigator.userAgent.indexOf("MSIE")&gt;0){return "MSIE";}
if(isFirefox=navigator.userAgent.indexOf("Firefox")&gt;0){return "Firefox";}
if(isSafari=navigator.userAgent.indexOf("Safari")&gt;0){return "Safari";}
if(isCamino=navigator.userAgent.indexOf("Camino")&gt;0){return "Camino";}
if(isMozilla=navigator.userAgent.indexOf("Gecko/")&gt;0){return "Gecko";}
}

6、怎么判断JS库是否加载完成或者怎么判断是否存在JS库框架
&lt;script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js"&gt;&lt;/script&gt;
&lt;script&gt;!window.jQuery &amp;&amp; document.write('&lt;script src="jquery-1.4.3.min.js"&gt;&lt;/script&gt;');&lt;/script&gt;

7、获取按下的按键的值

&lt;script event="onkeydown" for="document"&gt;
alert(event.keyCode)
&lt;/script&gt;

IE的event属性
event.button：按下的鼠标键。对于鼠标左键，这个属性值为1，而对于鼠标右键，则通常为2；
event.clientX：事件发生位置的x轴坐标（列，以像素为单位）；
event.clientY：事件发生位置的y轴坐标（行，以像素为单位）；
event.altkey：该标志表示事件发生时是否按下了Alt键；
event.ctrlkey：该标志表示事件发生时是否按下了Ctrl键；
event.shiftkey：该标志表示时间发生时是否按下了Shift键；
event.keyCode：所按键的键码（用Unicode表示）；
event.srcElement：元素出现的对象。
Netscape和Firefox的event属性
event.modifiers：表示时间发生时按下了哪一个修饰键（Shift、Ctrl和Alt等）。该属性值是一个整数，表示了不同键的二进制值的组合；
event.pageX：事件在网页中的x轴坐标；
event.pageY：事件在网页中的y轴坐标；
event.which：键盘事件的键码（用Unicode表示），或者是鼠标事件按下的键（最好使用跨浏览器的button属性）；
event.button：按下的鼠标键，其原理与IE一样，只是左键的属性值为0，右键的属性值为2；
event.target：元素出现的对象。

8、Mootools对象聚焦focus操作

Element.implement({
setFocus: function(index) {
this.setAttribute('tabIndex',index || 0);
this.focus();
}
});

$("name").setFocus();

9、Mootools自动触发对象事件（像JS的fireEvent方法）

Ele.fireEvent(mootool_event)

如：一载入自动打开客服系统

$('siderIMchat_hiddenbar').fireEvent('click');

10、JQuery自动触发对象事件（像JS的fireEvent方法）

$(<i>selector</i>).trigger(<i>event</i>,[<i>param1</i>,<i>param2</i>,...])

11、函数的参数序列，不固定的参数数量
function doAdd() {
  if(arguments.length == 1) {
    alert(arguments[0] + 5);
  } else if(arguments.length == 2) {
    alert(arguments[0] + arguments[1]);
  }
}
doAdd(10);	//输出 "15"
doAdd(40, 20);	//输出 "60"

12、Mootools 怎么停止事件的触发，或者说怎么停止事件
event.stop()

13、怎么检测一个Javascript的类有没有加载创建（同6）
if("undefined" == (typeof Scroller)){
    //没有创建怎么办？
}

14、表单提交验证
form 设置 onsubmit="return checkdata();"
在checkdata方法内返回true或者false就可以了，返回false则取消提交，其他情况为提交。
这个重点在于onsunmit事件里面的return关键词，一定要加！

15、如何获取select下拉列表的值的索引与怎么设置下拉列表的值
//根据select的ID获取相应值的索引
function getselectedIndex(id,val){
    var obj = $(id);
    for(i=0;i&lt;obj.options.length;i++){
        if(obj.options[i].value == val) return i;
    }
}
//根据select的ID获取相应值的索引并设置
function setselectedIndex(id,val){
    var obj = $(id);
    for(i=0;i&lt;obj.options.length;i++){
        if(obj.options[i].value == val) obj.selectedIndex = i;
    }
}

16、JS中怎么解析JSON字符串呢？</pre>
var json='{"name":"CJ","age":18}';
data =(new Function("","return "+json))();

此时的data就是一个会解析成一个json对象了。

17、Mootools怎么提交表单(form)内的全部表单内容？

$(‘form1’).toQueryString()

就可以将form1里面的全部表单内容都自动打包成查询字符串，如abc=1&amp;bcd=2&amp;cde=3...当然，你还可以在使用Ajax的时候直接post整个表单。

实例如下：

var myHTMLRequest = new Request.HTML({
url: '/demo.html',
<span style="color: #ff00ff;">data: $('form1').toQueryString(),</span>
onSuccess: function(responseText, responseXML){
alert(responseText);
}
}).post();

或者

var myHTMLRequest = new Request.HTML({
url: '/demo.html',
onSuccess: function(responseText, responseXML){
alert(responseText);
}
}).post(<span style="color: #ff00ff;">$('form1')</span>);

18、jQuery如何获取一个URL的完整地址或者获取如何获取一个URL的完整形式

比如：wap/bac.html这个链接，要如何自动变成完整的链接，如http://blog.ipodmp.com/wap/bac.html这个形式呢？

用JQ其实很容易，代码如下：

var myA = $('&lt;a&gt;&lt;/a&gt;'),
fullUrl = '';
fullUrl = myA.prop('href',"wap/bac.html").prop('href');

19、如何实现选择排除自身之外的对象？

在jQuery中，我们可以使用not方法来排除，在Mootools中我们则需要使用getSiblings。

JQ

$('div').not('.not').removeClass('.on');

Mootools

$('div').each(function(ele,index){
ele.getSiblings().removeClass('.on');
});

20、Mootools监视“后面”变动的事件

$('main').addEvents('click:relay(.btn-delete)',function(e) {});

21、