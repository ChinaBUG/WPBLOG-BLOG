---
ID: 1608
post_title: >
  ShopEX-二次开发-商城多地区选项挂件的设计(mootools版)
author: ChinaBUG
post_excerpt: |
  这个完全是偶自己整的，太不容易了，mootools太不容易弄了哈~但是懂了，就会觉得简单哈~抽空要好好的学习学习哈
  主要使用到下面的方法：
  Cookie.write("[area]",area);//设置COOKIE的值
  var zw_idx = ['xm','ct','qz','jj','ss','zz'].indexOf(area);//查找匹配area的值返回所在的索引
  $('area_info').set('text', zw[zw_idx]);//设置对象的文字值，相当于innerHTML
  $('area_info').get('text');//获取对象的文字值，与上面是配套地
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-store-option-in-many-parts-of-the-design-mootools-version.html
published: true
post_date: 2011-07-13 17:59:44
---
这个完全是偶自己整的，太不容易了，mootools太不容易弄了哈~但是懂了，就会觉得简单哈~抽空要好好的学习学习哈

&lt;script language="javascript" type="text/javascript"&gt;
window.addEvent('domready', function(){
//载入时,自动设置所在地
setArea( Cookie.read('[area]') );
//下拉菜单
$('drop_down_menu').getElements('li.menu').each( function( elem ){
var list = elem.getElement('ul.links');
var myFx = new Fx.Slide(list).hide();
elem.addEvents({
'mouseenter' : function(){
myFx.cancel();
myFx.slideIn();
},
'mouseleave' : function(){
myFx.cancel();
myFx.slideOut();
}
});
})
});
//设置地区的方法
function setArea( area ){
Cookie.write("[area]",area);
var zw=['厦门','长泰','泉州','晋江','石狮','漳州'];
var zw_idx = ['xm','ct','qz','jj','ss','zz'].indexOf(area);
$('area_info').set('text', zw[zw_idx]);
//alert( $('area_info').get('text') );
}
///
&lt;/script&gt;
&lt;style type="text/css" media="screen"&gt;
#menu-container { display:block; position:relative; width:180px; margin:0px auto 0px; font-size:11px; }
#drop_down_menu { display:block; position:absolute; clear:both; margin:0px; padding:0px; text-align:left; list-style-type:none; text-align:center; width:180px; float:none; left:0px; top:0px; }
#drop_down_menu li { font-size:12px; font-weight:bold; float:left; color:#11a2db; padding:0px; cursor:pointer;width:180px; }
#drop_down_menu li ul { margin:0px; padding:0px; list-style-type:none; padding-top:0px;background: #F0F0F0; }
#drop_down_menu li ul li { display:block; float:none; clear:both;  }
#drop_down_menu li ul li a { color:#000; /*font-weight:normal;*/ text-decoration:none; display:block; }
#drop_down_menu li ul li a:hover { text-decoration:underline; color:#CCCCCC; }
&lt;/style&gt;
&lt;div id="menu-container"&gt;
&lt;ul id="drop_down_menu"&gt;
&lt;li&gt;当前送货地区为：&lt;span id="area_info"&gt;&lt;/span&gt;
&lt;ul&gt;
&lt;li&gt;&lt;a href="javascript:setArea('jj');"&gt;晋江&lt;/a&gt;&lt;/li&gt;
&lt;li&gt;&lt;a href="javascript:setArea('ss');"&gt;石狮&lt;/a&gt;&lt;/li&gt;
&lt;li&gt;&lt;a href="javascript:setArea('ct');"&gt;长泰&lt;/a&gt;&lt;/li&gt;

&lt;/ul&gt;
&lt;/li&gt;

&lt;/ul&gt;
&lt;/div&gt;