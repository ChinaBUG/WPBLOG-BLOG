---
ID: 2262
post_title: WEB-一些有用的Web前端开发技巧
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/web-some-useful-web-front-end-development-skills.html
published: true
post_date: 2012-12-31 14:10:39
---
<h2>CSS代码调试片段</h2>
用下面的CSS代码片段，可以把XHTML(or HTML)里的每一元素的层次结构勾勒出来，通过层次结构中每一级的颜色边框的变化，可以方便的看出元素“深度”的变化。
给浮动和定位的调试带来了方便。
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
8</pre>
</td>
<td>
<pre>  * { outline: 2px dotted red }
  * * { outline: 2px dotted green }
  * * * { outline: 2px dotted orange }
  * * * * { outline: 2px dotted blue }
  * * * * * { outline: 1px solid red }
  * * * * * * { outline: 1px solid green }
  * * * * * * * { outline: 1px solid orange }
  * * * * * * * * { outline: 1px solid blue }</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>CSS代码块注释</h2>
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
<pre>/***************************

-----------HTML5------------

***************************/

/*模块一
--------------------------*/</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>更好，更富有语义的清除浮动</h2>
在960gs网格布局中我们用到的清除浮动是用clearfix命名的，我觉的group的命名更富有语义性。
<h3>以前的960gs网格清除浮动类</h3>
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
9</pre>
</td>
<td>
<pre>.group:after {
	visibility: hidden;
	display: block;
	font-size: 0;
	content: "\0020";
	clear: both;
	height: 0;
}
.group{*zoom:1;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h3>现在的960gs网格清除浮动类</h3>
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
12
13
14
15
16
17
18
19
20
21
22</pre>
</td>
<td>
<pre>.group:before,
.group:after {
	content: '\0020';
	display: block;
	overflow: hidden;
	visibility: hidden;
	width: 0;
	height: 0;
}

.group:after {
	clear: both;
}

/*
	The following zoom:1 rule is specifically for IE6 + IE7.
	Move to separate stylesheet if invalid CSS is a problem.
*/

.group {
	zoom: 1;
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h3>OOCSS 网格清除浮动类</h3>
<div>
<table>
<tbody>
<tr>
<td>
<pre>1
2</pre>
</td>
<td>
<pre>.group:after{clear:both;display:block;visibility:hidden;overflow:hidden;height:0 !important;line-height:0;font-size:xx-large;content:" x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x x ";}
.group{*zoom:1;}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>CSS Opcity</h2>
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
<pre>opacity: .75; /* Standard: FF gt 1.5, Opera, Safari */
filter: alpha(opacity=75); /* IE lt 8 */
-ms-filter: "alpha(opacity=75)"; /* IE 8 */
-khtml-opacity: .75; /* Safari 1.x */
-moz-opacity: .75; /* FF lt 1.5, Netscape */</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>容器内容超出(溢出)自动换行</h2>
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
9</pre>
</td>
<td>
<pre> word-break: break-word; /* 文本行的任意字内断开 */
 word-wrap: break-word; /* IE */ 
 white-space: -moz-pre-wrap; /* Mozilla */ 
 white-space: -hp-pre-wrap; /* HP printers */ 
 white-space: -o-pre-wrap; /* Opera 7 */ 
 white-space: -pre-wrap; /* Opera 4-6 */ 
 white-space: pre; /* CSS2 */
 white-space: pre-wrap; /* CSS 2.1 */ 
 white-space: pre-line; /* CSS 3 (and 2.1 as well, actually) */</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>Wrapping Long URLs and Text Content with CSS</h2>
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
10</pre>
</td>
<td>
<pre>pre {
	white-space: pre;           /* CSS 2.0 */
	white-space: pre-wrap;      /* CSS 2.1 */
	white-space: pre-line;      /* CSS 3.0 */
	white-space: -pre-wrap;     /* Opera 4-6 */
	white-space: -o-pre-wrap;   /* Opera 7 */
	white-space: -moz-pre-wrap; /* Mozilla */
	white-space: -hp-pre-wrap;  /* HP Printers */
	word-wrap: break-word;      /* IE 5+ */
}</pre>
</td>
</tr>
</tbody>
</table>
</div>
演示：<a href="http://perishablepress.com/press/wp-content/online/demos/css-wrap-demo.html">http://perishablepress.com/press/wp-content/online/demos/css-wrap-demo.html</a>
<h2>一行内文本溢出的处理</h2>
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
<pre>.textoverflow a {
 display:block;
 width:120px; /*注意宽度必须有点*/
 margin: 0px 0px 0px 3px;
 white-space: nowrap;
 overflow: hidden; 
 text-overflow: ellipsis;       /* for IE */
 -o-text-overflow: ellipsis;    /* for Opera */
}
.textoverflow:after{ content: "..."; }/* for Firefox */
@media all and (min-width: 0px){ .textoverflow:after{ content:""; }/* for Opera */ }</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>去除链接元素的虚线</h2>
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
<pre>:focus {outline:none;}
/*ie6 and ie7 outline:none bug*/
*html a{star:expression(this.onFocus=this.blur());}
*:first-child+html a{star:expression(this.onFocus=this.blur());}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>浏览器特定的Hacks</h2>
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
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43</pre>
</td>
<td>
<pre>    /* IE 6 */
	* html .yourclass { }

	/* IE 7 */
    *:first-child+html .yourclass{ }  
	*+html .yourclass{ }

	/* IE 7 and modern browsers */
	html&gt;body .yourclass { }

	/* Modern browsers (not IE 7) */
	html&gt;/**/body .yourclass { }

	/* Opera 9.27 and below */
	html:first-child .yourclass { }

	/* Safari 2-3*/
	html[xmlns*=""] body:last-child .yourclass { }

	/* Safari 3+, Chrome 1+, Opera 9+, Fx 3.5+ */
	body:nth-of-type(1) .yourclass { }

	/* Safari 3+, Chrome 1+, Opera 9+, Fx 3.5+ */
	body:first-of-type .yourclass {  }

	/* Safari 3+, Chrome 1+ */
	@media screen and (-webkit-min-device-pixel-ratio:0) {
	   .yourclass  {  }
	}

    /* webkit and opera */ 
    @media all and (min-width: 0px){ .yourclass  {  } }  

    /* webkit */ 
    @media screen and (-webkit-min-device-pixel-ratio:0){ .yourclass  {  } } 

    /* opera */  
    @media all and (-webkit-min-device-pixel-ratio:10000), not all and (-webkit-min-device-pixel-ratio:0) { .yourclass  {  } } 

    /* firefox * / 
    @-moz-document url-prefix(){ .yourclass  {  }} /* all firefox */  
    #veinticinco,  x:-moz-any-link, x:default  { .yourclass  {  } }/* Firefox 3.0+ */ 
    html&gt;/**/body .big, x:-moz-any-link, x:default { .yourclass  {  } } /* newest firefox */</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>CSS3 Media Queries</h2>
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
12
13
14
15
16
17
18
19
20
21</pre>
</td>
<td>
<pre>/* 2 column layout */
@media screen and (max-width: 1000px) {

}

/* 1 column layout */
@media screen and (max-width: 760px) {

}
/* small 1 column */
@media screen and (max-width: 600px) {

}
/* iphone landscape */
@media screen and (max-width: 480px) {

}
/* iphone portrait */
@media screen and (max-width: 320px) {

}</pre>
</td>
</tr>
</tbody>
</table>
</div>
<h2>让IE9以下的版本支持 HTML5</h2>
<div>
<div>
<pre>    // html5 shiv
    if (!+[1,]) {
        (function() {
            var tags = [
                'article', 'aside', 'details', 'figcaption',
                'figure', 'footer', 'header', 'hgroup',
                'menu', 'nav', 'section', 'summary',
                'time', 'mark', 'audio', 'video'],
                    i = 0, len = tags.length;
            for (; i &lt; len; i++) document.createElement(tags[i]);
        })();
    }</pre>
</div>
</div>
<h3>网络引用</h3>
<div>
<div>
<pre>&lt;!--[if lt IE 9]&gt;
	&lt;script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"&gt;&lt;/script&gt;
&lt;![endif]--&gt;</pre>
</div>
</div>