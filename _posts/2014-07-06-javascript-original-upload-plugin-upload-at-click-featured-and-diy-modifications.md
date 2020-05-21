---
ID: 3531
post_title: >
  Javascript-原生上传插件upload-at-click推荐及DIY修改
author: ChinaBUG
post_excerpt: |
  虫曰：
  最近在做项目时，需要使用到上传功能哈~做人太懒了，所以找了一下发现upload-at-click这家伙很棒~可能是javascript原生开发的比较简单还好用的插件第一吧，虫子不经常使用所以不知道具体的哈，不管怎样还是推荐一下。以后要常用了，毕竟现在的项目都是在JQuery与Mootools之间相互切换，哎，不兼容是一个大问题。
  插件网址：upload-at-click - Javascript for upload file at one click
  插件号称一键上传，实际上使用是真的一键操作！而参数也才数个，配置不像其他的那么复杂。
  需要插件的朋友可以点击上面的链接直接去下载，非常棒，值得拥有。
  不过在实际项目中用，出现一个问题就是不想上传文件怎么办？
  原插件中没有这个功能或者是我不懂吧，反正我自己改了一下哈，挺实用的噢，下面就是修改的过程噢，与您分享。
layout: post
permalink: >
  http://blog.ipodmp.com/2014/07/javascript-original-upload-plugin-upload-at-click-featured-and-diy-modifications.html
published: true
post_date: 2014-07-06 22:44:56
---
虫曰：

最近在做项目时，需要使用到上传功能哈~做人太懒了，所以找了一下发现upload-at-click这家伙很棒~可能是javascript原生开发的比较简单还好用的插件第一吧，虫子不经常使用所以不知道具体的哈，不管怎样还是推荐一下。以后要常用了，毕竟现在的项目都是在JQuery与Mootools之间相互切换，哎，不兼容是一个大问题。

插件网址：<a href="https://code.google.com/p/upload-at-click/" target="_blank">upload-at-click - Javascript for upload file at one click</a>
<blockquote>2015.10.30 update http://upload-at-click.narod.ru/demo.html</blockquote>
插件号称一键上传，实际上使用是真的一键操作！而参数也才数个，配置不像其他的那么复杂。

需要插件的朋友可以点击上面的链接直接去下载，非常棒，值得拥有。

不过在实际项目中用，出现一个问题就是不想上传文件怎么办？

原插件中没有这个功能或者是我不懂吧，反正我自己改了一下哈，挺实用的噢，下面就是修改的过程噢，与您分享。

&nbsp;

&nbsp;

要修改插件，第一步是要下载文件，就一个文件噢，名字叫upclick-min.js，这个事压缩版的，格式基本不好看，不用怕，我们进入<a href="http://tool.chinaz.com/Tools/JsFormat.aspx" target="_blank">JavaScript/HTML格式化</a>里面，将upclick-min.js里面的内容都拷贝到文本框内，点击格式化，然后，你就能看得懂代码了噢。

本来我想好好地研究一下这个的原理，然后发现里面的东西都是简短的变量头痛，就算了，不仔细研究了，直接就输入关键词onstart，然后就会找到一个地方，你要问我为什么选择这个关键词？其实这个事靠感觉的，有时候是没道理的，不过这个DIY教程里没有道理的也要瞎扯一个道理噢，选择这个关键词主要是因为这个插件支持两个事件（onstart，oncomplete），我用这个主要是想要找到相关的事件处理的逻辑，看看能不能模仿写一个哈。

我们找到的代码如下：

[code lang="javascript"]

var o = function() {
if (b.value) {
var f = d.onstart;
f &amp;&amp; f(b.value);
a.submit();
}
};

[/code]

这个逻辑我们一看，完蛋了！想要模仿写一个取消事件的功能，需要费很大的力！

因为这个逻辑中通直接就是将参数返给我们定制的开始事件处理函数，然后直接提交表单，开始上传！

既然我们知道他先执行我们定制的开始事件处理函数，然后才上传，那么我们要取消的话就是需要在这边提交表单，开始上传的时候下一个断点，加入我们的代码，破坏整个的上传流程。

