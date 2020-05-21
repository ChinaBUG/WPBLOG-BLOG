---
ID: 1905
post_title: mootools基本常用语法参考
author: ChinaBUG
post_excerpt: |
  1、基本的选择器
  2、样式操作相关
  3、对象元素的操作
  4、事件
layout: post
permalink: >
  http://blog.ipodmp.com/2012/01/mootools-commonly-used-in-reference-to-the-basic-syntax-mootools.html
published: true
post_date: 2012-01-31 13:47:38
---
1、基本的选择器

$（"元素ID"）

$$（"#元素ID|.元素样式类名|标签名"）

2、样式操作相关

ELE.setStyle("border-top","none");

ELE.getStyle("border-top");

ELE.addClass('hover');

ELE.removeClass('hover');

注意：属性值的正确使用，特别是数字与字符串的区别有带单位跟不带单位的。

3、对象元素的操作

ELE.each(function(item,index) {......});

ELE.getElement('ul');

ELE.get('class');

ELE.getPrevious();

ELE.getAllPrevious();

ELE.getNext();

ELE.getAllNext();

ELE.getFirst();

ELE.getLast();

ELE.getParent();

ELE.getParents();

ELE.getSiblings();

ELE.getChildren();

ELE.removeProperty('required');

注：对于框架(iframe)内操作框架父框架则需要使用parent.$('setquick').get('menu_text')

4、事件

ELE.addEvents({
mouseenter:function() {......},
mouseleave:function() {......}
});

ELE.addEvent('click', function(e){...... });

5、循环对象.each方法

inputs = $$('input.watermark');
inputs.each(addWaterMark);//addWaterMark方法

6、在数组中查找元素是否存在

array.indexOf(value,from)
value:要查找的值；from:从什么地方开始查找，默认是0

7、在模板内查找对应的内容给予替换

substitute()

如：

var text = "{name} is good~";
var result = text.substitute({name:'ChinaBUG'});

****************************************************

实例演练

****************************************************

&lt;fieldset&gt;
&lt;legend&gt;按会员资料筛选&lt;/legend&gt;&lt;input name="seo_id" type="hidden" value="&lt;{$node.node_id}&gt;" /&gt;
&lt;label&gt;关键字：&lt;input type="text" name="seo_keyword" class='_x_ipt newNodeTitle' id="seo_keyword" value="&lt;{$seo_data.keywords}&gt;" /&gt;&lt;/label&gt;
&lt;label&gt;描述：&lt;input type="text" name="seo_desc" class='_x_ipt newNodeTitle' id="seo_desc" value="&lt;{$seo_data.descript}&gt;" /&gt;&lt;/label&gt;
&lt;/fieldset&gt;

&lt;script&gt;
window.addEvent(
'domready',
function(){
$$('fieldset').each(function(ele){
ele.getElement('legend').addEvent('click',function(){
ele.getElements('label').each(function(el){
if(el.hasClass('on')){
el.getChildren('input').set('disabled','disabled');
el.removeClass('on');
}else{
el.getChildren('input').set('disabled','');
el.addClass('on');
}
});
});
}
);
});
&lt;/script&gt;