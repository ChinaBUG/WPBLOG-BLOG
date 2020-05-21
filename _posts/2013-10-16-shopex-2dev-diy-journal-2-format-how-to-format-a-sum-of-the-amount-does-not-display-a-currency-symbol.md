---
ID: 2610
post_title: >
  ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  ShopEX在使用的时候，我们在设计模板时可以限定输出的金额格式（变量后面带上“|cur”），主要就是小数点几位啦，顺便带上货币符号哈，那么有时候就会出现只需要格式化金额，但是不带上货币符号这个效果，怎么办？
  DIY日记就是要动手做的啦~
  我们找到core\include_v5\smartyplugins\compile_modifier.cur.php这个文件，当然如果你的PHP版本低于5.0的话，那么请找到core\include\smartyplugins\compile_modifier.cur.php这个文件。
  打开之后你会发现........这是个乱码，好吧，我忘记你没有破解噢。没事，下面就是破解的代码噢。
  <?php
  function tpl_compile_modifier_cur($attrs,&$compile) {
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-2-format-how-to-format-a-sum-of-the-amount-does-not-display-a-currency-symbol.html
published: true
post_date: 2013-10-16 12:21:25
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

ShopEX在使用的时候，我们在设计模板时可以限定输出的金额格式（变量后面带上“|cur”），主要就是小数点几位啦，顺便带上货币符号哈，那么有时候就会出现只需要格式化金额，但是不带上货币符号这个效果，怎么办？
<blockquote><strong><span style="text-decoration: underline;">名词解释：</span></strong>
修饰器：<a title="Chapter 5. 变量修饰器" href="http://www.smarty.net/docs/zh_CN/language.modifiers.tpl">修饰器</a>是一些小函数， 它们会对模板中的变量，在显示之前或使用前进行处理。 修饰器可以连用。</blockquote>
DIY日记就是要动手做的啦~

我们找到core\include_v5\smartyplugins\compile_modifier.cur.php这个文件，当然如果你的PHP版本低于5.0的话，那么请找到core\include\smartyplugins\compile_modifier.cur.php这个文件。

打开之后你会发现........这是个乱码，好吧，我忘记你没有破解噢。没事，下面就是破解的代码噢。

&lt;?php
function tpl_compile_modifier_cur($attrs,&amp;$compile) {
//todo 需要将货币汇率也缓存
if(!strpos($attrs,',') || false!==strpos($attrs,',')){
$compile-&gt;_head_stack['$CURRENCY = &amp;$this-&gt;system-&gt;loadModel(\'system/cur\')']=1;
return $attrs = '$CURRENCY-&gt;changer('.$attrs.')';
}
}
?&gt;

按照我们的思路，既然每个变量之后增加一个“|cur”就能自动话格式金额，那么我们是不是只要新建一个cur2然后它就可以自动格式化金额了呢？

请将代码修改为如下

&lt;?php
function tpl_compile_modifier_cur2($attrs,&amp;$compile) {
//todo 需要将货币汇率也缓存
if(!strpos($attrs,',') || false!==strpos($attrs,',')){
$compile-&gt;_head_stack['$CURRENCY = &amp;$this-&gt;system-&gt;loadModel(\'system/cur\')']=1;
return $attrs = '$CURRENCY-&gt;changer('.$attrs.')';
}
}
?&gt;

注意，上面的代码中我们在方法名上做了修改了噢，加了一个2，另存为core\include_v5\smartyplugins\compile_modifier.cur2.php文件，然后我们在需要使用的模板中定义一下&lt;{$xxxxxx|cur2}&gt;这样的形式噢，执行。

然后我们会发现......系统提示错误了！

提示找不到cur2这个方法，找不到方法证明系统没有调用到这个文件里面的方法，那么我们肯定会想到一定有什么文件在控制这个调用。

经过查找跟踪pageFactory.php文档的执行，我发现原来有一个数组很重要，就是位于core\include_v5\template\compile.plugin_path.php内的$path数组，我们在里面会看到'compile_modifier.cur.php' =&gt; 1,一行。

聪明的小伙伴们一定想到了，我们在这行后面增加一行

'compile_modifier.cur2.php' =&gt; 1,

吧，保存，刷新~噢，终于执行正常效果出来了，可是这个效果还是有带货币符号的噢。

<span style="text-decoration: underline;">虫曰：一般做修改的话，我都是求稳的先测试原先的功能是否可以用，可以用的话才继续做我们的修改，这样避免我们不懂的是我们的代码问题还是系统不识别问题，造成调试的麻烦。</span>

怎么修改呢？我们的思路很简单，在这边就是直接做一下处理，把数据返回呗。有更好的办法吗？

有！我们发现上面的代码中返回一个方法changer函数，那么这个干嘛用的？明显就是格式化金额的主要方法，那么聪明的小伙伴们肯定会知道偷懒啦，想知道这个方法是不是提供我们需要的功能，可以实现货币符号的不输出对吧？！

我们找一下system/cur这个对应的模块，在core\model_v5\system\mdl.cur.php文件中找到changer方法，同样的，如果PHP版本低于5.0的话，请找到core\model\system\mdl.cur.php文件的相关方法。

changer方法的代码定义如下：

/**
* 商店币别金额显示统一调用函数
* @param float $money 金额
* @param string $currency 显示的货币种类如：CNY/USD
* @param bool $is_sign 是否不带货币符号显示
* @param bool $is_rate 是否按汇率显示
* @return bool
*/
function changer($money, $currency='', $is_sign=false, $is_rate=false)
{
......
}

细心的小伙伴们眼睛为之一亮，上面参数$is_sign的说明哈~~“是否不带货币符号显示”，嗯，就是我们需要的。然后动手吧。

我们把tpl_compile_modifier_cur2里面的'$CURRENCY-&gt;changer('.$attrs.')'调用改为'$CURRENCY-&gt;changer('.$attrs.',\'\',true)';保存，执行，哈哈~~~格式化不带符号的效果出来了噢。

完整代码如下：

&lt;?php
/*
2013.10.16
修改为只输出金额数，而不输出符号
*/
function tpl_compile_modifier_cur2($attrs,&amp;$compile) {
if(!strpos($attrs,',') || false!==strpos($attrs,',')){
$compile-&gt;_head_stack['$CURRENCY = &amp;$this-&gt;system-&gt;loadModel(\'system/cur\')']=1;
return $attrs = '$CURRENCY-&gt;changer('.$attrs.',\'\',true)';
}
}
?&gt;

你做到了嘛？！

当然，也许有纠结的小伙伴们会说，我想要直接修改，还要修改两个文件，太麻烦！

好吧~

其实，这个修改我不会~~不懂得怎么修改噢。目前还在研究之中。你要是知道怎么修改的话。欢迎你留言交流。

......经过昨天跟早上的拼命的跟踪分析终于解决问题了~~原来，挺简单的哈~~~代码如下：

&lt;?php
/*
2013.10.17
修改为只输出金额数，而不输出符号
*/
function tpl_compile_modifier_cur2($attrs,&amp;$compile) {
$compile-&gt;_head_stack['function cur2($string){return number_format(trim($string),2, \'.\', \'\');}']=1;
return 'cur2('.$attrs.')';
}
?&gt;

保存，上传到core\include_v5\smartyplugins\compile_modifier.cur2.php就可以了噢。下面来分析一下这个代码的含义噢。

首先，$compile-&gt;_head_stack是在模板缓存文件里面的文件头包含我们想要的内容，这边的意思是包含一个自定义方法cur2，这个专门用来转换我们的金额的。具体如下：

function cur2($string){
return number_format(trim($string),2, '.', '');
};

这个方法就是借助PHP本身的函数number_format来格式化金额，具体参数含义，请查看官方噢<a href="http://www.php.net/manual/zh/function.number-format.php" target="_blank">PHP: <em>number_format</em> - Manual</a>

return 'cur2('.$attrs.')';这行就是输出的PHP代码为&lt;?php echo cur2($this-&gt;_vars['ol']); ?&gt;这样的形式。

好了，终于完成这个简单的工作了，代码简单，可是整个流程好复杂哈~~

PS：如果对我如何跟踪查到这个调用方法的人，可以访问我即将写的&lt;&lt;<a title="ShopEX-二次开发DIY日记3之如何跟踪模板的缓存代码" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-3-how-to-trace-cache-code-template/">ShopEX-二次开发DIY日记3之如何跟踪模板的缓存代码</a>&gt;&gt;哈~~