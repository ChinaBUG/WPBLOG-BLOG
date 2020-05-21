---
ID: 2384
post_title: >
  jQuery-前端必备：jQuery 1.7.1
  API手册
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: 'http://blog.ipodmp.com/2013/03/jquery-%e5%89%8d%e7%ab%af%e5%bf%85%e5%a4%87%ef%bc%9ajquery-1-7-1-api%e6%89%8b%e5%86%8c.html'
published: true
post_date: 2013-03-02 21:00:20
---
本文基于jQuery1.7.1版本，是对官方API的整理和总结，完整的官方API见http://api.jquery.com/browser/

<strong>0、总述</strong>

jQuery框架提供了很多方法，但大致上可以分为3大类：获取jQuery对象的方法、在jQuery对象间跳转的方法，以及获取jQuery对象后调用的方法

其中第一步是怎样获取jQuery对象。大致来说，是通过最核心的$()方法，将页面上的元素(或者在页面上不存在的html片段)包装成jQuery对象。

$()方法里面支持的语法又包括3大类，分别是表达式(包括类表达式.，id表达式#，元素表达式等)、符号(包括后代符号space，next符号+等)、过滤器(包括:过滤器和[]过滤器)。

通过以上3种的组合，“查询”得到想要操作的元素或者元素集合，作为$()的参数，得到jQuery对象(或者jQuery对象的集合)

第二步是在jQuery对象间的跳转。也就是说，已经得到了一个jQuery对象，但是并不是想要的，那么可以通过一系列的跳转方法，比如 parent()、next()、children()、find()等，或者过滤筛选的方法，比如eq()、filter()、not()等，来得到最 终想要操作的jQuery对象。

用跳转和过滤方式得到的jQuery结果，往往通过比较复杂的表达式组合，可以达到同样的目的。

比如说$("div").eq(3)，也可以用$("div:eq(3)")达到同样的目的。

又比如说$("div").find("span")，可以用$("div span")取到同样的元素。

方法是很灵活的，要根据具体的情况来选择。一般来说，HTML页面写得越规范，使用jQuery就越简单

还有一种情况，在得到了jQuery()对象之后，想要判断其是否满足条件，那么可以调用is()、hasClass()等方法，返回一个boolean值，进行后续的判断。这类方法也可以归到这类。

第三步是在获取准确的jQuery对象之后，调用其上的各种方法，来进行操作。这一步反而是比较简单的了。

后面就是对jQuery框架各种方法的简要介绍，更详细的内容，还是以官方API为准

<strong>1、$(...)</strong>

$() 一切的核心，可以跟4种参数

$(expression)，比如$("#id")、$(".class")等，返回jQuery对象，或者jQuery对象的集合

$(html)，比如$("&lt;span&gt;hello world&lt;/span&gt;")，返回jQuery对象，或者jQuery对象的集合

$(element)，比如$(document.body)，返回jQuery对象，或者jQuery对象的集合

$(*)，所有元素

<strong>2、jQuery Object Accessors</strong>

jQuery.index(element)，返回该jQuery对象在集合中的索引

jQuery.each(function)，遍历jQuery对象集合，在每个对象上执行function函数，function callback(index, domElement){this};

jQuery.size()，返回jQuery对象集合的大小

jQuery.length，相当于size()方法

jQuery.get()，获取原生DomElement对象的Array

jQuery.get(index)，获取原生DomElement对象

jQuery.eq(position)，获取jQuery对象集合中的一个jQuery对象

<strong>3、Data相关方法</strong>

jQuery.data(name)

jQuery.data(name, value)

jQuery.removeData(name)

<strong>4、选择符</strong>

multiple(selector1, selector2)，可以选择多个元素或者表达式，包装成jQuery对象的集合

例子：$("div,span")

id(id)

例子：$("#id")

class(class)

例子：$(".class")

element(element)

例子：$("div")

all

例子：$("*")

descendant

例子：$("table tr td")

child(parent, child)

例子：$("#id &gt; span")，和上一个descendant的区别在于，descendant只要是后代就会被选中，而child必须是直接子节点，不包括孙子节点

next(prev, next)

例子：$("label + input")，选中的是label标签的下一个input标签，返回jQuery对象的集合

siblings(prev, siblings)

例子：$("#prev ~ div")，选中的是#prev之后的所有div标签，返回jQuery对象的集合，有点像next，但是范围更大

Basic Filters

$(":header")，选中所有header，包括&lt;h1&gt;&lt;h2&gt;等

$("tr:odd")，选中所有奇数行

$("tr:even")，选中所有偶数行

$(":animated")，选中所有当前有特效的元素，$("div:animated")，选中当前所有有特效的&lt;div&gt;

$("tr:first")，选中第一行

$("tr:last")，选中最后一行

$("input:not(:checked)")，选中所有没有“checked”的input元素

$("td:gt(4)")，选中所有index是4之后的td

$("td:lt(4)")，选中所有index是4之前的td

$("td:eq(4)")，选中index是4的td，可以用$("td").eq(4)来实现同样的效果

Content Filters

$("div:contains('John')")，选中所有包含"John"字符串的div

$("td:empty")，选中所有内容为空的td

$("div:has(p)")，选中包含有&lt;p&gt;元素的&lt;div&gt;元素，返回jQuery对象集合

$("td:parent")，选中所有包含子节点的元素，包括文本也可以算是子节点

Visibility Filters

$("span:hidden")，选中所有隐藏的&lt;span&gt;

$("span:visible")，选中所有可见的&lt;span&gt;

Attribute Filters

$("div[id]")，选中包含id属性的&lt;div&gt;元素

$("input[name$='letter']")，选中包含某个属性的&lt;input&gt;元素，这个属性名是以'letter'结尾的

$("input[name^='letter']")，选中包含某个属性的&lt;input&gt;元素，这个属性名是以'letter'开头的

$("input[name*='man']")，选中包含某个属性的&lt;input&gt;元素，这个属性的属性名里包含'man'

$("input[name='newsletter']")，选中包含一个属性的&lt;input&gt;元素，这个属性的名字是'newsletter'

$("input[name!='newsletter']")，选中所有不包含'newsletter'属性的&lt;input&gt;元素

$("input[id][name$='man']")，选中包含id属性，和以'man'结尾属性的&lt;input&gt;元素

Child Filters

$("ul li:nth-child(2)")，选中自身是&lt;ul&gt;元素的第二个子节点的&lt;li&gt;元素，注意这个计算是从1开始的，不是从0开始

$("div span:firstChild")，选中自身是&lt;div&gt;元素的第一个子节点的&lt;span&gt;元素

$("div span:lastChild")，选中自身是&lt;div&gt;元素的最后一个子节点的&lt;span&gt;元素

$("div span:onlyChild")，选中自身是&lt;div&gt;元素的唯一子节点的&lt;span&gt;元素

Forms

$(":button")，所有&lt;button&gt;元素，和&lt;input type="button"&gt;元素

$("form :checkbox")，选中所有&lt;form&gt;标签下的&lt;input type="checkbox"&gt;，不过这样会比较慢，官方建议使用$("input:checkbox")

$(":file")，选中所有&lt;input type="file"&gt;

$(":hidden")，选中所有隐藏元素，以及&lt;input type="hidden"&gt;

$(":input")，选中所有&lt;input&gt;

$(":text")，选中所有&lt;input type="text"&gt;

$(":password")，选中所有&lt;input type="password"&gt;

$(":radio")，选中所有&lt;input type="radio"&gt;，不过这样会比较慢，建议使用$("input:radio")

$(":image")，选中所有&lt;input type="image"&gt;

$(":reset")，选中所有&lt;input type="reset"&gt;

$(":submit")，选中所有&lt;input type="submit"&gt;

Form Filters

$("input:enabled")，选中所有enabled的&lt;input&gt;元素

$("input:disabled")，选中所有disabled的&lt;input&gt;元素

$("input:checked")，选中所有checked的&lt;input type="checkbox"&gt;元素

$("input:selected")，选中所有selected的&lt;option&gt;元素

<strong>5、属性相关的方法 </strong>

jQuery.removeAttr(name)

jQuery.attr(name)，返回属性的值，比如$("img").attr("src")

jQuery.attr(key,value)，这是设置属性的值

jQuery.attr(properties)，也是设置属性的值

例子：
<ol>
	<li>$("img").attr({</li>
	<li>src: "/images/hat.gif",</li>
	<li>title: "jQuery",</li>
	<li>alt: "jQuery Logo"</li>
	<li>});</li>
</ol>
jQuery.attr(key,function)，也是设置属性的值，这个function计算出的结果，赋给key
<ol>
	<li>function callback(index) {</li>
	<li>// index == position in the jQuery object</li>
	<li>// this means DOM Element</li>
	<li>}</li>
</ol>
<strong>6、class相关的方法 </strong>

jQuery.toggleClass(class)，反复切换class属性，该方法第一次执行，增加class，然后去除该class，循环

jQuery.toggleClass(class,switch)，增加一个switch表达式

jQuery.hasClass(class)，返回boolean

jQuery.removeClass(class)，删除class

jQueyr.addClass(class)，增加class

<strong>7、HTML相关的方法 </strong>

jQuery.html()，返回包含的html文本

jQuery.html(val)，用val替换包含的html文本

<strong>8、文本相关的方法 </strong>

jQuery.text()，返回包含的纯文本，不会包括html标签，比如&lt;span&gt;abcd&lt;/span&gt;，调用.text()方法，只会返回abcd，不会返回&lt;span&gt;abcd&lt;/span&gt;
jQuery.text(val)，用val替换包含的纯文本，和html(val)方法的区别在于，所有的内容会被看作是纯文本，不会作为html标签 进行处理，比如调用.text("&lt;span&gt;abcd&lt;/span&gt;")，&lt;span&gt;和&lt; /span&gt;不会被认为是html标签

9、值相关的方法

jQuery.val()，返回string或者array

jQuery.val(val)，设置string值

jQuery.val(array)，设置多个值，以上3个方法，主要都是用在表单标签里，如&lt;input type="text"&gt;，&lt;input type="checkbox"&gt;等

10、在jQuery对象集合中进行过滤

以下几类方法有点像把选择符Filter进行方法化，比如$("label:eq(4)")，取到第4个&lt;label&gt;元素，这个就可以用$("label").eq(4)来替代，达到同样的效果

jQuery.is(expr)，返回boolean，比如$(this).is(":first-child")，判断一个元素，是不是其父节点的第一个子节点

jQuery.eq(index)，$("div").eq(2)，取出第2个&lt;div&gt;元素

jQuery.filter(expr)，比如$("div").filter(".middle")，会从div元素中筛选出属于middle的 class的元素；再比如$("p").filter(".selected, :first")，会取出是selected类，或者是第一个元素的&lt;p&gt;元素，这个可以用$("p.class, p:first")来代替这个方法，会从初始的结果集中，筛选保留一部分

jQuery.filter(fn)，类似于上一个函数，可以传进去一个function，用这个function的返回值，进行筛选
<ol>
	<li>function callback(index){</li>
	<li>// index == position in the jQuery object</li>
	<li>// this means DOM Element</li>
	<li>return boolean;</li>
	<li>}</li>
</ol>
jQuery.not(expr)，是和filter(expr)相反的方法，不是和is(expr)相反的方法。该方法把满足expr的元素给排 除掉，比如$("div").not(".green, #blue")，把class是green或者id是blue的元素过滤掉

jQuery.slice(start, end)，从jQuery对象集合中选出一段

jQuery.map(callback)，不知道是干嘛的

<strong>11、在jQuery对象之间查找 </strong>

jQuery.parent(expr)，找父亲节点，可以传入expr进行过滤，比如$("span").parent()或者$("span").parent(".class")

jQuery.parents(expr)，类似于jQuery.parent(expr)，但是是查找所有祖先元素，不限于父元素

jQuery.children(expr)，返回所有子节点，和parents()方法不一样的是，这个方法只会返回直接的孩子节点，不会返回所有的子孙节点

jQuery.contents()，返回下面的所有内容，包括节点和文本。这个方法和children()的区别就在于，包括空白文本，也会被作为一个jQuery对象返回，children()则只会返回节点

jQuery.prev()，返回上一个兄弟节点，不是所有的兄弟节点

jQuery.prevAll()，返回所有之前的兄弟节点

jQuery.next()，返回下一个兄弟节点，不是所有的兄弟节点

jQuery.nextAll()，返回所有之后的兄弟节点

jQuery.siblings()，返回兄弟姐妹节点，不分前后

jQuery.add(expr)，往既有的jQuery对象集合中增加新的jQuery对象，例子：$("div").add("p")

jQuery.find(expr)，跟jQuery.filter(expr)完全不一样。jQuery.filter()是从初始的 jQuery对象集合中筛选出一部分，而jQuery.find()的返回结果，不会有初始集合中的内容，比如$("p").find("span")， 是从&lt;p&gt;元素开始找&lt;span&gt;，等同于$("p span")

<strong>12、串联方法 </strong>

jQuery.andSelf()，把最后一次查询前一次的集合，也增加到最终结果集中，比 如$("div").find("p").andSelf()，这样结果集中包括所有的&lt;p&gt;和&lt;div&gt;。如果 是$("div").find("p")，那就只有&lt;p&gt;，没有&lt;div&gt;

jQuery.end()，把最后一次查询前一次的集合，作为最终结果集，比如$("p").find("span").end()，这样的结果集，是所有的&lt;p&gt;，没有&lt;span&gt;

<strong>13、DOM文档操作方法 </strong>

jQuery.append(content)，这个方法用于追加内容，比如$("div").append("&lt;span&gt;hello&lt;/span&gt;");

jQuery.appendTo(selector)，这个方法和上一个方法相反，比如$("&lt;span&gt;hello&lt;/span&gt;").appendTo("#div")，这个方法其实还有一个隐藏的move作用，即原来的元素被移动了

jQuery.prepend(content)，跟append()方法相对应，在前面插入

jQuery.prependTo(selector)，跟上一个方法相反

jQuery.after(content)，在外部插入，插入到后面，比如$("#foo").after("&lt;span&gt;hello&lt;/span&gt;");

jQuery.insertAfter(selector)，和上一个方法相反，比如$("&lt;span&gt;hello&lt;/span&gt;").insertAfter("#foo");

jQuery.before(content)，在外部插入，插入到前面

jQuery.insertBefore(selector)，跟上一个方法相反

jQuery.wrapInner(html)，在内部插入标签，比如$("p").wrapInner("&lt;span&gt;&lt;/span&gt;");

jQuery.wrap(html)，在外部插入标签，比如$("p").wrap("&lt;div&gt;&lt;/div&gt;")，这样的话，所有的&lt;p&gt;都会被各自的&lt;div&gt;包裹

jQuery.wrapAll(html)，类似上一个，区别在于，所有的&lt;p&gt;会被同一个&lt;div&gt;包裹

jQuery.replaceWith(content)，比如$(this).replaceWith("&lt;div&gt;"+$(this).text()+"&lt;/div&gt;");

jQuery.replaceAll(selector)，比如$("&lt;div&gt;hello&lt;/div&gt;").replaceAll("p");

jQuery.empty()，比如$("p").empty()，这样的话，会把&lt;p&gt;下面的所有子节点清空

jQuery.remove(expr)，比如$("p").remove()，这样的话，会把所有&lt;p&gt;移除，可以用表达式做参数，进行过滤

jQuery.clone()，复制一个页面元素

<strong>14、CSS相关方法 </strong>

jQuery.css(name)，获取一个css属性的值，比如$("p").css("color")

jQuery.css(object)，设置css属性的值，比如$("p").css({"color":"red","border":"1px red solid"});

jQuery.css(name,value)，设置css属性的值，比如$("p").css("color","red");

<strong>15、位置计算相关方法 </strong>

jQuery.scrollLeft()，设置滚动条偏移，这个方法对可见元素或不可见元素都生效

jQuery.scrollTop()，设置滚动条偏移，这个方法对可见元素或不可见元素都生效

jQuery.offset()，计算偏移量，返回值有2个属性，分别是top和left

jQuery.position()，计算位置，返回值也有2个属性，top和left

<strong>16、宽度和高度计算相关方法 </strong>

这组方法需要结合CSS的盒子模型来理解，margin始终不参与计算

jQuery.height()，这个方法计算的是content

jQuery.innerHeight()，这个方法计算的是content+padding

jQuery.outerHeight()，这个方法计算的是content+padding+border

jQuery.width();

jQuery.innerWidth();

jQuery.outerWidth();

<strong>17、页面加载完成事件 </strong>

$(document).ready(function(){})，可以简写为$(function(){})

<strong>18、事件绑定方法 </strong>

jQuery.bind(type,data,fn)

bind()方法可以接受3个参数，第1个是事件类型，类型是string，可能的值有blur, focus, load, resize, scroll, unload, beforeunload, click, dblclick, mousedown, mouseup, mousemove, mouseover, mouseout, mouseenter, mouseleave, change, select,

submit, keydown, keypress, keyup, error

第3个参数是当事件发生时，要执行的函数，函数原型是
<ol>
	<li>function callback(eventObject) {</li>
	<li>this; // dom element</li>
	<li>}</li>
</ol>
在这个方法里return false会阻止事件冒泡并中止默认行为，如果在这个方法里调用eventObject.preventDefault()则会中止默认行为，如果在这个方法里调用eventObject.stopPropagation()则只会阻止事件冒泡

第2个参数是可选的，会赋值给e.data，比如
<ol>
	<li>function handler(event) {</li>
	<li>alert(event.data.foo);</li>
	<li>}</li>
	<li>$("p").bind("click", {foo: "bar"}, handler)</li>
</ol>
jQuery.one(type,data,fn)，这个方法类似于bind()方法，区别在于只会绑定一次

jQuery.unbind(type,fn)，解除绑定

jQuery.trigger(event,data)，触发事件，要注意这个方法，同样会引起浏览器的默认行为，比如submit

另外，这个方法如果和bind()方法里定义的handler配合使用，可以更加灵活地传递参数，比如
<ol>
	<li>$("#test").bind("click", {name : "kyfxbl"}, function(e, foo) {</li>
	<li>alert(e.data.name);</li>
	<li>alert("foo: " + foo);</li>
	<li>});</li>
</ol>
以上代码，如果直接点击#test，则foo的值是undefined

但是如果通过$("#test").trigger("click",["foo"])来触发，则参数foo会被赋值为"foo"

jQuery.triggerHandler(event,data)，这个方法和trigger()方法十分相像，主要有2点不同，1是这个方法不会触发浏览器的默认行为，2是它只会在jQuery对象集合的第一个元素上触发

jQuery.live(type,fn)，这个方法十分类似jQuery.bind()方法，区别在于这个方法对后来才添加进来的元素同样有效

jQuery.die(type,fn)，这个是jQuery.live()的相反方法

<strong>19、事件快捷方法 </strong>

jQuery.hover(over,out)，这个方法是mouseenter和mouseleave的便捷方法，2个参数的函数原型是：
<ol>
	<li>function callback(eventObject) {</li>
	<li>this; // dom element</li>
	<li>}</li>
</ol>
jQuery.toggle(fn,fn2,fn3,...)，这个方法是多次点击的便捷方法，参数的函数原型是：
<ol>
	<li>function callback(eventObject) {</li>
	<li>this; // dom element</li>
	<li>}</li>
</ol>
jQuery提供了两类便捷方法：

第一类是类似于click()这种，相当于简化的jQuery.trigger()方法，比如$("p").click()相当 于$("p").trigger("click")，不过该方法，无法像完整的jQuery.trigger("click", data)方法一样，传递一个附带的参数

第二类是类似于click(function)这种，相当于简化的jQuery.bind()方法，比 如$("p").click(function)相当于$("p").bind("click",function)，不过该方法，无法像完整的 jQuery.bind("click", data, func)一样，传递一个额外的参数

<strong>20、切换元素显示与否的方法 </strong>

jQuery.toggle()，原本显示的元素会不显示，原本不显示的会显示出来。这些元素可以是通过show()和hide()切换的，也可以是通过display:none来设置的

jQuery.show()，显示元素

jQuery.hide()，隐藏元素

jQuery.show(speed, callback)，类似于上面的jQuery.show()，不过可以设置速度以及回调函数

speed可以是"slow"、"normal"、"fast"，也可以是毫秒数

callback函数的原型是：
function callback() {
this; // dom element
}
jQuery.hide(speed, callback)
jQuery.toggle(speed, callback)

<strong>21、页面一些特效方法 </strong>

jQuery.slideDown(speed, callback)，让一个元素下滑，从无到有

jQuery.slideUp(speed, callback)，让一个元素上升，从有到无

jQuery.slideToggle(speed, callback)，切换一个下滑和上升

jQuery.fadeIn(speed, callback)，淡入效果

jQuery.fadeOut(speed, callback)，淡出效果

jQuery.fadeTo(speed, opacity, callback)，变淡效果

<strong>22、ajax相关方法 </strong>

$.ajax(options)，这个是底层方法，上层的$.get()和$.post()都是基于此方法的封装

options:

async：是否异步，默认为true

url：目标地址

type：请求类型，可以是"POST"或者"GET"

data：请求参数，比如"name=kyfxbl&amp;location=shenzhen"

complete(function)：请求结束后的回调函数，函数原型是
<ol>
	<li>function (XMLHttpRequest, textStatus) {</li>
	<li>this; // the options for this ajax request</li>
	<li>}</li>
</ol>
success(function)：请求成功后的回调函数，函数原型是
<ol>
	<li>function (data, textStatus) {</li>
	<li>// data could be xmlDoc, jsonObj, html, text, etc...</li>
	<li>this; // the options for this ajax request</li>
	<li>}</li>
</ol>
例子：
<ol>
	<li>$.ajax({</li>
	<li>url : "user/ajax",</li>
	<li>type : "GET",</li>
	<li>data : "name=kyfxbl&amp;location=shenzhen",</li>
	<li>success : function(data, textStatus) {</li>
	<li>alert(data);</li>
	<li>alert(this.success);</li>
	<li>}</li>
	<li>});</li>
</ol>
$.get(url, data, callback, type)，$.ajax()的简易方法，用于发送GET请求

url：请求地址

data：发送到服务端的请求参数

callback：请求成功后的回调函数，函数原型是：
<ol>
	<li>function (data, textStatus) {</li>
	<li>// data could be xmlDoc, jsonObj, html, text, etc...</li>
	<li>this; // the options for this ajax request</li>
	<li>}</li>
</ol>
$.post(url, data, callback, type)，$.ajax()的简易方法，跟$.get()差不多，用于发送POST请求

<strong>23、浏览器及特性检测 </strong>

$.support，可以检测当前浏览器是否支持下列属性，返回boolean。包括boxModel、cssFloat、opacity、tbody等
$.browser，检测当前浏览器类型，返回一个map，其中可能的值有safari、opera、msie、mozilla

<strong>24、数据缓存方法 </strong>

该类方法是jQuery.data()方法和jQuery.removeData()的另一种形式，增加的elem参数是DOM Element

$.data(elem, name)，取出elem上name的值

$.data(elem, name, value)，设置elem上name的值为value

$.removeData(elem, name)，删除elem上的name

$.removeData(elem)，删除elem上的所有缓存数据

<strong>25、工具方法 </strong>

$.isArray(obj)，检测一个对象是否是数组

$.isFunction(obj)，检测一个对象是否是函数

$.trim(str)，去除string的空格

$.inArray(value, array)，返回value在array中的下标，如果没有找到则返回-1，比如$.inArray(123, ["john",1,123,"f"])将会返回2

$.unique(array)，去除array中的重复元素，该方法只对DOM Element有效，对string和number无效