所以，我就修改了下面的代码噢

[code lang="javascript"]
var o = function() {
if (b.value) {
var f = d.onstart;
f &amp;&amp; (err = f(b.value));
if(err!=false){
a.submit();
}else alert(error);
}
};[/code]

然后，修改就完成了噢，只要在onstart里面判断我们的条件，然后返回值，返回不是false的话都是可以直接上传，如果要破坏上传的话，那么就是返回true呗~顺便要设置一下error变量。

具体调用如下：

[code lang="javascript"]
var uploader = document.getElementById('uploadbtn');
upclick({
element: uploader,
dataname:'Filedata',
maxsize:0,
action: 'URL',
onstart:function(filename){
if(countItems()&lt;3){
var myElement = new Element('div',{class:'pj-box-item',html:'&lt;input name=&quot;ShowImages[]&quot; type=&quot;hidden&quot; value=&quot;images/'+filename+'&quot; /&gt;&lt;img style=&quot;width:120px;height:160px;&quot; src=&quot;images/'+filename+'&quot;/&gt;'});
PJboxitems.grab(myElement, 'top');
}else{
this.error = '对不起，您只能上传3张图片.';//这个错误提示会被调用噢
return false;
}
},
oncomplete:function(response_data){
alert(response_data);
}
});[/code]

&nbsp;

---

摘录原插件作者的教程噢，防止网站打不开~~
<h1>如何使用？</h1>
<h2><a name="Developer._4_steps:"></a>开发四步骤:</h2>
1. 增加脚本库链接:
<pre class="prettyprint"><span class="pln">   </span><span class="tag">&lt;script</span> <span class="atn">type</span><span class="pun">=</span><span class="atv">"text/javascript"</span> <span class="atn">src</span><span class="pun">=</span><span class="atv">"/path_to/upclick-min.js"</span><span class="tag">&gt;&lt;/script&gt;</span></pre>
2. 增加一个文件上传元素:
<pre class="prettyprint"><span class="pln">   </span><span class="tag">&lt;input</span> <span class="atn">type</span><span class="pun">=</span><span class="atv">"button"</span> <span class="atn">id</span><span class="pun">=</span><span class="atv">"uploader"</span> <span class="atn">value</span><span class="pun">=</span><span class="atv">"Upload"</span><span class="tag">&gt;</span></pre>
3. 调用 <strong>upclick()</strong> 方法:
<pre class="prettyprint"><span class="pln">   </span><span class="tag">&lt;script</span> <span class="atn">type</span><span class="pun">=</span><span class="atv">"text/javascript"</span><span class="tag">&gt;</span><span class="pln">

   </span><span class="kwd">var</span><span class="pln"> uploader </span><span class="pun">=</span><span class="pln"> document</span><span class="pun">.</span><span class="pln">getElementById</span><span class="pun">(</span><span class="str">'uploader'</span><span class="pun">);</span><span class="pln">

   upclick</span><span class="pun">(</span><span class="pln">
     </span><span class="pun">{</span><span class="pln">
      element</span><span class="pun">:</span><span class="pln"> uploader</span><span class="pun">,</span><span class="pln">
      action</span><span class="pun">:</span> <span class="str">'/path_to/you_server_script.php'</span><span class="pun">,</span><span class="pln"> 
      onstart</span><span class="pun">:</span><span class="pln">
        </span><span class="kwd">function</span><span class="pun">(</span><span class="pln">filename</span><span class="pun">)</span><span class="pln">
        </span><span class="pun">{</span><span class="pln">
          alert</span><span class="pun">(</span><span class="str">'Start upload: '</span><span class="pun">+</span><span class="pln">filename</span><span class="pun">);</span><span class="pln">
        </span><span class="pun">},</span><span class="pln">
      oncomplete</span><span class="pun">:</span><span class="pln">
        </span><span class="kwd">function</span><span class="pun">(</span><span class="pln">response_data</span><span class="pun">)</span><span class="pln"> 
        </span><span class="pun">{</span><span class="pln">
          alert</span><span class="pun">(</span><span class="pln">response_data</span><span class="pun">);</span><span class="pln">
        </span><span class="pun">}</span><span class="pln">
     </span><span class="pun">});</span><span class="pln">

   </span><span class="tag">&lt;/script&gt;</span></pre>
