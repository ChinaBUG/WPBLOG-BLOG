---
ID: 4308
post_title: Javascript-常用代码片段
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/12/javascript-common-code-snippets.html
published: true
post_date: 2015-12-07 18:29:46
---
1、获取各种屏幕相关的尺寸：

function getPos(){
var html = [];
html.push( '网页可见区域 : ' + "t" + 'W - ' + document.body.clientWidth + ' ; H - ' + document.body.clientHeight );
html.push( '网页可见区域(包括边线) : ' + "t" + 'W - ' + document.body.offsetWidth + ' ; H - ' + document.body.offsetHeight );
html.push( '网页正文全文 : ' + "t" + 'W - ' + document.body.scrollWidth + ' ; H - ' + document.body.scrollHeight );
html.push( '网页被卷去 : ' + "t" + 'W - ' + document.body.scrollTop + ' ; H - ' + document.body.scrollLeft );
html.push( '网页正文部分 : ' + "t" + 'T - ' + (window.screenTop?window.screenTop:window.screenX) + ' ; L - ' + (window.screenLeft?window.screenLeft:window.screenY) );
html.push( '屏幕分辨率 : ' + "t" + 'W - ' + window.screen.width + ' ; H - ' + window.screen.height );
html.push( '屏幕可用工作区 : ' + "t" + 'W - ' + window.screen.availWidth + ' ; H - ' + window.screen.availHeight );
return html.join( "n" );
}
alert(getPos());

注：随便网上找的代码封装的，有需要的请自行查找相关的资料查阅。

2、获取cookie

function getCookie(c_name){
if (document.cookie.length&gt;0){
c_start=document.cookie.indexOf(c_name + "=");
if (c_start!=-1){
c_start=c_start + c_name.length+1;
c_end=document.cookie.indexOf(";",c_start);
if (c_end==-1) c_end=document.cookie.length;
return unescape(document.cookie.substring(c_start,c_end));
}
}
return "";
}

3、延迟执行

function sleep(n) {
var start = new Date().getTime();
while(true)  if(new Date().getTime()-start &gt; n) break;
}

4、