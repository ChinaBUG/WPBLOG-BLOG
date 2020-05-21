---
ID: 3216
post_title: jQuery-$.getJSON()跨域请求
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/jquery-getjson-cross-domain-requests.html
published: true
post_date: 2014-03-11 10:11:18
---
最近在研究SharePoint APP的开发时，希望从SharePoint hosted上获取自己服务器的数据，进行数据交换，然后就发现了$.getJSON()的作用，然后不懂这个方法，结果死调试得不出一个结果出来~

其实在例子之中就已经体现了，可惜，没这方面的基础直接给跳过去了噢

var parameters = {
tagsmode: "any",
format: "json"
};
$.getJSON("https://secure.flickr.com/services/feeds/photos_public.gne?jsoncallback=?",
parameters,
function (results) {
$.each(results.items, function (index, item) {
$('#Images').append($("&lt;img /&gt;").attr("src", item.media.m));
});
}
);

换了好多的关键词，终于找到解答的文章，感动呀：<a href="www.cnblogs.com/loogn/archive/2011/12/21/2295772.html">$.getJSON()跨域请求</a>

原文摘抄一段吧，留个历史~

2，不同域名下

js:
var url="http://localhost:2589/a.ashx?callback=?";
$(function(){
$.getJSON(url,function(data){
alert (data.Name);
})
});

服务器返回字符串：

jQuery1706543070425920333_1324445763158({"Name":"loogn","Age":23})

返回的字符串就是一个调用一个叫“jQuery1706543070425920333_1324445763158” 的函数，参数是{"Name":"loogn","Age":23}。

其实这个很长的函数名是请求路径中callback=?的作用，我想应该是这样的：$.getJSON方法生成一个对回调方法的引用的名字，换掉？。上面请求会变成

http://localhost:2589/a.ashx?callback=jQuery1706543070425920333_1324445763158&amp;_=1324445763194，所以服务器回返json时要处理一下，如：

string cb = context.Request["callback"];
context.Response.Write(cb + "(" + json + ")");