4. 书写服务端程序.
<i>上传数据的名字叫做： <strong>Filedata</strong>.</i>
<h3><a name="Server-side_script_example_1"></a>服务端脚本例子1</h3>
<pre class="prettyprint"><span class="pun">&lt;?</span><span class="pln">php
$tmp_file_name </span><span class="pun">=</span><span class="pln"> $_FILES</span><span class="pun">[</span><span class="str">'Filedata'</span><span class="pun">][</span><span class="str">'tmp_name'</span><span class="pun">];</span><span class="pln">
move_uploaded_file</span><span class="pun">(</span><span class="pln">$tmp_file_name</span><span class="pun">,</span> <span class="str">'/path_to/new_filename'</span><span class="pun">);</span></pre>
<h3><a name="Server-side_script_example_2"></a>服务器脚本例子2</h3>
<pre class="prettyprint"><span class="pun">&lt;?</span><span class="pln">php
$tmp_file_name </span><span class="pun">=</span><span class="pln"> $_FILES</span><span class="pun">[</span><span class="str">'Filedata'</span><span class="pun">][</span><span class="str">'tmp_name'</span><span class="pun">];</span><span class="pln">
$ok </span><span class="pun">=</span><span class="pln"> move_uploaded_file</span><span class="pun">(</span><span class="pln">$tmp_file_name</span><span class="pun">,</span> <span class="str">'/path_to/new_filename'</span><span class="pun">);</span>

<span class="com">// This message will be passed to 'oncomplete' function</span><span class="pln">
echo $ok </span><span class="pun">?</span> <span class="str">"OK"</span> <span class="pun">:</span> <span class="str">"FAIL"</span><span class="pun">;</span></pre>
<h1>参数表</h1>
全部的参数都保存在一个对象中.

可能的关键词如下:
<table class="wikitable">
<tbody>
<tr>
<td><strong>Name</strong></td>
<td><strong>Type</strong></td>
<td><strong>Description</strong></td>
<td><strong>Default</strong></td>
</tr>
<tr>
<td>element</td>
<td>object|string</td>
<td>DOM element or element id</td>
<td></td>
</tr>
<tr>
<td>action</td>
<td>string</td>
<td>Server script who receive file</td>
<td></td>
</tr>
<tr>
<td>action_params</td>
<td>object</td>
<td>Server script parameters.
Example:
{name: 'Amadeus', family: 'Mozart'}</td>
<td>{}</td>
</tr>
<tr>
<td>dataname</td>
<td>string</td>
<td>File data name</td>
<td>'Filedata'</td>
</tr>
<tr>
<td>maxsize</td>
<td>integer</td>
<td>Maximum file size. In Bytes. 0 - unlimited (work in IE only)</td>
<td>0</td>
</tr>
<tr>
<td>target</td>
<td>string</td>
<td>Target window name for response. Like a <tt>_blank</tt>, <tt>_self</tt>, <tt>_top</tt> or some frame name</td>
<td></td>
</tr>
<tr>
<td>zindex</td>
<td>integer|'auto'</td>
<td>z-index style property of listener</td>
<td>'auto'</td>
</tr>
<tr>
<td>onstart</td>
<td>function</td>
<td>Callback function
Emited on start upload.
onstart(filename)</td>
<td>null</td>
</tr>
<tr>
<td>oncomplete</td>
<td>function</td>
<td>Callback function
Emited when file successfully uploaded
oncomplete(server_response_data)</td>
<td>null</td>
</tr>
</tbody>
</table>
<h1><a name="Tested_with_browsers:"></a>如下浏览器已测试：</h1>
<ul>
	<li>IE 6.0</li>
	<li>FireFox 3.6</li>
	<li>Chrome 6.0</li>
	<li>Opera 10.63</li>
</ul>
&nbsp;