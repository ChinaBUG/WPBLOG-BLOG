---
ID: 2528
post_title: >
  ShopEX-mootools图片特效之图像图像幻灯展示(image
  slider)
author: ChinaBUG
post_excerpt: |
  最近在设计ShopEX的全屏幻灯展示的时候，之前一直使用一个免费插件，里面只能是顺序一直轮播，到尾就一个个的滚动回第一页，客户觉得这个效果很呆板，没办法，修改吧，就偷了一下s.cn家的幻灯展示的代码了，还不错，挺容易使用的，包装一下做成视图吧。
  下面代码是完整的，请保存为视图调用即可，不懂得制作ShopEX的挂件的朋友可以自学或者联系我有偿提供噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/07/shopex-mootools-picture-effects-of-image-slide-show-image-slider.html
published: true
post_date: 2013-07-31 09:34:44
---
最近在设计ShopEX的全屏幻灯展示的时候，之前一直使用一个免费插件，里面只能是顺序一直轮播，到尾就一个个的滚动回第一页，客户觉得这个效果很呆板，没办法，修改吧，就偷了一下s.cn家的幻灯展示的代码了，还不错，挺容易使用的，包装一下做成视图吧。

下面代码是完整的，请保存为视图调用即可，不懂得制作ShopEX的挂件的朋友<span style="text-decoration: underline;">可以自学</span>或者联系我<span style="text-decoration: underline; color: #ff0000;">有偿提供</span>噢。

&lt;script&gt;
var Tabs = new Class({
Implements: [Options, Events],
options: {
selectedTabCss: "on",
selectedSectionCss: "on",
mvSH: 1,
firstShow: 0,
autoInterval: 4,
mouseEvent: "mouseover"
},
initialize: function(tabs, sections, options) {
this.setOptions(options);
this.current = this.options.firstShow;
this.tabs = $$(tabs);
this.sections = $$(sections);
this.sectionsLength = $$(sections).length;
this.attach();
this.render()
},
render: function() {
this.sections.each(function(section, index) {
if (index !== this.current) {
section.hide()
} else {
this.showSection(index)
}
},
this)
},
attach: function() {
this.tabs.each(function(tab, index) {
tab.addEvent(this.options.mouseEvent, this.tabEvent.bindWithEvent(this, tab))
},
this)
},
tabEvent: function(e, tab) {
e.preventDefault();
var index = this.tabs.indexOf(tab);
this.toggleSection(index)
},
toggleSection: function(index) {
if (this.current === index) {
return
}
this.hideSection(this.current);
this.current = index;
this.showSection(this.current)
},
showSection: function(index) {
var tab = this.tabs[index];
var section = this.sections[index];
tab.addClass(this.options.selectedTabCss);
section.addClass(this.options.selectedSectionCss).show();
this.fireEvent("onShow", [index, tab, section])
},
hideSection: function(index) {
if (index === false) {
return
}
var tab = this.tabs[index];
var section = this.sections[index];
tab.removeClass(this.options.selectedTabCss);
section.removeClass(this.options.selectedSectionCss).hide();
this.fireEvent("onHide", [index, tab, section])
}
});
Tabs.implement({
showSection: function(index) {
var tab = this.tabs[index];
var section = this.sections[index];
switch (this.options.mvSH) {
case 1:
section.setStyles({
display:
"block",
opacity: 0
}).fade(1);
tab.addClass(this.options.selectedTabCss);
break;
default:
tab.addClass(this.options.selectedTabCss);
section.addClass(this.options.selectedSectionCss).show()
}
this.fireEvent("onShow", [index, tab, section])
},
hideSection: function(index) {
if (index === false) {
return
}
var tab = this.tabs[index];
var section = this.sections[index];
switch (this.options.mvSH) {
case 1:
section.hide();
tab.removeClass(this.options.selectedTabCss);
break;
default:
tab.removeClass(this.options.selectedTabCss);
section.removeClass(this.options.selectedSectionCss).hide()
}
this.fireEvent("onHide", [index, tab, section])
}
});
Tabs.implement({
startAuto: function() {
this.attachAuto();
this.start()
},
attachAuto: function() {
this.bindOver = this.stop.bind(this);
this.bindOut = this.start.bind(this);
this.tabs.getParent().addEvents({
"mouseenter": this.bindOver,
"mouseleave": this.bindOut
});
this.sections.getParent().addEvents({
"mouseenter": this.bindOver,
"mouseleave": this.bindOut
})
},
start: function() {
this.autoId = this.autoToggle.periodical(this.options.autoInterval * 1000, this)
},
stop: function() {
$clear(this.autoId)
},
autoToggle: function() {
this.numNext = this.current + 1;
this.numNext = this.numNext &gt;= this.sectionsLength ? 0 : this.numNext;
this.toggleSection(this.numNext)
}
});

window.addEvent('domready', function() {
$('slidewigetsStyle').inject($E('link'), 'before');
var bgS = $(document).getSize();
$('tabm_s_nav').setStyle('left',bgS.x/2);
var tab_1 = new Tabs($$('#tabm_s_nav li'),$$('#tabm_s_con li'),{autoInterval:'6'});
tab_1.startAuto();
});
&lt;/script&gt;
&lt;style id="slidewigetsStyle" name="&lt;{$setting.slideTime}&gt;"&gt;
.m131_scroll .scroll_nav{position:absolute;bottom:10px;left:260px;z-index:3}
.m131_scroll .scroll_nav li{display:block;float:left;width:14px;height:14px;text-indent:-99px;overflow:hidden;background:#B5B5B5;border-radius:50%;margin-right:8px;cursor:pointer}
.m131_scroll .scroll_nav li.on{background:#C80002}
&lt;/style&gt;
&lt;div style="position:relative; display:block; clear:both;height:&lt;{$setting.height}&gt;px;overflow:hidden;width:100%;"&gt;
&lt;div&gt;
&lt;ul id="tabm_s_con"&gt;
&lt;{foreach from=$setting.pic item=data key=key}&gt;
&lt;li&gt;
&lt;a href="&lt;{$data.linktarget}&gt;" title="&lt;{$data.title}&gt;" target="_blank"&gt;&lt;img width="&lt;{$setting.width}&gt;" height="&lt;{$setting.height}&gt;"  src="&lt;{$data.link}&gt;"  title="&lt;{$data.title}&gt;"/&gt;&lt;/a&gt;
&lt;/li&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;ul id="tabm_s_nav"&gt;
&lt;{foreach from=$setting.pic item=data key=key name="item"}&gt;
&lt;li&gt;&lt;{$smarty.foreach.item.iteration}&gt;&lt;/li&gt;
&lt;{/foreach}&gt;
&lt;/ul&gt;
&lt;/div&gt;
&lt;/div&gt;