---
ID: 2628
post_title: >
  ShopEX二次开发DIY日记3之如何跟踪模板的缓存代码(front_tmpl与admin_tmpl的作用)
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  在上一篇ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？中，我们对ShopEX系统的底层的修饰器做了一个增加，请看最后的修改的代码：
  <?php
  /*
  2013.10.17
  修改为只输出金额数，而不输出符号
  */
  function tpl_compile_modifier_cur2($attrs,&$compile) {
  $compile->_head_stack['function cur2($string){return number_format(trim($string),2, \'.\', \'\');}']=1;
  return ‘cur2(‘.$attrs.’)';
  }
  ?>
  在上面代码之后，很多朋友可能会问为什么我知道需要return ‘cur2(‘.$attrs.’)';这个值，这个返回值需要这样写？
  其实我也想知道，我在分析缓存代码之前我对这个也不懂，顶多是根据原有的方法做修改哈，但是，对缓存代码作分析之后我就懂了，今天，你学了你也会了~^_^
  现在，请跟我来，先打开ShopEX目录的home\cache目录，你会看到三个文件，两个文件夹，分别是：cachedata.stat.php、cachedata.php、cache.status、front_tmpl、admin_tmpl。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-3-how-to-trace-cache-code-template.html
published: true
post_date: 2013-10-17 15:46:06
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

在上一篇<a title="ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-2-format-how-to-format-a-sum-of-the-amount-does-not-display-a-currency-symbol/">ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？</a>中，我们对ShopEX系统的底层的修饰器做了一个增加，请看最后的修改的代码：

&lt;?php
/*
2013.10.17
修改为只输出金额数，而不输出符号
*/
function tpl_compile_modifier_cur2($attrs,&amp;$compile) {
$compile-&gt;_head_stack['function cur2($string){return number_format(trim($string),2, \'.\', \'\');}']=1;
return ‘cur2(‘.$attrs.’)';
}
?&gt;

在上面代码之后，很多朋友可能会问为什么我知道需要return ‘cur2(‘.$attrs.’)';这个值，这个返回值需要这样写？

其实我也想知道，我在分析缓存代码之前我对这个也不懂，顶多是根据原有的方法做修改哈，但是，对缓存代码作分析之后我就懂了，今天，你学了你也会了~^_^

现在，请跟我来，先打开ShopEX目录的home\cache目录，你会看到三个文件，两个文件夹，分别是：cachedata.stat.php、cachedata.php、cache.status、front_tmpl、admin_tmpl。

如果你发现你没有这几个文件的话，那么请检查config/config.php内的

define('WITHOUT_CACHE',false);

是不是跟我上面的值是一致的，如果是true的话，那么这个文件夹内的文件跟文件夹可能被你删除了，那么你需要设置为false噢，就是上面的代码！

然后一定要新建front_tmpl、admin_tmpl两个文件夹噢，要不系统是不会生成缓存文件的噢。

上面的设置一切正常的话，请运行一下ShopEX商城，就打开主页即可，然后你到front_tmpl文件夹内，你就会看到生成好多的PHP文件，文件名像

3c57539b1018b6aad10d539e89ebc593default.html-zh_CN.php

这样的形式。

我们尝试打开一个文件，你会发现其实就是一个PHP文件~里面由PHP代码与HTML代码组成的。

既然是PHP文件，那么可不可以直接运行呢？

我们来试试，在浏览器地址栏内输入：

http://127.0.0.1:12580/home/cache/front_tmpl/3c57539b1018b6aad10d539e89ebc593default.html-zh_CN.php

然后~~~

出现了错误啦！这个证明里面的代码是可以运行的（出错提示都是缺少文件的，你要是补充好，自然就可以运行了噢）。

写到现在，各位同学明白了什么没有？

我们的模板定义之后，ShopEX系统会自动将我们的模板转换为这种PHP+HTML内容的PHP文件，然后再需要的时候调用并将结果显示出来。

简单的说，这个文件就是最终的PHP代码，你修改里面的内容的话，最后执行的结果就是你修改的内容了。

基础说完，再来说说怎么找到我们需要的文件噢。

