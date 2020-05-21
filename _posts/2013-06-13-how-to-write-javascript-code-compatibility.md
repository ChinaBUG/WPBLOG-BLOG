---
ID: 2481
post_title: >
  如何写出兼容性强的Javascript代码？
author: ChinaBUG
post_excerpt: '长久以来JavaScript兼容性一直是Web开发者的一个主要问题。在正式规范、事实标准以及各种实现之间的存在的差异让许多开发者日夜煎熬。为此，主要从以下几方面差异总结IE和Firefox的Javascript兼容性: 一、函数和方法差异； 二、样式访问和设置； 三、DOM方法及对象引用； 四、事件处理； 五、其他差异的兼容处理。'
layout: post
permalink: >
  http://blog.ipodmp.com/2013/06/how-to-write-javascript-code-compatibility.html
published: true
post_date: 2013-06-13 15:06:21
---
作者：望极天涯 发布于：2012-9-14 10:04 分类：JavaScript

长久以来JavaScript兼容性一直是Web开发者的一个主要问题。在正式规范、事实标准以及各种实现之间的存在的差异让许多开发者日夜煎熬。为此，主要从以下几方面差异总结IE和Firefox的Javascript兼容性:

一、函数和方法差异；

二、样式访问和设置；

三、DOM方法及对象引用；

四、事件处理；

五、其他差异的兼容处理。

&nbsp;

一、函数和方法差异

1. getYear()方法

【分析说明】先看一下以下代码： var year= new Date().getYear(); document.write(year); 在IE中得到的日期是"2010"，在Firefox中看到的日期是"110"，主要是因为在 Firefox 里面 getYear 返回的是 "当前年份-1900" 的值。 【兼容处理】 加上对年份的判断，如: var year= new Date().getYear(); year = (year&lt;1900?(1900+year):year); document.write(year); 也可以通过 getFullYear getUTCFullYear 去调用: var year = new Date().getFullYear(); document.write(year);

2. eval()函数

【分析说明】在IE中，可以使用eval("idName")或getElementById("idName")来取得id为idName的HTML对象；Firefox下只能使用getElementById("idName")来取得id为idName的HTML对象。 【兼容处理】统一用getElementById("idName")来取得id为idName的HTML对象。

3. const声明

【分析说明】在 IE 中不能使用const 关键字。如： constconstVar = 32; 在IE中这是语法错误。 【兼容处理】不使用 const ，以 var 代替。

4. var

【分析说明】请看以下代码： echo=function(str){ document.write(str); } pre&gt; 这个函数在IE上运行正常，Firefox下却报错了。 【兼容处理】而在echo前加上var就正常了，这个就是我们提到var的目的。

5. const 问题

【分析说明】在 IE 中不能使用const 关键字。如 const constVar = 32; 在IE中这是语法错误。 【解决方法】不使用 const ，以 var 代替。

二、样式访问和设置 1. CSS的"float"属性 【分析说明】Javascript访问一个给定CSS 值的最基本句法是：object.style.property，但部分CSS属性跟Javascript中的保留字命名相同，如"float"，"for"，"class"等，不同浏览器写法不同。 在IE中这样写： document.getElementById("header").style.styleFloat= "left"; 在Firefox中这样写： document.getElementById("header").style.cssFloat= "left"; 【兼容处理】在写之前加一个判断，判断浏览器是否是IE： if(document.all){ document.getElementById("header").style.styleFloat= "left"; } else{ document.getElementById("header").style.cssFloat= "left"; }

2. 访问&lt;label&gt;标签中的"for" 【分析说明】和"float"属性一样，同样需要使用不现的句法区分来访问&lt;label&gt;标签中的"for"。 在IE中这样写： var myObject = document.getElementById("myLabel"); var myAttribute= myObject.getAttribute("htmlFor"); 在Firefox中这样写： var myObject = document.getElementById("myLabel"); var myAttribute= myObject.getAttribute("for"); 【兼容处理】解决的方法也是先 判断浏览器类型。

