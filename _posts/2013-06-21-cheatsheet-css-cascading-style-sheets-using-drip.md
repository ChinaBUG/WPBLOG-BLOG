---
ID: 2496
post_title: >
  CheatSheet-CSS层叠样式表使用点滴
author: ChinaBUG
post_excerpt: |
  1、IE6不支持display:inline-block?
  2、怎么样来区分IE6，IE7，firefox或者是ie6 hack？
  3、开启 IE8 的兼容模式（用于解决ShopEX4.8.5的IE8,IE9,IE10的兼容问题）
  4、IE6兼容position:fixed
  5、自动换行与不换行
  6、360浏览器为什么输入框外面有蓝色框框呢？怎么去除这个蓝色框呢？
  7、IE系列的CSS条件判断注释（参考Conditional comments）
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/cheatsheet-css-cascading-style-sheets-using-drip.html
published: true
post_date: 2013-06-21 17:44:19
---
CSS布局口诀

一、IE边框若显若无，须注意，定是高度设置已忘记；

二、浮动产生有缘故，若要父层包含住，紧跟浮动要清除，容器自然显其中；

三、三像素文本慢移不必慌，高度设置帮你忙；

四、兼容各个浏览须注意，默认设置行高可能是杀手；

五、独立清除浮动须铭记，行高设无，高设零，设计效果兼浏览；

六、学布局须思路，路随布局原理自然直，轻松驾驭html，流水布局少hack，代码清爽，兼容好，友好引擎喜欢迎。

七、所有标签皆有源，只是默认各不同，span是无极，无极生两仪—内联和块级，img较特殊，但也遵法理，其他只是改造各不同，一个*号全归原，层叠样式理须多练习，万物皆规律。

八、图片链接排版须小心，图片链接文字链接若对齐，padding和vertical-align:middle要设定，虽差微细倒无妨。

九、IE浮动双边距，请用display：inline拘。

十、列表横向排版，列表代码须紧靠，空隙自消须铭记。

~~~~~~~~

1、IE6不支持display:inline-block?

使用float:left; display:inline;代替display:inline-block;能很好的解决这种不兼容问题。

2、怎么样来区分IE6，IE7，firefox或者是ie6 hack？

background:orange;*background:green;_background:blue;

注：不管是什么方法，书写的顺序都是firefox的写在前面，IE7的写在中间，IE6的写在最后面。

3、开启 IE8 的兼容模式（用于解决ShopEX4.8.5的IE8,IE9,IE10的兼容问题）

&lt;!--“Quirks 模式”--&gt;
&lt;meta http-equiv="X-UA-Compatible" content="IE=5" /&gt;

&lt;!--IE7 标准模式--&gt;
&lt;meta http-equiv="X-UA-Compatible" content="IE=7" /&gt;

&lt;!--IE8 标准模式--&gt;
&lt;meta http-equiv="X-UA-Compatible" content="IE=8" /&gt;

4、IE6兼容position:fixed（摘自清风创科）

.fixed-top{/* 头部固定 */
position:fixed;bottom:auto;top:0px;
}
.fixed-bottom{/* 底部固定 */
position:fixed;bottom:0px;top:auto;
}
.fixed-left{/* 左侧固定 */
position:fixed;right:auto;left:0px;
}
.fixed-right{/* 右侧固定 */
position:fixed;right:0px;left:auto;
}
再来看看IE6的解决方案
* html,* html body{/* 修正IE6振动bug */
background-image:url(about:blank);background-attachment:fixed;
}
* html .fixed-top{/* IE6 头部固定 */
position:absolute;bottom:auto;top:expression(eval(document.documentElement.scrollTop));
}
* html .fixed-right{/* IE6 右侧固定 */
position:absolute;right:auto;left:expression(eval(document.documentElement.scrollLeft+document.documentElement.clientWidth-this.offsetWidth)-
(parseInt(this.currentStyle.marginLeft,10)||0)-(parseInt(this.currentStyle.marginRight,10)||0));
}
* html .fixed-bottom{/* IE6 底部固定 */
position:absolute;bottom:auto;top:expression(eval(document.documentElement.scrollTop+document.documentElement.clientHeight-this.offsetHeight-
(parseInt(this.currentStyle.marginTop,10)||0)-(parseInt(this.currentStyle.marginBottom,10)||0)));
}
* html .fixed-left{/* IE6 左侧固定 */
position:absolute;right:auto;left:expression(eval(document.documentElement.scrollLeft));
}

5、自动换行与不换行

强制不换行：white-space:nowrap;
自动换行：word-wrap: break-word;word-break: normal;
强制英文单词断行：word-break:break-all;

6、360浏览器为什么输入框外面有蓝色框框呢？怎么去除这个蓝色框呢？

这个是外框的问题噢，所以设置一下outline:none;就可以了噢。

7、IE系列的CSS条件判断注释（参考<a href="http://www.quirksmode.org/css/condcom.html">Conditional comments</a>）
&lt;!--[if IE]&gt;这是IE浏览器&lt;![endif]--&gt;
&lt;!--[if IE 6]&gt;这是IE 6 浏览器&lt;![endif]--&gt;
&lt;!--[if IE 7]&gt;这是IE 7 浏览器&lt;![endif]--&gt;
&lt;!--[if IE 8]&gt;这是IE 8 浏览器&lt;![endif]--&gt;
&lt;!--[if IE 9]&gt;这是IE 9 浏览器&lt;![endif]--&gt;
&lt;!--[if gte IE 8]&gt;这是高于 IE 8 的浏览器&lt;![endif]--&gt;
&lt;!--[if lt IE 9]&gt;这是低于 IE 8 的浏览器&lt;![endif]--&gt;
&lt;!--[if lte IE 7]&gt;这是小于等于于 IE 8 的浏览器&lt;![endif]--&gt;
&lt;!--[if !IE]&gt; --&gt;这不是IE的浏览器&lt;!-- &lt;![endif]--&gt;

8、