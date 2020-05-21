---
ID: 3226
post_title: >
  Javascript-一个只有100行的原生DOM操作类
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/javascript-one-of-only-100-rows-of-native-dom-manipulation-class.html
published: true
post_date: 2014-03-13 22:21:57
---
今天看了<a href="http://flippinawesome.org/">flippinawesome.org</a>网站的《<a href="http://flippinawesome.org/2014/03/10/a-dom-manipulation-class-in-100-lines-of-javascript/">a dom manipulation class in 100 lines of javascript</a>》一文之后才发现，这世界变化真是大呀~一不留意原来世界已经过去了，咱也错过了什么，老了~

真的100行？恩~其实不在于是不是100行而在于，使用了新的技术特性，我们以后操作更方便了，不需要再引用好大的一个Javascript库了~

下面就是源码：

[code lang="javascript"]
var dom = function(el, parent) {
var api = { el: null }
var qs = function(selector, parent) {
parent = parent || document;
return parent.querySelector(selector);
};
var qsa = function(selector, parent) {
parent = parent || document;
return parent.querySelectorAll(selector);
};
switch(typeof el) {
case 'string':
parent = parent &amp;&amp; typeof parent === 'string' ? qs(parent) : parent;
api.el = qs(el, parent);
break;
case 'object':
if(typeof el.nodeName != 'undefined') {
api.el = el;
} else {
var loop = function(value, obj) {
obj = obj || this;
for(var prop in obj) {
if(typeof obj[prop].el != 'undefined') {
obj[prop] = obj[prop].val(value);
} else if(typeof obj[prop] == 'object') {
obj[prop] = loop(value, obj[prop]);
}
}
delete obj.val;
return obj;
}
var res = { val: loop };
for(var key in el) {
res[key] = dom.apply(this, [el[key], parent]);
}
return res;
}
break;
}
api.val = function(value) {
if(!this.el) return null;
var set = !!value;
var useValueProperty = function(value) {
if(set) { this.el.value = value; return api; }
else { return this.el.value; }
}
switch(this.el.nodeName.toLowerCase()) {
case 'input':
var type = this.el.getAttribute('type');
if(type == 'radio' || type == 'checkbox') {
var els = qsa('[name=&quot;' + this.el.getAttribute('name') + '&quot;]', parent);
var values = [];
for(var i=0; i&lt;els.length; i++) {
if(set &amp;&amp; els[i].checked &amp;&amp; els[i].value !== value) {
els[i].removeAttribute('checked');
} else if(set &amp;&amp; els[i].value === value) {
els[i].setAttribute('checked', 'checked');
els[i].checked = 'checked';
} else if(els[i].checked) {
values.push(els[i].value);
}
}
if(!set) { return type == 'radio' ? values[0] : values; }
} else {
return useValueProperty.apply(this, [value]);
}
break;
case 'textarea':
return useValueProperty.apply(this, [value]);
break;
case 'select':
if(set) {
var options = qsa('option', this.el);
for(var i=0; i&lt;options.length; i++) {
if(options[i].getAttribute('value') === value) {
this.el.selectedIndex = i;
} else {
options[i].removeAttribute('selected');
}
}
} else {
return this.el.value;
}
break;
default:
if(set) {
this.el.innerHTML = value;
} else {
if(typeof this.el.textContent != 'undefined') {
return this.el.textContent;
} else if(typeof this.el.innerText != 'undefined') {
return typeof this.el.innerText;
} else {
return this.el.innerHTML;
}
}
break;
}
return set ? api : null;
}
return api;
}[/code]

从代码中我们发现了两个专业的方法：querySelector 和 querySelectorAll。

语法如下：

原始：

[code lang="javascript"]document.getElementById(&quot;test&quot;);[/code]

新的：

[code lang="javascript"]document.querySelector(&quot;#test&quot;);

document.querySelectorAll(&quot;#test&quot;)[0];[/code]

具体说明可以google或者查看<a href="http://www.nowamagic.net/librarys/veda/detail/388">原生的强大DOM选择器querySelector和querySelectorAll</a>一文