3. 访问和设置class属性 【分析说明】同样由于class是Javascript保留字的原因，这两种浏览器使用不同的 JavaScript 方法来获取这个属性。 IE8.0之前的所有IE版本的写法： var myObject = document.getElementById("header"); var myAttribute= myObject.getAttribute("className"); 适用于IE8.0 以及firefox的写法： var myObject = document.getElementById("header"); var myAttribute= myObject.getAttribute("class"); 另外，在使用setAttribute()设置Class属性的时候，两种浏览器也存在同样的差异。 setAttribute("className",value); 这种写法适用于IE8.0之前的所有IE版本，注意：IE8.0也不支持"className"属性了。 setAttribute("class",value);适用于IE8.0 以及 firefox。 【兼容处理】 方法一，两种都写上： var myObject = document.getElementById("header"); myObject.setAttribute("class","classValue"); myObject.setAttribute("className","classValue"); //设置header的class为classValue 方法二，IE和FF都支持object.className，所以可以这样写： var myObject = document.getElementById("header"); myObject.className="classValue";//设置header的class为classValue 方法三，先判断浏览器类型，再根据浏览器类型采用对应的写法。

4. 对象宽高赋值问题 【分析说明】FireFox中类似obj.style.height = imgObj.height 的语句无效。 【兼容处理】统一使用 obj.style.height =imgObj.height + ‘px’;

三、DOM方法及对象引用 1. getElementById 【分析说明】先来看一组代码： &lt;!-- input对象访问1 --&gt; &lt;input id="id" type="button" value="click me" ōnclick="alert(id.value)"/&gt; 在Firefox中，按钮没反应，在IE中，就可以，因为对于IE来说，一个HTML元素的 ID 可以直接在脚本中当作变量名来使用，而Firefox中不可以。 【兼容处理】尽量采用W3C DOM 的写法，访问对象的时候，用document.getElementById("id") 以ID来访问对象，且一个ID在页面中必须是唯一的，同样在以标签名来访问对象的时候，用document.getElementsByTagName("div")[0] 。该方式得到较多浏览器的支持。 &lt;!-- input对象访问2 --&gt; &lt;input id="id" type="button" value="click me" onclick="alert(document.getElementById('id').value)"/&gt;

2. 集合类对象访问 【分析说明】IE下，可以使用()或[]获取集合类对象；Firefox下，只能使用[]获取集合类对象。如： document.write(document.forms("formName").src); //该写法在IE下能访问到Form对象的scrc属性 【兼容处理】将document.forms("formName")改为document.forms["formName"]。统一使用[]获取集合类对象。

3. frame的引用 【分析说明】IE可以通过id或者name访问这个frame对应的window对象，而Firefox只可以通过name来访问这个frame对应的window对象。 例如如果上述frame标签写在最上层的window里面的htm里面，那么可以这样访问： IE： window.top.frameId或者window.top.frameName来访问这个window对象； Firefox：只能这样window.top.frameName来访问这个window对象。 【兼容处理】使用frame的name来访问frame对象，另外，在IE和Firefox中都可以使用window.document.getElementById(”frameId”)来访问这个frame对象。

4. parentElement 【分析说明】IE中支持使用parentElement和parentNode获取父节点。而Firefox只可以使用parentNode。 【兼容处理】因为firefox与IE都支持DOM，因此统一使用parentNode来访问父节点。

5. table操作 【分析说明】IE下table中无论是用innerHTML还是appendChild插入&lt;tr&gt;都没有效果，而其他浏览器却显示正常。 【兼容处理】解决的方法是，将&lt;tr&gt;加到table的&lt;tbody&gt;元素中，如下面所示： var row = document.createElement("tr"); var cell =document.createElement("td"); var cell_text =document.createTextNode("插入的内容"); cell.appendChild(cell_text); row.appendChild(cell); document.getElementsByTagName("tbody")[0].appendChild(row);

6. 移除节点removeNode()和removeChild() 【分析说明】appendNode在IE和Firefox下都能正常使用，但是removeNode只能在IE下用。 removeNode方法的功能是删除一个节点，语法为node.removeNode（false）或者node.removeNode（true），返回值是被删除的节点。 removeNode（false）表示仅仅删除指定节点，然后这个节点的原孩子节点提升为原双亲节点的孩子节点。 removeNode（true）表示删除指定节点及其所有下属节点。被删除的节点成为了孤立节点，不再具有有孩子节点和双亲节点。 【兼容处理】Firefox中节点没有removeNode方法，只能用removeChild方法代替，先回到父节点，在从父节点上移除要移除的节点。 node.parentNode.removeChild(node); // 为了在ie和firefox下都能正常使用，取上一层的父结点，然后remove。

