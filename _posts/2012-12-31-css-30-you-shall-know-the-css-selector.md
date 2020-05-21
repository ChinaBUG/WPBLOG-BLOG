---
ID: 2259
post_title: CSS-你须知道的30个CSS选择器
author: ChinaBUG
post_excerpt: >
  你也许已经掌握了id、class、后台选择器这些基本的css选择器。但这远远不是css的全部。下面向大家系统的解析css中30个最常用的选择器，包括我们最头痛的浏览器兼容性问题。掌握了它们，才能真正领略css的巨大灵活性。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/css-30-you-shall-know-the-css-selector.html
published: true
post_date: 2012-12-31 13:44:51
---
<section>你也许已经掌握了id、class、后台选择器这些基本的css选择器。但这远远不是css的全部。下面向大家系统的解析css中30个最常用的选择器，包括我们最头痛的浏览器兼容性问题。掌握了它们，才能真正领略css的巨大灵活性。
<h2>1. *</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>* {margin: 0; padding: 0;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
星状选择符会在页面上的每一个元素上起作用。web设计者经常用它将页面中所有元素的margin和padding设置为0。

*选择符也可以在子选择器中使用。
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>	#container * { border: 1px solid black;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
上面的代码中会应用于id为container元素的所有子元素中。

除非必要，我不建议在页面中过的的使用星状选择符，因为他的作用域太大，相当耗浏览器资源。

兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>2. #X</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>#container {width: 960px;margin: auto; }</pre>
</td>
</tr>
</tbody>
</table>
</div>
井号作用域有相应id的元素。id是我们最常用的css选择器之一。id选择器的优势是精准，高优先级（优先级基数为100，远高于class的10），作为javascript脚本钩子的不二选择，同样缺点也很明显优先级过高，重用性差，所以在使用id选择器前，我们最好问下自己，真的到了非用id选择器的地步？

兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>3. .X</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>.error {color: red;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这是一个class(类)选择器。class选择器与id选择器的不同是class选择器能作用于期望样式化的一组元素。

兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>4. X Y</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>li a { text-decoration: none;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这也是我们最常用的一种选择器——后代选择器。用于选取X元素下子元素Y，要留意的点是，这种方式的选择器将选取其下所有匹配的子元素，无视层级，所以有的情况是不宜使用的，比如上述的代码去掉li下的所有a的下划线，但li里面还有个ul，我不希望ul下的li的a去掉下划线。使用此后代选择器的时候要考虑是否希望某样式对所有子孙元素都起作用。这种后代选择器还有个作用，就是创建类似命名空间的作用。比如上述代码样式的作用域明显为li。

兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>5. X</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2</pre>
</td>
<td>
<pre>a { color: red; }  
ul { margin-left: 0; }</pre>
</td>
</tr>
</tbody>
</table>
</div>
标签选择器。使用标签选择器作用于作用域范围内的所有对应标签。优先级仅仅比*高。

兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>6. X:visited和X:link</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2</pre>
</td>
<td>
<pre>a:link { color: red; }   
a:visted { color: purple; }</pre>
</td>
</tr>
</tbody>
</table>
</div>
使用:link伪类作用于未点击过的链接标签。:hover伪类作用于点击过的链接。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>7. X+Y</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>ul + p {color: red;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
相邻选择器，上述代码中就会匹配在ul后面的第一个p，将段落内的文字颜色设置为红色。(只匹配第一个元素)

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>8. X&gt;Y</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>div#container &gt; ul { border: 1px solid black;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6
7
8
9
10
11
12</pre>
</td>
<td>
<pre>&lt;div id=”container”&gt;
	&lt;ul&gt;
		&lt;li&gt; List Item
			&lt;ul&gt;
				&lt;li&gt; Child &lt;/li&gt;
			&lt;/ul&gt;
		&lt;/li&gt;
		&lt;li&gt; List Item &lt;/li&gt;
		&lt;li&gt; List Item &lt;/li&gt;
		&lt;li&gt; List Item &lt;/li&gt;
	&lt;/ul&gt; 
  &lt;/div&gt;</pre>
</td>
</tr>
</tbody>
</table>
</div>
子选择器。与后代选择器X Y不同的是，子选择器只对X下的直接子级Y起作用。在上面的css和html例子中，div#container&gt;ul仅对container中最近一级的ul起作用。从理论上来讲X &gt; Y是值得提倡选择器，可惜IE6不支持。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>9. X ~ Y</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>ul ~ p { color: red;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
相邻选择器，与前面提到的X+Y不同的是，X~Y匹配与X相同级别的所有Y元素，而X+Y只匹配第一个。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>10. X[title]</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>a[title] { color: green;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
属性选择器。比如上述代码匹配的是带有title属性的链接元素。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>11. X[title="foo"]</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>a[href="http://blog.moocss.com"] { color: #1f6053; }</pre>
</td>
</tr>
</tbody>
</table>
</div>
属性选择器。 上面的代码匹配所有拥有href属性，且href为http://blog.moocss.com的所有链接。

这个功能很好，但是多少又有些局限。如果我们希望匹配href包含css9.net的所有链接该怎么做呢？看下一个选择器。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>12. X[href*="moocss"]</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>a[href*="moocss"] {color: #1f6053;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
属性选择器。正如我们想要的，上面代码匹配的是href中包含”http://blog.moocss.com“的所有链接。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>13. X[href^="http"]</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>a[href^="http"] {background: url(path/to/external/icon.png) no-repeat; padding-left: 10px;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
属性选择器。上面代码匹配的是href中所有以http开头的链接。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>14. X[href$=".jpg"]</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>a[href^="http"] { background: url(path/to/external/icon.png) no-repeat;padding-left: 10px;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
属性选择器。在属性选择器中使用$，用于匹配结尾为特定字符串的元素。在上面代码中匹配的是所有链接到扩展名为.jpg图片的链接。（注意，这里仅仅是.jpg图片，如果要作用于所有图片链接该怎么做呢，看下一个选择器。）

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>15. X[data-*="foo"]</h2>
在上一个选择器中提到如何匹配所有图片链接。如果使用X[href$=".jpg"]实现，需要这样做：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6</pre>
</td>
<td>
<pre>a[href$=".jpg"],
a[href$=".jpeg"],
a[href$=".png"],
a[href$=".gif"] {
	color: red;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
看上去比较麻烦。另一个解决办法是为所有的图片链接加一个特定的属性，例如‘data-file’

html代码

<a href="”path/to/image.jpg”" data-filetype="”image”">图片链接 </a>

css代码如下：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>a[data-filetype="image"] {
	color: red;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这样所有链接到图片的链接字体颜色为红色。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>16. X[foo~="bar"]</h2>
属性选择器。属性选择器中的波浪线符号可以让我们匹配属性值中用空格分隔的多个值中的一个。看下面例子：

html代码
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>&lt;a href=”path/to/image.jpg” data-info=”external image”&gt; Click Me, Fool &lt;/a&gt;</pre>
</td>
</tr>
</tbody>
</table>
</div>
css代码
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6</pre>
</td>
<td>
<pre>a[data-info~="external"] {
	color: red;
}
a[data-info~="image"] {
	border: 1px solid black;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
在上面例子中，匹配data-info属性中包含“external”链接的字体颜色为红色。匹配data-info属性中包含“image”的链接设置黑色边框。

兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>17. X:checked</h2>
checked伪类用来匹配处于选定状态的界面元素，如radio、checkbox。
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>input[type=radio]:checked {
	border: 1px solid black;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
上面代码中匹配的是所有处于选定状态的单选radio，设置1px的黑色边框。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>18. X:after和X:before</h2>
这两个伪类与content结合用于在元素的前面或者后面追加内容，看一个简单的例子：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1</pre>
</td>
<td>
<pre>h1:after {content:url(/i/logo.gif)}</pre>
</td>
</tr>
</tbody>
</table>
</div>
上面的代码实现了在h1标题的后面显示一张图片。

我们也经常用它来实现清除浮动，写法如下：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6
7
8
9
10
11</pre>
</td>
<td>
<pre>.clearfix:after {
	content: “”;
	display: block;
	clear: both;
	visibility: hidden;
	font-size: 0;
	height: 0;
}      
.clearfix {
	*zoom:1
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>19. X:hover</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>div:hover {
	background: #e3e3e3;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
:hover伪类设定当鼠标划过时元素的样式。上面代码中设定了div划过时的背景色。

需要注意的是，在ie 6中，:hover只能用于链接元素。

这里分享一个经验，在设定链接划过时出现下滑线时，使用border-bottom会比text-decoration显得更漂亮些。代码如下：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>a:hover {
    border-bottom: 1px solid black;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera
<h2>20. X:not(selector)</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>div:not(#container) {
	color: blue;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
否定伪类选择器用来在匹配元素时排除某些元素。在上面的例子中，设定除了id为container的div元素字体颜色为blue。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>21. X::pseudoElement</h2>
::伪对象用于给元素片段添加样式。比如一个段落的第一个字母或者第一行。需要注意的是，这个::伪对象只能用于块状元素。

下面的代码设定了段落中第一个字母的样式：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6
7</pre>
</td>
<td>
<pre>p::first-letter {
	float: left;
	font-size: 2em;
	font-weight: bold;
	font-family: cursive;
	padding-right: 2px;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
下面的代码中设定了段落中第一行的样式：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4</pre>
</td>
<td>
<pre>p::first-line {
	font-weight: bold;
	font-size: 1.2em;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
兼容浏览器：IE6+、Firefox、Chrome、Safari、Opera

（IE6竟然支持，有些意外啊。）
<h2>22. X:nth-child(n)</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>li:nth-child(3) {
	color: red;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这个伪类用于设定一个序列元素（比如li、tr）中的第n个元素（从1开始算起）的样式。在上面例子中，设定第三个列表元素li的字体颜色为红色。

看一个更灵活的用法，在下面例子中设定第偶数个元素的样式，可以用它来实现隔行换色：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>tr:nth-child(2n) {
	background-color: gray;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
兼容浏览器：IE9+、Firefox、Chrome、Safari
<h2>23. X:nth-last-child(n)</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>li:nth-last-child(2) {
	color: red;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
与X:nth-child(n)功能类似，不同的是它从一个序列的最后一个元素开始算起。上面例子中设定倒数第二个列表元素的字体颜色。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>24. X:nth-of-type(n)</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul:nth-of-type(3) {
	border: 1px solid black;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
与X:nth-child(n)功能类似，不同的是它匹配的不是某个序列元素，而是元素类型。例如上面的代码设置页面中出现的第三个无序列表ul的边框。

兼容浏览器：IE9+、Firefox、Chrome、Safari
<h2>25. X:nth-last-of-type(n)</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul:nth-last-of-type(3) {
	border: 1px solid black;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
与X:nth-of-type(n)功能类似，不同的是它从元素最后一次出现开始算起。上面例子中设定倒数第三个无序列表的边框。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>26. X:first-child</h2>
:first-child伪类用于匹配一个序列的第一个元素。我们经常用它来实现一个序列的第一个元素或最后一个元素的上（下）边框，如：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul li:first-child {  
   border-top: none;  
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
兼容浏览器：IE7+、Firefox、Chrome、Safari、Opera
<h2>27. X:last-child</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul &gt; li:last-child {
	border-bottom:none;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
与:first-child类似，它匹配的是序列中的最后一个元素。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>28. X:only-child</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>div p:only-child {
	color: red;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这个伪类用的比较少。在上面例子中匹配的是div下有且仅有一个的p，也就是说，如果div内有多个p，将不匹配。
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5</pre>
</td>
<td>
<pre>&lt;div&gt;&lt;p&gt; My paragraph here. &lt;/p&gt;&lt;/div&gt;
&lt;div&gt;
	&lt;p&gt; Two paragraphs total. &lt;/p&gt;
	&lt;p&gt; Two paragraphs total. &lt;/p&gt;
&lt;/div&gt;</pre>
</td>
</tr>
</tbody>
</table>
</div>
在上面代码中第一个div中的段落p将会被匹配，而第二个div中的p则不会。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>29. X:only-of-type</h2>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>li:only-of-type {
	font-weight: bold;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
这个伪类匹配的是，在它上级容器下只有它一个子元素，它没有邻居元素。例如上面代码匹配仅有一个列表项的列表元素。

兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera
<h2>30. X:first-of-type</h2>
:first-of-type伪类与:nth-of-type(1)效果相同，匹配出现的第一个元素。我们来看个例子：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3
4
5
6
7
8
9
10
11</pre>
</td>
<td>
<pre>&lt;div&gt;
	&lt;p&gt; My paragraph here. &lt;/p&gt;
	&lt;ul&gt;
		&lt;li&gt; List Item 1 &lt;/li&gt;
		&lt;li&gt; List Item 2 &lt;/li&gt;
	&lt;/ul&gt;
	&lt;ul&gt;
		&lt;li&gt; List Item 3 &lt;/li&gt;
		&lt;li&gt; List Item 4 &lt;/li&gt;
	&lt;/ul&gt;
&lt;/div&gt;</pre>
</td>
</tr>
</tbody>
</table>
</div>
在上面的html代码中，如果我们希望仅匹配List Item 2列表项该如何做呢：

方案一：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul:first-of-type &gt; li:nth-child(2) {
	font-weight: bold;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
方案二：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>p + ul li:last-child {
	font-weight: bold;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
方案三：
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2
3</pre>
</td>
<td>
<pre>ul:first-of-type li:nth-last-child(1) {
	font-weight: bold;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
兼容浏览器：IE9+、Firefox、Chrome、Safari、Opera。

总结：

如果你正在使用老版本的浏览器，如IE 6，在使用上面css选择器时一定要注意它是否兼容。不过，这不应成为阻止我们学习使用它的理由。在设计时，你可以参考浏览器兼容性列表，也可以通过脚本手段让老版本的浏览器也支持它们。
<blockquote>原文来自：<a href="http://net.tutsplus.com/tutorials/html-css-techniques/the-30-css-selectors-you-must-memorize/">The 30 CSS Selectors you Must Memorize</a></blockquote>
</section>