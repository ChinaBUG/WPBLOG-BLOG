---
ID: 2749
post_title: >
  CSS-常用的浮出层箭头特效框如何用纯CSS样式写？
author: ChinaBUG
post_excerpt: |
  浮出层是web页面中经常用到的功能，带有小小尖角的浮出层则更为生动，今天从大前端看到这篇文章，受益匪浅，转过来给有需要的人一起学习。
  下面这张图就是浮出层的效果，引用大前端。
  上面就是最终的效果噢，将下面的代码另存为一个.html超本文的文件之后你就能看到效果了哦~~
  需要注意的是下面代码之中标红色字体的style="top: 50%;"与style="left: 50%;"，这两个加上去就会让相应的箭号都垂直居中（top属性）或者水平居中（left属性）的。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/arrow-effects-commonly-surfaced-layer-with-pure-css-style-box-how-to-write.html
published: true
post_date: 2013-11-08 14:02:29
---
<style><!--
/* poptip */ .poptip{position: absolute;top: 20px;left:20px;padding: 6px 10px 5px;*padding: 7px 10px 4px;line-height: 16px;color: #DB7C22;font-size: 12px;background-color: #FFFCEF;border: solid 1px #FFBB76;border-radius: 2px;box-shadow: 0 0 3px #ddd;} .poptip-arrow{position: absolute;overflow: hidden;font-style: normal;font-family: simsun;font-size: 12px;text-shadow:0 0 2px #ccc;} .poptip-arrow em,.poptip-arrow i{position: absolute;left:0;top:0;font-style: normal;} .poptip-arrow em{color: #FFBB76;} .poptip-arrow i{color: #FFFCEF;text-shadow:none;} .poptip-arrow-top,.poptip-arrow-bottom{height: 6px;width: 12px;left:12px;margin-left:-6px;} .poptip-arrow-left,.poptip-arrow-right{height: 12px;width: 6px;top: 12px;margin-top:-6px;} .poptip-arrow-top{top: -6px;} .poptip-arrow-top em{top: -1px;} .poptip-arrow-top i{top: 0px;} .poptip-arrow-bottom{bottom: -6px;} .poptip-arrow-bottom em{top: -8px;} .poptip-arrow-bottom i{top: -9px;} .poptip-arrow-left{left:-6px;} .poptip-arrow-left em{left:1px;} .poptip-arrow-left i{left:2px;} .poptip-arrow-right{right:-6px;} .poptip-arrow-right em{left:-6px;} .poptip-arrow-right i{left:-7px;}
--></style>
<div class="poptip" style="margin-bottom: 50px; position: relative; width: 200px;"><span class="poptip-arrow poptip-arrow-top" style="left: 50%;"><em>◆</em><i>◆</i></span><span class="poptip-arrow poptip-arrow-right" style="top: 50%;"><em>◆</em><i>◆</i></span><span class="poptip-arrow poptip-arrow-bottom" style="left: 50%;"><em>◆</em><i>◆</i></span><span class="poptip-arrow poptip-arrow-left" style="top: 50%;"><em>◆</em><i>◆</i></span>《<a href="http://blog.ipodmp.com/?s=二 次 开 发DIY日记">二 次 开 发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点......</div>
<div>

<strong>浮出层</strong>是web页面中经常用到的功能，带有小小尖角的浮出层则更为生动，今天从大前端看到这篇文章，受益匪浅，转过来给有需要的人一起学习。

下面这张图就是浮出层的效果，引用大前端。

上面就是最终的效果噢，将下面的代码另存为一个.html超本文的文件之后你就能看到效果了哦~~

需要注意的是下面代码之中标红色字体的style="top: 50%;"与style="left: 50%;"，这两个加上去就会让相应的箭号都垂直居中（top属性）或者水平居中（left属性）的。
<pre>&lt;style&gt;
/* poptip */
.poptip{position: absolute;top: 20px;left:20px;padding: 6px 10px 5px;*padding: 7px 10px 4px;line-height: 16px;color: #DB7C22;font-size: 12px;background-color: #FFFCEF;border: solid 1px
#FFBB76;border-radius: 2px;box-shadow: 0 0 3px #ddd;}
.poptip-arrow{position: absolute;overflow: hidden;font-style: normal;font-family: simsun;font-size: 12px;text-shadow:0 0 2px #ccc;}
.poptip-arrow em,.poptip-arrow i{position: absolute;left:0;top:0;font-style: normal;}
.poptip-arrow em{color: #FFBB76;}
.poptip-arrow i{color: #FFFCEF;text-shadow:none;}
.poptip-arrow-top,.poptip-arrow-bottom{height: 6px;width: 12px;left:12px;margin-left:-6px;}
.poptip-arrow-left,.poptip-arrow-right{height: 12px;width: 6px;top: 12px;margin-top:-6px;}
.poptip-arrow-top{top: -6px;}
.poptip-arrow-top em{top: -1px;}
.poptip-arrow-top i{top: 0px;}
.poptip-arrow-bottom{bottom: -6px;}
.poptip-arrow-bottom em{top: -8px;}
.poptip-arrow-bottom i{top: -9px;}
.poptip-arrow-left{left:-6px;}
.poptip-arrow-left em{left:1px;}
.poptip-arrow-left i{left:2px;}
.poptip-arrow-right{right:-6px;}
.poptip-arrow-right em{left:-6px;}
.poptip-arrow-right i{left:-7px;}
&lt;/style&gt;
&lt;div class="poptip"&gt;
&lt;span class="poptip-arrow poptip-arrow-top" <span style="color: #ff0000;">style="left: 50%;"</span>&gt;&lt;em&gt;◆&lt;/em&gt;&lt;i&gt;◆&lt;/i&gt;&lt;/span&gt;
&lt;span class="poptip-arrow poptip-arrow-right" <span style="color: #ff0000;">style="top: 50%;"</span>&gt;&lt;em&gt;◆&lt;/em&gt;&lt;i&gt;◆&lt;/i&gt;&lt;/span&gt;
&lt;span class="poptip-arrow poptip-arrow-bottom" <span style="color: #ff0000;">style="left: 50%;"</span>&gt;&lt;em&gt;◆&lt;/em&gt;&lt;i&gt;◆&lt;/i&gt;&lt;/span&gt;
&lt;span class="poptip-arrow poptip-arrow-left" <span style="color: #ff0000;">style="top: 50%;"</span>&gt;&lt;em&gt;◆&lt;/em&gt;&lt;i&gt;◆&lt;/i&gt;&lt;/span&gt;
《&lt;a href="http://blog.ipodmp.com/?s=二 次 开 发DIY日记"&gt;二 次 开 发DIY日记&lt;/a&gt;》系列由&lt;a href="http://blog.ipodmp.com/about-chinabug/"&gt;ChinaBUG&lt;/a&gt;企划，根据开发过程中客户需求做的修改而延伸出来的开发要点......
&lt;/div&gt;</pre>
</div>