7. childNodes获取的节点 【分析说明】childNodes的下标的含义在IE和Firefox中不同，看一下下面的代码： &lt;ul id="main"&gt; &lt;li&gt;1&lt;/li&gt; &lt;li&gt;2&lt;/li&gt; &lt;li&gt;3&lt;/li&gt; &lt;/ul&gt; &lt;input type=button value="clickme!" onclick= "alert(document.getElementById('main').childNodes.length)"&gt; 分别用IE和Firefox运行，IE的结果是3，而Firefox则是7。Firefox使用DOM规范，"#text"表示文本（实际是无意义的空格和换行等）在Firefox里也会被解析成一个节点，在IE里只有有实际意义的文本才会解析成"#text"。 【兼容处理】 方法一，获取子节点时，可以通过node.getElementsByTagName()来回避这个问题。但是 getElementsByTagName对复杂的DOM结构遍历明显不如用childNodes，因为childNodes能更好的处理DOM的层次结构。 方法二，在实际运用中，Firefox在遍历子节点时，不妨在for循环里加上： if(childNode.nodeName=="#text") continue;//或者使用nodeType == 1。 这样可以跳过一些文本节点。 延伸阅读 《IE和FireFox中的childNodes区别》

8. Firefox不能对innerText支持 【分析说明】Firefox不支持innerText，它支持textContent来实现innerText，不过textContent没有像innerText一样考虑元素的display方式，所以不完全与IE兼容。如果不用textContent，字符串里面不包含HTML代码也可以用innerHTML代替。也可以用js写个方法实现，可参考《为firefox实现innerText属性》一文。 【兼容处理】通过判断浏览器类型来兼容： if(document.all){ document.getElementById('element').innerText = "my text"; } else{ document.getElementById('element').textContent = "my text"; }

四、事件处理 如果在使用javascript的时候涉及到event处理，就需要知道event在不同的浏览器中的差异，主要的JavaScript的事件模型有三种（参考《Supporting Three Event Models atOnce》），它们分别是NN4、IE4+和W3C/Safar。

