---
ID: 3539
post_title: >
  ShopEX二次开发DIY日记18之挂件goods_show的商品自定义排序
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/07/shopex-development-diy-diary-18-pendant-of-goods-goods-show-a-custom-sort.html
published: true
post_date: 2014-07-24 14:38:34
---
今天，老大交代要把goods_show挂件输出的商品按照指定的顺序排序，有经验的人都是知道goods_show挂件使用的是SQL直接获取商品信息，然后按照默认的几个排序选项做排序，根本就没有选项让我们按照自己指定的顺序来排序，怎么办？难道一定要挖一个个的坑，一个挂件一个挂件的调用？

[su_quote]提示：SQL内一般的情况都是按照字段名做顺序排序或者是逆序排序的，就是没有按照我们指定的顺序排序，那么怎么办呢？有需要了解更深的朋友可以查看本博客的另外一篇文章，专门解答这个问题噢，传送门：<a title="永久链接到：SQL-MySQL开发常用的语句参考" href="http://blog.ipodmp.com/archives/sql-mysql-to-develop-a-common-statement-reference/" rel="bookmark">SQL-MySQL开发常用的语句参考：2、在MySQL之中怎么按照in语句的参数顺序来排序呢？</a>[/su_quote]

从上面的参考网站我们知道要按照我们指定的顺序来排序的话，需要指定order by参数噢，那么我们应该怎样修改goods_show挂件呢？

打开goods_show挂件的\plugins\widgets\goods_show\widget_goods_show.php文件我们会看到

[php]$result=$o-&gt;getList(null,$filter,0,$limit,$orderby['sql']);[/php]

我就猜想$orderby['sql']就是控制排序的参数，然后输出一看，哈还真的是，他的格式就是一个字符串，如price desc就是价格从高到低排序。

根据<a title="永久链接到：SQL-MySQL开发常用的语句参考" href="http://blog.ipodmp.com/archives/sql-mysql-to-develop-a-common-statement-reference/" rel="bookmark">SQL-MySQL开发常用的语句参考</a>一文我们知道只需要将order by语句后面的参数修改为：

[php]instr(\','.$setting['sort_custom'].',\', CONCAT(\',\', goods_id,\',\'))[/php]

就可以了呗，$setting['sort_custom']就是我们在挂件配置文件内设定的参数。

完整的代码如下：

[php]//2014.07.24 自定义排序
if($setting['sort_custom']){
$orderby['sql'] = 'instr(\','.$setting['sort_custom'].',\', CONCAT(\',\', goods_id,\',\'))';
}
$result=$o-&gt;getList(null,$filter,0,$limit,$orderby['sql']);[/php]

保存，再修改一下_config.html文件：

[code lang="html"]      &lt;tr&gt;
&lt;th&gt;显示商品数：&lt;/th&gt;
&lt;td&gt;&lt;{input name=&quot;limit&quot; value=$setting.limit|default:'4' required=&quot;true&quot; type=&quot;digits&quot; class='inputstyle'}&gt;&lt;/td&gt;
&lt;th&gt;每行显示商品数：&lt;/th&gt;
&lt;td&gt;&lt;{input name=&quot;column&quot; value=$setting.column|default:'4' required=&quot;true&quot; type=&quot;digits&quot; class='inputstyle'}&gt;&lt;/td&gt;
&lt;/tr&gt;
&lt;tr&gt;
&lt;th&gt;排序设定：&lt;/th&gt;
&lt;td colspan=&quot;3&quot;&gt;&lt;select name=&quot;goods_orderby&quot;&gt;
&lt;{foreach from=$data item=orderby key=key}&gt;
&lt;option value=&quot;&lt;{$key}&gt;&quot; &lt;{if $setting.goods_orderby==$key}&gt;selected&lt;{/if}&gt;&gt;&lt;{$orderby.label}&gt;&lt;/option&gt;
&lt;{/foreach}&gt;
&lt;/select&gt;
&lt;!--2014.07.24 增加排序--&gt;
&lt;{input name=&quot;sort_custom&quot; class='inputstyle' value=$setting.sort_custom|default:'' style=&quot;width:200px;&quot;}&gt;*指定排序后，左边设定无效
&lt;/td&gt;
&lt;/tr&gt;[/code]

保存，运行一下然后根据需要的将商品的编码输入到这个sort_custom的文字框内，然后就会看到商品已经按照我们的需要排序了噢。

这个修改说的唯一问题就是，必须有多少个商品就要都指定顺序，要不就会按照默认的来排序，而指定的宝贝才会按照顺序排序，但是是在前面还是在后面就是根据默认排序来定了。有想法的朋友可以自己做一下修改，把这个改为需要排序的指定，不需要排序的则自动生成参数排在指定的序列后面，这样子比较完美噢。