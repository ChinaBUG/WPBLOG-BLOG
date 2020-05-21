---
ID: 2204
post_title: >
  Mootools-用Mootools寫的一个类似facebook的弹出对话框(支持html内容及框架)
author: ChinaBUG
post_excerpt: |
  /*/
  mootools版本要求:
  mootools1.4以上
  
  調用并顯示普通內容的方法:
  Dilog({tit:'這是標題',htm:'這是內容<br>This should be big enough?'});
  
  調用框架頁面的方法:
  Dilog({tit:'這是標題',htm:'http://7di.net',iframe:'yes',cov:true});
  /*/
layout: post
permalink: >
  http://blog.ipodmp.com/2012/10/mootools-dilog-as-facebook-pop-window.html
published: true
post_date: 2012-10-15 17:04:55
---
/*/
mootools版本要求:
mootools1.4以上

調用并顯示普通內容的方法:
Dilog({tit:'這是標題',htm:'這是內容&lt;br&gt;This should be big enough?'});

調用框架頁面的方法:
Dilog({tit:'這是標題',htm:'http://7di.net',iframe:'yes',cov:true});
/*/

//核心代碼:
var Dilog=function(o){
document.getElement('body').setStyle('overflow','hidden');
if(!o || !o.htm){
if($('xDialog')){$('xDialog').destroy();}
if($('xCover')){$('xCover').destroy();}
document.getElement('body').setStyle('overflow','auto');
return false;
}
if($('xDialog')){return ;}//如果已經打開了一個則不可再打開第二個

w=document.documentElement.clientWidth;
o.tit=(o.tit) || '易居網';                //標題
o.wid=(o.wid) || '560px';                //寬度
o.hgt=(o.hgt) || '450px';                //高度
o.cov=(o.cov) || false;                    //是否遮罩
o.covcl=(o.covcl) || 'white';                    //是否遮罩
o.btn=(typeOf(o.btn)=='boolean')?o.btn:true;    //是否有關閉按鈕
o.lft=(o.lft) || ((w-(o.wid.toInt()))/3).toInt()+'px';
o.top=(o.obj)?o.obj.getPosition().y+'px':(parseInt(document.documentElement.scrollTop,10))+'px';
css='#xDialog{padding:8px;margin:0;z-index:1201;position:absolute;top:'+o.top+';background:transparent url(img/bg.png);border:0px solid #888;min-width:200px;}';
css+='#xDialog .box{border:1px solid #3B5998;background:white;}';
css+='#xDialog .title{background:#6D84B4;color:white;cursor:default;font-size:14px;font-weight:bold;letter-spacing:1px;line-height:30px;padding:1px 8px;height:30px;}#xDialog .title span{font-size:14px;}';
css+='#xDialog .message{line-height:20px;font-size:13px;color:#333;}';
css+='#xDialog .close{background:url("img/ico.gif") no-repeat scroll -134px -60px transparent;cursor:pointer;float:right;height:20px;width:20px;}';
if(o.cov){css+='#xCover{border:0;left:0;top:0;position:fixed;right:0;bottom:0;background:'+o.covcl+';z-index:1200;filter:Alpha(opacity=80);-moz-opacity:.8;opacity:0.8;}';}

AddCss(css,'xDialog_css');

$xCover=new Element('div',{'id':'xCover','style':'display:block;'}).inject(document.body,'top');
$xDialog=new Element('div',{'id':'xDialog','style':'width:'+o.wid+';left:'+o.lft+';top:'+o.top+';visibility:visible;zoom:1;opacity:1;'}).inject(document.body,'top');

o.btn=(o.btn)?'&lt;span title="Close Dialog" onclick="Dilog();"&gt;&lt;/span&gt;':'';
if(o.iframe=='yes'){
$xDialog.set('html','&lt;div&gt;&lt;div&gt;&lt;span style="float:left;"&gt;'+o.tit+'&lt;/span&gt;'+o.btn+'&lt;/div&gt;&lt;div id="xDil_mess" style="height:auto;"&gt;&lt;iframe src="'+o.htm+'" name="InfoFrame" scrolling="auto" width="100%" style="height:'+o.hgt+';" frameborder="0"&gt;&lt;/iframe&gt;&lt;/div&gt;&lt;/div&gt;');
}else{
$xDialog.set('html','&lt;div&gt;&lt;div&gt;&lt;span style="float:left;"&gt;'+o.tit+'&lt;/span&gt;'+o.btn+'&lt;/div&gt;&lt;div id="xDil_mess" style="height:auto;"&gt;'+o.htm+'&lt;/div&gt;&lt;/div&gt;');
}
o=null;
};