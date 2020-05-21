---
ID: 2238
post_title: >
  ThinkPHP-ThinkPHP中的Widget扩展详解
author: ChinaBUG
post_excerpt: |
  Widget扩展用于根据页面需要输出不同内容，它在项目目录中的Lib/Widget下定义。
  定义：
  class NewsListWidget extends Widget{
  public function render($data){
  // code...
  }
  }
  注意：
  1）Widget是一个抽象类，其中有一个抽象方法（abstract）render，必须在子类中实现；
  2）Widget的render方法必须使用return返回，而不是直接输出；
  3）$data是传入Widget的参数。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/thinkphp-widget-expansion-detailed-in-the-thinkphp.html
published: true
post_date: 2012-12-27 09:48:50
---
<div>

Widget扩展用于根据页面需要输出不同内容，它在项目目录中的Lib/Widget下定义。

定义：
<div>
<div>class NewsListWidget extends Widget{
public function render($data){
// code...
}
}</div>
</div>
注意：
1）Widget是一个抽象类，其中有一个抽象方法（abstract）render，必须在子类中实现；
2）Widget的render方法必须使用return返回，而不是直接输出；
3）$data是传入Widget的参数。

&nbsp;

然后我们可以在模板中直接调用这个Widget
<div>
<div>{:W('NewsList', array('tmpl' =&gt; 'a'))}</div>
</div>
这里我传入了一个参数，这是比较常见的用法，Widget用来做什么？根据页面需要输出<strong>不同内容</strong>，这个不同内容，可以是数据不同，当然也可以是模板不同。
<div>
<div>class NewsListWidget extends Widget{
public function render($data){
// code
$news; // 这里可以是数据检索语句检索出来一个数据集
$html = $this-&gt;renderFile($data['tmpl'], $news);
return $html;
}
}</div>
</div>
这时候会自动渲染模板文件/Lib/Widget/NewsList/a.html的内容，并把$news传送过去，可以当普通模板文件处理，然后输出。

当然，还可以在Action控制器里面获取Widget的内容，进行二次加工。
<div>
<div>$content = W('NewsList', <a href="http://www.php.net/array">array</a>('tmpl' =&gt; 'a'), TRUE); // 第三个参数表示是否返回字符串，默认是FALSE，代表直接输出。</div>
</div>
另外，ThinkPHP是MVC框架，请大家把数据检索相关的内容放在Model层，我曾经看过一个朋友写的Widget…全是检索语句，维护起来多困难。

&nbsp;

本文摘自&lt;<a href=" http://jack.wilead.com/thinkphp-widget/" target="_blank">ThinkPHP中的Widget扩展详解</a>&gt;

</div>