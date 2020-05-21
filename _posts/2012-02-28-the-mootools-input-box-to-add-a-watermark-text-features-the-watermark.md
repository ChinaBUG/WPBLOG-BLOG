---
ID: 1968
post_title: >
  mootools-输入框添加水印文字功能(WaterMark)
author: ChinaBUG
post_excerpt: >
  你看过输入框内显示提示信息，而点击之后提示信息自动隐藏，可以输入文字的效果不？这个还真的很常见，这边提供的是mootools版的代码，很简单的一段代码噢。mootools的功能还是不错的。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/02/the-mootools-input-box-to-add-a-watermark-text-features-the-watermark.html
published: true
post_date: 2012-02-28 10:10:20
---
你看过输入框内显示提示信息，而点击之后提示信息自动隐藏，可以输入文字的效果不？这个还真的很常见，这边提供的是mootools版的代码，很简单的一段代码噢。mootools的功能还是不错的。


[code lang="javascript"]&lt;script&gt;
//账户框的水印功能
var values = new Array();
var inputs = new Array();
function addWaterMark(el){
try {
values.push(el.value);
el.addEvent('focus',function(){
if (el.value === values[inputs.indexOf(el)]){el.value = ''};
});
el.addEvent('blur',function(){
if(this.value === ''){el.value = values[inputs.indexOf(el)]};
});
} catch(e) {dbug.log('addWaterMark: ', e)}
};
window.addEvent('domready', function(){
inputs = $$('input.watermark');
inputs.each(addWaterMark);
});
&lt;/script&gt;[/code]