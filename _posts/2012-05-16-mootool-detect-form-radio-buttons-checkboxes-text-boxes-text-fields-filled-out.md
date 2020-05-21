---
ID: 2056
post_title: >
  mootool-检测表单内的单选框、多选框、文本框、文字域是否都填写了
author: ChinaBUG
post_excerpt: |
  今天在做ShopEX领取卷与调查问卷插件，在调查问卷遇上问题，怎么才能识别会员填写了全部的表单呢？对吧，找了一下没找到，郁闷，只好自己写一个了，也算是原创吧。
  在这边与大家分享一下。
  //检测表单内的单选框、多选框、文本框、文字域是否都填写了@ChinaBUG 2012.05.16
layout: post
permalink: >
  http://blog.ipodmp.com/2012/05/mootool-detect-form-radio-buttons-checkboxes-text-boxes-text-fields-filled-out.html
published: true
post_date: 2012-05-16 16:22:14
---
今天在做ShopEX领取卷与调查问卷插件，在调查问卷遇上问题，怎么才能识别会员填写了全部的表单呢？对吧，找了一下没找到，郁闷，只好自己写一个了，也算是原创吧。
在这边与大家分享一下。

//检测表单内的单选框、多选框、文本框、文字域是否都填写了@ChinaBUG 2012.05.16
function check(){
var flag=true,rflag=false;;
var radios=[],checkboxs=[];
//单选框处理
$$('input[type=radio]').each(
function(ele){
if(radios.indexOf(ele.name)&lt;0){
radios.push(ele.name);
}
}
);
for(i=0;i&lt;radios.length;i++){
obj_byname = document.getElementsByName(radios[i]);
rflag = false;
for(j=0;j&lt;obj_byname.length;j++){if(obj_byname[j].checked==true){rflag = true;}}
if(rflag == false){flag = false;}
}
//多选处理
$$('input[type=checkbox]').each(function(ele){if(checkboxs.indexOf(ele.get('class'))&lt;0){checkboxs.push(ele.get('class'));}});
for(i=0;i&lt;checkboxs.length;i++){
rflag = false;
$$('.'+checkboxs[i]).each(function(ele){if(ele.checked==true){rflag = true;}});
if(rflag == false){flag = false;}
}
//文字框处理
rflag = false;
$$('input[type=textarea]').each(function(ele){if(ele.value==''){flag = false;}});
rflag = false;
$$('input[type=text]').each(function(ele){if(ele.value==''){flag = false;}});
if(!flag){
//alert("全部的选项都为必填，请认真作答，谢谢配合。");
MessageBox.error('全部的选项都为必填，请认真作答，谢谢配合。');
return false;
}
return true;
}

2012.05.17补充

上面的代码会将整个页面都计算在内，有时候不喜欢这个功能，怎么有针对性一点呢？修改了一下，增加了指定form对象的功能，调用时 onsubmit="return check('对象的ID名');"

&lt;script type="text/javascript"&gt;
//检测是否都填写了响应的部分@ChinaBUG 2012.05.16
function check(obj){
var flag=true,rflag=false;;
var radios=[],checkboxs=[];
//单选框处理
$(obj).getElements('input[type=radio]').each(
function(ele){
if(radios.indexOf(ele.name)&lt;0){
radios.push(ele.name);
}
}
);
for(i=0;i&lt;radios.length;i++){
obj_byname = document.getElementsByName(radios[i]);
rflag = false;
for(j=0;j&lt;obj_byname.length;j++){if(obj_byname[j].checked==true){rflag = true;}}
if(rflag == false){flag = false;}
}
//多选处理
$(obj).getElements('input[type=checkbox]').each(function(ele){if(checkboxs.indexOf(ele.get('class'))&lt;0){checkboxs.push(ele.get('class'));}});
for(i=0;i&lt;checkboxs.length;i++){
rflag = false;
$$('.'+checkboxs[i]).each(function(ele){if(ele.checked==true){rflag = true;}});
if(rflag == false){flag = false;}
}
//文字框处理
rflag = false;
$(obj).getElements('input[type=textarea]').each(function(ele){if(ele.value==''){flag = false;}});
rflag = false;
$(obj).getElements('input[type=text]').each(function(ele){if(ele.value==''){flag = false;}});
if(!flag){
//alert("全部的选项都为必填，请认真作答，谢谢配合。");
MessageBox.error('全部的选项都为必填，请认真作答，谢谢配合。');
return false;
}
return true;
}
&lt;/script&gt;