1. window.event 【分析说明】先看一段代码 function et() { alert(event);//IE: [object] } 以上代码在IE运行的结果是[object]，而在Firefox无法运行。 因为在IE中event作为window对象的一个属性可以直接使用，但是在Firefox中却使用了W3C的模型，它是通过传参的方法来传播事件的，也就是说你需要为你的函数提供一个事件响应的接口。 【兼容处理】添加对event判断，根据浏览器的不同来得到正确的event： function et() { evt=evt?evt:(window.event?window.event:null); //兼容IE和Firefox alert(evt); }

2. 键盘值的取得 【分析说明】IE和Firefox获取键盘值的方法不同，可以理解，Firefox下的event.which与IE下的event.keyCode相当。关于彼此不同，可参考《键盘事件中keyCode、which和charCode 的兼容性测试》 【兼容处理】 function myKeyPress(evt){ //兼容IE和Firefox获得keyBoardEvent对象 evt = (evt) ? evt : ((window.event) ?window.event : "") //兼容IE和Firefox获得keyBoardEvent对象的键值 var key =evt.keyCode?evt.keyCode:evt.which; if(evt.ctrlKey &amp;&amp; (key == 13 || key == 10)){ //同时按下了Ctrl和回车键 //do something; } }

3. 事件源的获取 【分析说明】在使用事件委托的时候，通过事件源获取来判断事件到底来自哪个元素，但是，在IE下，event对象有srcElement属性，但是没有target属性；Firefox下，even对象有target属性，但是没有srcElement属性。 【兼容处理】 ele=function(evt){ //捕获当前事件作用的对象 evt=evt||window.event; return (obj=event.srcElement?event.srcElement:event.target;); }

4. 事件监听 【分析说明】在事件监听处理方面，IE提供了attachEvent和detachEvent两个接口，而Firefox提供的是addEventListener和removeEventListener。 【兼容处理】最简单的兼容性处理就是封装这两套接口： function addEvent(elem, eventName, handler) { if (elem.attachEvent) { elem.attachEvent("on"+ eventName, function(){ handler.call(elem)}); //此处使用回调函数call()，让this指向elem } else if(elem.addEventListener) { elem.addEventListener(eventName,handler, false); } } function removeEvent(elem, eventName, handler) { if (elem.detachEvent) { elem.detachEvent("on"+ eventName, function(){ handler.call(elem)}); //此处使用回调函数call()，让this指向elem } else if(elem.removeEventListener) { elem.removeEventListener(eventName,handler, false); } } 需要特别注意，Firefox下，事件处理函数中的this指向被监听元素本身，而在IE下则不然,可使用回调函数call，让当前上下文指向监听的元素。

5. 鼠标位置 【分析说明】IE下，even对象有x，y属性，但是没有pageX，pageY属性；Firefox下，even对象有pageX，pageY属性，但是没有x，y属性。 【兼容处理】使用mX(mX = event.x ? event.x :event.pageX;)来代替IE下的event.x或者Firefox下的event.pageX。复杂点还要考虑绝对位置。 function getAbsPoint(e){ var x = e.offsetLeft, y = e.offsetTop; while (e = e.offsetParent) { x += e.offsetLeft; y += e.offsetTop; } alert("x:" + x +"," + "y:" + y); }

五、其他差异的兼容处理 1. XMLHttpRequest 【分析说明】new ActiveXObject("Microsoft.XMLHTTP");只在IE中起作用，Firefox不支持，但支持XMLHttpRequest。 【兼容处理】 function createXHR() { var xhr=null; if(window.XMLHttpRequest){ xhr=new ActiveXObject("Msxml2.XMLHTTP"); }else{ try { xhr=new ActiveXObject("Microsoft.XMLHTTP"); } catch() { xhr=null; } } if(!xhr)return; return xhr; }

2. 模态和非模态窗口 【分析说明】IE中可以通过showModalDialog和showModelessDialog打开模态和非模态窗口，但是Firefox不支持。 【解决办法】直接使用window.open(pageURL,name,parameters)方式打开新窗口。 如果需要传递参数，可以使用frame或者iframe。

3. input.type属性问题 IE下 input.type属性为只读，但是Firefox下可以修改

4. 对select元素的option操作 设置options，IE和Firefox写法不同： Firefox:可直接设置 option.text ='foooooooo'; IE:只能设置 option.innerHTML= 'fooooooo'; 删除一个select的option的方法： Firefox:可以 select.options.remove(selectedIndex); IE7:可以用 select.options= null; IE6:需要写 select.options.outerHTML= null;

5. img对象alt和title的解析 【分析说明】img对象有alt和title两个属性，区别在于，alt：当照片不存在或者load错误时的提示。 title：照片的tip说明, 在IE中如果没有定义title，alt也可以作为img的tip使用，但是在Firefox中，两者完全按照标准中的定义使用 在定义img对象时。 【兼容处理】最好将alt和title对象都写全，保证在各种浏览器中都能正常使用。

6. img的src刷新问题 【分析说明】先看一下代码： &lt;img id="pic" onclick= "this.src= 'a.jpg'" src="aa.jpg" style="cursor: pointer"/&gt; 在IE 下，这段代码可以用来刷新图片，但在FireFox下不行。主要是缓存问题。 【兼容处理】在地址后面加个随机数就解决了: &lt;img id="pic" onclick= "javascript:this.src=this.src+'?' 　+Math.random()"src="a.jpg" style="cursor: pointer"/&gt;

总结 IE和Firefox的Javascript方面存在着不少的差异，要做到兼容，我觉得很有必要把一些常见的整理成一个js库，如DOM的操作，事件的处理，XMLHttpRequest请求等，或者也可以选择使用现有的一些库(如jQuery，YUI，ExtJs等)，不过我觉得还是有必要了解一下这些差异，这样对于我们参加兼容性和可用性代码很有帮助。 办法总比问题多，无论浏览器兼容如何折腾人，做前端开发的总能迎刃而解的！