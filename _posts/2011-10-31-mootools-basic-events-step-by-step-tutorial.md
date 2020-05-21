---
ID: 1782
post_title: 'MooTools Basic: Events Step By Step Tutorial'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/10/mootools-basic-events-step-by-step-tutorial.html
published: true
post_date: 2011-10-31 09:54:01
---
<h3>Window Event</h3>
<table>
<tbody>
<tr>
<th align="left">HTML event attribute</th>
<th align="left">MooTools event name</th>
<th align="left">Description</th>
</tr>
<tr>
<td align="left">onload</td>
<td align="left">load</td>
<td align="left">This event happens when the window and images on the page have fully loaded, and/or when all of the iFrames in the page have loaded</td>
</tr>
<tr>
<td align="left">onunload</td>
<td align="left">unload</td>
<td align="left">This even happens when a window or an iFrame is removed from the web page</td>
</tr>
</tbody>
</table>
<h3>Form Event</h3>
<table>
<tbody>
<tr>
<th align="left">HTML event attribute</th>
<th align="left">MooTools event name</th>
<th align="left">Description</th>
</tr>
<tr>
<td align="left">onblur</td>
<td align="left">blur</td>
<td align="left">This event occurs when an element loses focus</td>
</tr>
<tr>
<td align="left">onchange</td>
<td align="left">change</td>
<td align="left">This event happens when the element loses focus or when its original value has changed</td>
</tr>
<tr>
<td align="left">onfocus</td>
<td align="left">focus</td>
<td align="left">it is triggered when the user focuses on an element</td>
</tr>
<tr>
<td align="left">onreset</td>
<td align="left">reset</td>
<td align="left">This event is triggered when the form has been reset to its default values.</td>
</tr>
<tr>
<td align="left">onselect</td>
<td align="left">select</td>
<td align="left">This event happens when the user highlights (selects) text in a text field</td>
</tr>
<tr>
<td align="left">onsubmit</td>
<td align="left">submit</td>
<td align="left">This event occurs when the user submits a web form</td>
</tr>
</tbody>
</table>
<h3>Keyboard Event</h3>
<table>
<tbody>
<tr>
<th align="left">HTML event attribute</th>
<th align="left">MooTools event name</th>
<th align="left">Description</th>
</tr>
<tr>
<td align="left">onkeydown</td>
<td align="left">keydown</td>
<td align="left">This event occurs when the user holds down a keyboard key</td>
</tr>
<tr>
<td align="left">onkeypress</td>
<td align="left">keypress</td>
<td align="left">This event is triggered whenever the user presses a keyboard key</td>
</tr>
<tr>
<td align="left">onkeyup</td>
<td align="left">keyup</td>
<td align="left">This event happens when the user releases a key.</td>
</tr>
</tbody>
</table>
<h3>Mouse Event</h3>
<table>
<tbody>
<tr>
<th align="left">HTML event attribute</th>
<th align="left">MooTools event name</th>
<th align="left">Description</th>
</tr>
<tr>
<td align="left">onclick</td>
<td align="left">click</td>
<td align="left">This event occurs whenever the user uses the mouse button to click on an element</td>
</tr>
<tr>
<td align="left">ondblclick</td>
<td align="left">dblclick</td>
<td align="left">This event occurs whenever the user double-clicks on an element</td>
</tr>
<tr>
<td align="left">onmousedown</td>
<td align="left">mousedown</td>
<td align="left">This event occurs when the mouse button is held down</td>
</tr>
<tr>
<td align="left">onmouseup</td>
<td align="left">mouseup</td>
<td align="left">This event occurs when the mouse button is released</td>
</tr>
<tr>
<td align="left">onmousemove</td>
<td align="left">mousemove</td>
<td align="left">This event occurs when the mouse is moved</td>
</tr>
<tr>
<td align="left">onmouseout</td>
<td align="left">mouseout</td>
<td align="left">This event occurs when the mouse pointer is removed from the target element</td>
</tr>
<tr>
<td align="left">onmouseover</td>
<td align="left">mouseover</td>
<td align="left">This event occurs when the mouse pointer enters the target element</td>
</tr>
</tbody>
</table>
<h3>MooTools Custom Mouse Event</h3>
<table>
<tbody>
<tr>
<th align="left">MooTools event name</th>
<th align="left">Description</th>
</tr>
<tr>
<td align="left">mouseenter</td>
<td align="left">This event is triggered when the user's mouse pointer enters an element</td>
</tr>
<tr>
<td align="left">mouseleave</td>
<td align="left">triggered only once when the mouse pointer exits the target element</td>
</tr>
<tr>
<td align="left">mousewheel</td>
<td align="left">This even is triggered when the scroller on a mouse is used</td>
</tr>
</tbody>
</table>
&nbsp;

Event: 兼容各浏览器的事件管理

参数:
event  浏览器的event对象

属性:
shift  如果按了键盘的shift键，则返回true
control   如果按了键盘的ctrl键，则返回true
alt   如果按了键盘的alt键，则返回true
meta   如果按了键盘的meta键，则返回true
wheel   鼠标滚轮的滚动量
code  按下的键盘的键代码
page.x   相对于整个浏览器的鼠标x位置
page.y   相对于整个浏览器的鼠标y位置
client.x   相对于可视区域的鼠标x位置
client.y   相对于可视区域的鼠标y位置
key   按下的键盘的键的名称。 如 ‘enter’, ‘up’, ‘down’, ‘left’, ‘right’, ‘space’, ‘backspace’, ‘delete’, ‘esc’.
target   事件的target
relatedTarget   事件的参照target（即前一个事件的target）