---
ID: 2542
post_title: >
  Mootools-ShopEX4.8.5内的图片高度宽度自动调整方法及扩展
author: ChinaBUG
post_excerpt: |
  ShopEX4.8.5本身就有一个方法用于针对图片的高度宽度的自动修正，根据父容器的大小自动调整图片的大小适应父容器。但是，昨天在修改程序的时候才发现，不知道为什么尽然失效，父容器有指定大小但是，执行fixProductImageSize方法之后全部的图片的大小全部变成负数的了，咱是农民，不深入研究为什么，直接修改方法吧，省的这么麻烦。具体代码如下：
  方法fixProductImageSize为ShopEX4.8.5的原方法，位于statics\script_src\jstools.js末端，调用的时候随时随地调用，格式为：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/08/mootools-shopex4-8-5-the-picture-height-and-width-adjustment-method-automatically-extended.html
published: true
post_date: 2013-08-22 10:16:06
---
ShopEX4.8.5本身就有一个方法用于针对图片的高度宽度的自动修正，根据父容器的大小自动调整图片的大小适应父容器。但是，昨天在修改程序的时候才发现，不知道为什么尽然失效，父容器有指定大小但是，执行fixProductImageSize方法之后全部的图片的大小全部变成负数的了，咱是农民，不深入研究为什么，直接修改方法吧，省的这么麻烦。具体代码如下：

方法fixProductImageSize为ShopEX4.8.5的原方法，位于statics\script_src\jstools.js末端，调用的时候随时随地调用，格式为：

html:
&lt;div class='item'&gt;&lt;img src='1.jpg'/&gt;&lt;/div&gt;
&lt;div class='item'&gt;&lt;img src='2.jpg'/&gt;&lt;/div&gt;
&lt;div class='item'&gt;&lt;img src='3.jpg'/&gt;&lt;/div&gt;

Javascript:
try{
fixProductImageSize(<span style="color: #ff0000;">$$('.item img')</span>,<span style="color: #ff0000;">'div'</span>);
}catch(e){}

说明：fixProductImageSize方法的两个参数分别是（图片组对象，父容器标签）。CSS样式自己设置吧。

/*fix Image size*/
var fixProductImageSize=function(images,ptag){
if(!images||!images.length)return;
images.each(function(img){
if(!img.src)return;
new Asset.image(img.src,{
onload:function(){
var imgparent=img.getParent((ptag||'a'));
if(!this||!this.get('width'))return imgparent.adopt(img);
var imgpsize={x:imgparent.getTrueWidth(),y:imgparent.getTrueHeight()};
var nSize=this.zoomImg(imgpsize.x,imgpsize.y,true);
img.set(nSize);
var _style={'margin-top':0};
if(img&amp;&amp;img.get('height'))
if(img.get('height').toInt()&lt;imgpsize.y){
_style=$merge(_style,{'margin-top':(imgpsize.y-img.get('height').toInt())/2});
}
img.setStyles(_style);
return true;
},onerror:function(){}
});
});
};

方法fixImageSize为扩展方法，在需要的地方定义，就可以调用，格式为：

html:
&lt;div class='item'&gt;&lt;img src='1.jpg'/&gt;&lt;/div&gt;
&lt;div class='item'&gt;&lt;img src='2.jpg'/&gt;&lt;/div&gt;
&lt;div class='item'&gt;&lt;img src='3.jpg'/&gt;&lt;/div&gt;

Javascript:
try{
fixImageSize(<span style="color: #ff0000;">$$('.item img')</span>,<span style="color: #ff0000;">'div'</span>);
}catch(e){}

说明：fixImageSize方法的两个参数分别是（图片组对象，父容器标签，固定的大小）。其中“固定的大小”优先权高于父容器，有指定则父容器的大小将被忽略。

/*
fixImageSize:自动以父容器为参照物对图片进行缩放
修改自ShopEX4.8.5的fixProductImageSize方法(位于statics\script_src\jstools.js末端)
主要修改内容：增加参数3可以指定容器大小，用于对付容器高宽无法正确获取的情况
*/
var fixImageSize=function(images,ptag,fixC){
if(!images||!images.length)return;
images.each(function(img){
if(!img.src)return;
new Asset.image(img.src,{
onload:function(){
var imgparent=img.getParent((ptag||'a'));
if(!this||!this.get('width'))return imgparent.adopt(img);
var imgpsize={x:(fixC.w?fixC.w:imgparent.getTrueWidth()),y:(fixC.h?fixC.h:imgparent.getTrueHeight())};
var nSize=this.zoomImg(imgpsize.x,imgpsize.y,true);
img.set(nSize);
var _style={'margin-top':0};
if(img&amp;&amp;img.get('height'))
if(img.get('height').toInt()&lt;imgpsize.y){
_style=$merge(_style,{'margin-top':(imgpsize.y-img.get('height').toInt())/2});
}
img.setStyles(_style);
return true;
},onerror:function(){}
});
});
};