PS：在这边需要提醒的是，最好不要根据前台的缓存文件来分析代码，因为前台的缓存代码，一个页面内可能包含很多歌的文件，不利于迅速找到我们需要的文件，就如index.php执行之后将会生成一片的文件，那些文件的名字除了MD5加密串不同之外，后面都是一样的，只是文件的内容不一样，要在这样的情况下迅速找到我们需要的文件基本上是不可能的哈。怎么解决噢？我一般是使用后台的APP程序来做测试哈~这样子生成的代码就比较简单，逻辑不复杂了。

调试光有热情还不够还是需要方法方式的噢！

就拿<a title="ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-2-format-how-to-format-a-sum-of-the-amount-does-not-display-a-currency-symbol/">DIY日记2</a>中的案例来说明吧，我们先打开plugins\app\shopex_stat\view\index.html文件，然后在顶部录入下面代码：

测试&lt;{$ceshi}&gt;&lt;br/&gt;

保存，然后请清空home\cache\front_tmpl文件夹内的全部文件，都删除了噢。

然后进入后台找到“统计报表》查看分析报表”点击执行，你会看到最顶上出现了“测试”两个字了。

那么，请再一次清空front_tmpl文件夹，这个步骤在我们分析的时候要一直操作！记住了噢，下面的说到“清空缓存”则是删除front_tmpl、admin_tmpl内的文件噢。

然后再次执行“统计报表》查看分析报表”，然后转到front_tmpl文件夹中，你会发现多出一个文件名字中带有index.html.php字眼的文件，直接打开（因为此时的缓存都是后台这个页面需要调用到的文件生成的，数量比较少，我测试时就生成两个文件）。

文件内的代码变成如下形式：
<blockquote>测试&lt;?php echo $this-&gt;_vars['ceshi']; ?&gt;&lt;br/&gt; &lt;iframe id="iframe1" frameborder="0" style="width: 100%; height: 100%;" src="&lt;?php echo $this-&gt;_vars['shoex_stat_webUrl']; ?&gt;"&gt;&lt;/iframe&gt;</blockquote>
跟你的模板文件上写的对比一下看看
<blockquote>测试&lt;{$ceshi}&gt;&lt;br/&gt;
&lt;iframe id="iframe1" frameborder="0" style="width: 100%; height: 100%;" src="&lt;{$shoex_stat_webUrl}&gt;"&gt;&lt;/iframe&gt;</blockquote>
会发现我们的变量都被系统给转换了噢。而HTML代码则不变。

来吧，我们再进一步测试一下。将上面的代码修改为：

测试&lt;{$ceshi|cur}&gt;&lt;br/&gt;

清空缓存一下，再次执行“查看分析报表”，然后你会看到原先测试的后面变成了“测试￥0.00”字样了！

请打开缓存文件查看一下代码（代码本身是在一行的，我给他分行了噢）：
<blockquote>&lt;?php $CURRENCY = &amp;$this-&gt;system-&gt;loadModel('system/cur'); ?&gt;
测试&lt;?php echo $CURRENCY-&gt;changer($this-&gt;_vars['ceshi']); ?&gt;&lt;br/&gt; &lt;iframe id="iframe1" frameborder="0" style="width: 100%; height: 100%;" src="&lt;?php echo $this-&gt;_vars['shoex_stat_webUrl']; ?&gt;"&gt;&lt;/iframe&gt;</blockquote>
我们会发现，比之前多了点东西了噢。首先，增加一个模块调用，第二增加一个方法调用。

看到这个是不是觉得很眼熟噢，在<a title="ShopEX二次开发DIY日记2之前台模板金额的格式如何格式化金额不显示货币符号？" href="http://blog.ipodmp.com/archives/shopex-2dev-diy-journal-2-format-how-to-format-a-sum-of-the-amount-does-not-display-a-currency-symbol/">DIY日记2</a>中，那个cur修饰器最后返回的是什么呢？！原来在这边哈~~

那模块调用是怎么出现的呢？现在知道$compile-&gt;_head_stack数组的用途了吧？！

剩下的就是自己分析了噢。照做总会把~~^_^