---
ID: 501
post_title: AddEventListener
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2010/05/flash-as-addeventlistener.html
published: true
post_date: 2010-05-05 11:22:46
---
menu.addEventListener(<strong><span style="text-decoration: underline;">Event.ENTER_FRAME</span></strong>, onEnterFrameHandler);

或者

menu.addEventListener(<span style="text-decoration: underline;"><strong>"enterFrame"</strong></span>, onEnterFrameHandler);

function onEnterFrameHandler(event:MouseEvent):void {
       trace(event.<code>stageX</code>);
}

<a href="http://www.adobe.com/livedocs/flash/9.0/ActionScriptLangRefV3/flash/events/MouseEvent.html" target="_blank">鼠标事件参考</a>