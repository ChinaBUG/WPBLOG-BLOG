---
ID: 3389
post_title: >
  ShopNC二次开发研究日记9:2.4版本内的商品地区怎么修改与开发
author: ChinaBUG
post_excerpt: |
  今天乘清明假期回来，腰有点疼就来说说怎么修改ShopNC2.4版本的商品地区数据，让它按照你自己的需求来变换噢。
  注意：请安装火狐的firebug插件，然后开启。
  1、商品列表右边“所在地”修改
  2、所在地区列表的常用地区及省份的增加删除
  3、改完布局，我们还能修改什么呢？对~前台，商品发布有所在地选项怎么修改噢？
  4、前台所在地区只有二级（省市），怎样扩展为三级（省市区）？
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/shopnc-2dev-study-of-diary-9-how-to-modify-and-develop-the-product-area-within-version-2-4.html
published: true
post_date: 2014-04-08 11:43:25
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

最近ShopNC群里小白又开始泛滥了，就懂得整天在群里面喊什么怎么做，就是不懂得自己先研究一下分析一下，也不愿意自己狗狗一下或者问一下度娘，最鄙视这类人了，人笨不是你的错，可是还偷懒就不是玩意了~害的我们这些人也跟着受累，高手不出现呀~话就说到这，开始正文吧

最近去研究其他的了，都没啥时间研究ShopNC，其实，说真的ShopNC真的不需要太花时间研究的，因为啥都是写在一起的，除了是加密的之外，你只要有破解版的代码，你想怎么开发就怎么开发哈~难度中等

今天趁清明假期回来，腰有点疼就来说说怎么修改ShopNC2.4版本的商品地区数据，让它按照你自己的需求来变换噢。

<em><span style="text-decoration: underline;"><strong>注意：请安装火狐的firebug插件，然后开启。</strong></span></em>

1、商品列表右边“所在地”修改

可能有的人觉得这个“所在地”名字叫的不响亮？或者是样式不够豪华？

没事，改吧！

用火狐打开我们的ShopNC测试站，直接点击搜索，然后你会看到好多商品，在右边的就是有一个“所在地”，这个地方想要改掉的话，需要怎么做噢？大家请思考一下？
<blockquote><em>提示：ShopNC比较差，我个人觉得在于代码跟样式是没有真正的分离的，啥都是在一起，不过相应的，就是不用在多个文件之间跳来跳去。</em></blockquote>
要改掉这个区域的话，那么我们第一步就是需要找到这个功能所在模板的位置，那么很多朋友就是这个地方不懂的操作，其实很简单呀，为什么就会很多人会找不到呢？只能够说一声：懒！

今天我就教一下基础，希望有看到的朋友可以好好地学习噢。

我们要找到这个地方，那么请使用firebug找到“所在地”这个区域的html代码，下面是我查看到的代码噢：

[php]
&lt;div&gt;
&lt;div&gt;&lt;em nc_type=&amp;quot;area_name&amp;quot;&gt;所在地&lt;!-- 所在地 --&gt;&lt;/em&gt;&lt;/div&gt;
&lt;div&gt;&lt;a nc_type=&amp;quot;area_name&amp;quot;&gt;所在地&lt;!-- 所在地 --&gt;&lt;/a&gt;&lt;/div&gt;
&lt;i&gt;&lt;/i&gt;
&lt;ul&gt;
&lt;div id=&amp;quot;addressDraw&amp;quot;&gt;
......
&lt;/div&gt;&lt;/ul&gt;
&lt;/div&gt;
[/php]

看到了没有呢？明显写着所在地，后面还有注释呢~那么我们知道注释一般都是开发人员写上去的，那么这个肯定是不会变的！如果注释都是会随时变化的话，就没必要注释了，所以，我们直接就是找到关键词了，就是&lt;!-- 所在地 --&gt;这个注释的内容，注释的内容要复制完整噢，特别是尖括号内部的东西最好都不要修改，因为有可能这里面存在特殊符号，会影响搜索。

请用DW打开测试站，全站搜索这个关键词（&lt;!-- 所在地 --&gt;）然后你会发现好厉害噢，都出来了噢。我一共查找到8个记录（其实就是四个文件，一共八个地方有这个关键词），都是在templates\default\home里面，你呢？如果没出现文件那就自己去学一下DW的教程吧。
<blockquote><em>提示：建议大家使用DW的站点管理工具，这个一定是事半功倍的好东西噢。</em></blockquote>
我们随便打开一个文件，我这边打开的是templates\default\home\search.php（这个文件就是搜索时调用的模板）文件，然后DW就会自动定位到关键词所在，我们会看到如下代码：

[php]
&lt;div&gt;
&lt;div&gt;&lt;em nc_type=&amp;quot;area_name&amp;quot;&gt;&lt;?php echo $lang['goods_class_index_area']; ?&gt;&lt;!-- 所在地 --&gt;&lt;/em&gt;&lt;/div&gt;
&lt;div&gt;&lt;a nc_type=&amp;quot;area_name&amp;quot;&gt;&lt;?php echo $lang['goods_class_index_area']; ?&gt;&lt;!-- 所在地 --&gt;&lt;/a&gt;&lt;/div&gt;
&lt;i&gt;&lt;/i&gt;
&lt;ul&gt;&lt;?php require(BASE_TPL_PATH.'/home/goods_class_area_'.(strtolower(CHARSET)=='utf-8'?'utf-8':'gbk').'.html');?&gt;&lt;/ul&gt;
&lt;/div&gt;
[/php]

对比之前的代码我们能发现一件事情那就是很神奇的&lt;?php echo $lang['goods_class_index_area']; ?&gt;就是等于“所在地”三个字噢。

提示：这个$lang变量就是ShopNC的语言变量，他的作用很多，比如你需要之多多语言版本的商城，就是定义这个变量，那么这个表示定义了一个语言参数，这个的值就是“所在地”。

我们第一时间想，我要是只要修改“所在地”三个字的话，那么是不是就把这个变量删除换成我需要的或者将这个变量的值换成是我的不就可以了？

事实上，这个想法是正确的。

我们直接将&lt;?php echo $lang['goods_class_index_area']; ?&gt;这个删除掉，写上我们需要的文字，比如我们写成“生产地”，然后搜索一下，你会发现原先的“所在地”变成了“生产地”了，你要是没出现的话，那么请清空缓存吧，一般情况修改完是可以立马起作用的。

提示：ShopNC系统的缓存在后台清除，但是实践证明，后台的清空缓存的作用有限，就是有时候缓存清空了。你会发现还是照旧，怎么办？直接打开网站根目录下的cache目录，然后将里面的东西除session文件夹之外的东西删除即可。另外，如果发现删除还是没起作用的话，请重启LAMP环境噢或者其他的PHP环境。

修改起作用了，这个时候如果你的需求就是修改这个文字的话，你就会发现，你还有三个文件需要修改噢，不记得的话，请重新搜索一下关键词吧。

好在我们看到那些文档的格式都是一样的：

[php]
&lt;?php echo $lang['brand_index_area']; ?&gt;
&lt;?php echo $lang['goods_class_index_area']; ?&gt;
&lt;?php echo $lang['store_class_index_location'];?&gt;
[/php]

聪明的你一看就应该能反应到，这三个变量的值应该都等于“所在地”才对，那么以这些语言变量为关键词找一下，你会发现，在language文件夹内发现zh与zh_cn文件价内的相关文件内找到如下样的代码：

[php]
$lang['brand_index_area'] = '所在地';
$lang['goods_class_index_area']= '所在地';
$lang['store_class_index_location']= '所在地';
[/php]

一个个文件打开（其实只修改自己的语言版本的文件即可），将这个变量修改为“生产地”，然后前台查看，噢都改过来了噢。

文字改完了，很多人可能会不知足想要给他美化一下对吧，那么怎么修改噢？

<em>PS：如果什么都是靠我，那么你的需求一定是做不完的，因为我是技术，不是最终的需求者，我肯定不会引导你去做你需要的修改的。毕竟我不懂得你的需求。</em>

我们在上面已经找到“所在地”所在的位置了，那么他附近的代码自然就是样式控制的元素了，直接根据元素的样式在根据自己的需要直接调整即可。想要多豪华就有多豪华了。

2、所在地区列表的常用地区及省份的增加删除

再来说说这个怎么增加我们的常用地区噢，鼠标一上去我们就会看到一列表，那么我们从代码中分析，这个列表的代码就是在之前我们查阅的地方！阅读一下代码，对比一下，很快我们会发现其实这个在templates\default\home\search.php文件的位置就是：

[php]
&lt;div style=&amp;quot;background-image: none; float:right;&amp;quot;&gt;
&lt;div style=&amp;quot;margin:0;&amp;quot;&gt;
&lt;div&gt;&lt;em nc_type=&amp;quot;area_name&amp;quot;&gt;&lt;?php echo $lang['store_class_index_location'];?&gt;&lt;!-- 所在地 --&gt;&lt;/em&gt;&lt;/div&gt;
&lt;div&gt;&lt;a nc_type=&amp;quot;area_name&amp;quot;&gt;&lt;?php echo $lang['store_class_index_location'];?&gt;&lt;!-- 所在地 --&gt;&lt;/a&gt;&lt;/div&gt;
&lt;i&gt;&lt;/i&gt;
&lt;ul&gt;&lt;?php require(BASE_TPL_PATH.'/home/goods_class_area_'.(strtolower(CHARSET)=='utf-8'?'utf-8':'gbk').'.html');?&gt;&lt;/ul&gt;
&lt;/div&gt;
&lt;/div&gt;
[/php]

重点是&lt;?php require(BASE_TPL_PATH.'/home/goods_class_area_'.(strtolower(CHARSET)=='utf-8'?'utf-8':'gbk').'.html');?&gt;这句代码，他引入了一个文件，而这个文件自然就是最关键的了。。我们没打开这个文件之前，我们猜测，那些地区的信息应该在这个文件里面或者在里面调用生成的。那么我们根据这个路径打开这个文件。

根据你的设置我们一共可以获取到两个文件：goods_class_area_utf-8.html和goods_class_area_gbk.html这两文件，打开utf-8的，因为我是这个版本的。。你的话自行决定吧，反正都是差不多的。

果然，这个文件就是数据所在噢，我们看到的地区都在里面，那么你可以按照自己的需要调整布局，修改文字了，一级改为二级改为三级，都是任你修改噢，轻松加愉快~

3、改完布局，我们还能修改什么呢？对~前台，商品发布有所在地选项怎么修改噢？

这个根据之前的办法搜索，然后分析代码，很快我们就盯住一个文件：resource\js\area_array.js，这个脚本文件用DW打开你会发现里面都是地区的名字，很明显这个就是定义的所在。

nc_a[0]表示一级分类，如果你需要增加一级地区的话那么就在这个数组中找到相关的删除即可。

......

个人建议，没有经验的千万不要随便修改这个文件，要不出错了还会怨我教坏小孩哦~

4、前台所在地区只有二级（省市），怎样扩展为三级（省市区）？

这个需求其实说简单吧，挺简单的，说复杂吧，还真的有点复杂。

我们根据之前的查询可以知道生成省市二级选项的是在templates\default\member\store_goods_add_step2.php文件的第992行，在这边由省的选择框触发之后产生市级的选项，那么我们能不能增加一个区级的呢？这个时候需要看我们的技术啦（<em><span style="text-decoration: underline;">模仿、抄袭、修改的能力就体现在这个地方，但是这些能力的基础就是你需要对这些技术有点小研究，起码你要能看得懂代码是干嘛用的</span></em>），请看下面的代码：

[code lang="javascript"]
//2014.04.08 ChinaBUG
$(&amp;quot;#city_id&amp;quot;).live('change',function(){
// 删除后面的select
$(this).nextAll(&amp;quot;select&amp;quot;).remove();
if (this.value &gt; 0){
var text = $(this).get(0).options[$(this).get(0).selectedIndex].text;
var area_id = this.value;
if(typeof(nc_a[area_id]) != 'undefined'){//数组存在
var areas = new Array();
var option = &amp;quot;&amp;quot;;
areas = nc_a[area_id];
$(&amp;quot;&lt;select name='area_id' id='area_id'&gt;&amp;quot;+option+&amp;quot;&lt;/select&gt;&amp;quot;).insertAfter(this);
for (i = 0; i &lt;areas.length; i++){
$(this).next(&amp;quot;select&amp;quot;).append(&amp;quot;
&lt;option value='&amp;quot; + areas[i][0] + &amp;quot;'&gt;&amp;quot; + areas[i][1] + &amp;quot;&lt;/option&gt;&amp;quot;);
}
}
}
});
[/code]

上面的代码就是在城市后面增加一个输入框，里面的数据就是每个城市的区数据。只要加在provinceChange方法后面即可。
<blockquote><em>提示：如果你不知道怎么添加，我个人建议你还是去学学基础，真的基础很关键，没有那个人有那个闲工夫给你普及基础的。</em></blockquote>
测试一下，完美呀~区级选项出现了噢。

这个时候你要是点击保存，你会发现......哈~~怎么保存不了呢~~^_^

来解决这个问题吧。我们先从地址栏找到控制器及方法：index.php?act=store_goods&amp;op=save_goods，然后我打开control\store_goods.php找到save_goodsOp方法，然后一步步看下来，这边会告诉你怎么调试的，请自己按照步骤一步步的自己分析一下噢！

先通读一下整个方法的内容，然后找到

[php]
$goods_array['city_id']      = intval($_POST['city_id']);
$goods_array['province_id']  = intval($_POST['province_id']);
[/php]

这两个就是保存省市的字段，我们就去数据库里面查看shopnc_goods表是不是有这两个字段，一看，哈，真的存在噢，那么我们就参照这个新建一个字段，名字就是我上面弄得那个名字area_id，请直接参照省市的字段设置噢，或者使用phpmyadmin直接执行下面的SQL语句：

[code lang="sql"]
ALTER TABLE `shopnc_goods` ADD `area_id` MEDIUMINT( 8 ) UNSIGNED NULL DEFAULT '0' AFTER `province_id`
[/code]

然后再回过头来，数据库已经有这个字段了，那么就需要有数据填充的操作了，我们回到上面的代码中，增加一个获取的操作哈~

[php]
$goods_array['city_id']     = intval($_POST['city_id']);
$goods_array['province_id'] = intval($_POST['province_id']);
$goods_array['area_id']     = intval($_POST['area_id']);
[/php]

基本的都弄好了，如果这两种我们都没有弄得话，那么出错是活该出错！

我满怀希望的再次执行，然后......用phpmyadmin查看发现area_id的值还是为0噢，怎么回事呢？

好吧，下面就是我一般使用的调试方法，我会一步步的说，希望大家可以参考一下分析调试过程，个人觉得这个方法挺笨的，不过就懂这个办法，其他办法不懂，有懂的人可以教教我呗。

我们刚才通读了save_goodsOp方法看到有一个：

[php]
$model_store_goods    = Model('goods');
[/php]

这个是实例化店铺商品模型，那么我先输出它的内容看看有什么比较神奇的内容没~

[php]
$model_store_goods    = Model('goods');
print_r($model_store_goods);
exit;
[/php]

然后我们再添加一个商品试试看输出什么内容来噢~~看到的是一般的信息，没啥帮助，那么我们可能会想会不会是数据没提交？好吧。找到下面的代码：

[php]
$state = $model_store_goods-&gt;saveGoods($goods_array);
[/php]

修改为

[php]
print_r($goods_array);
exit;
$state = $model_store_goods-&gt;saveGoods($goods_array);
[/php]

然后刷新刚才的提交页面，查看一下输出什么内容了~我们会看到这次输出的就是我们提交的内容，那么重点查看我们需要的地区变量，如下：

[php]
[city_id] =&gt; 204
[province_id] =&gt; 13
[area_id] =&gt; 2342
[/php]

居然都是有值的，那么我们可以很明显的猜测到，这个是正确的，那么数据没有提交完成。就是商品模型保存数据出现问题了，因为我们这次的截取断点是在saveGoods方法之前截取的。

接着我们只能是到saveGoods方法里面去分析了，注释掉上面的输出代码，然后我们打开model\goods.model.php文件，找到saveGoods方法然后，你就会很明显的看到原来，他里面还做了二次的验证，我们这边没有添加我们的数据哈。看下面代码：

[php]
......
$goods_array['city_id']     = $param['city_id'];
$goods_array['province_id'] = $param['province_id'];
[/php]

增加一个我们的字段名吧。最后代码为：

[php]
......
$goods_array['city_id']     = $param['city_id'];
$goods_array['province_id'] = $param['province_id'];
$goods_array['area_id']     = $param['area_id'];
[/php]

然后提交，哈~用phpmyadmin检查一下，呵呵，字段city_id、province_id、area_id都有值了噢~这个调试完毕。

点击编辑商品，然后我们查看一下所在地，发现，这边只显示出省市二级地区，难道不能显示省市区三级地区吗？

当然可以的，不可以我写什么呢对吧。

从地址栏上获取操作的方法：index.php?act=store_goods&amp;op=edit_goods&amp;goods_id=89

我们在控制器store_goods里面找edit_goods方法，然后阅读一下这个方法会发现，里面没有涉及到单独的字段的操作，所以我们暂时可以当做，这个字段是系统模型自动获取显示出来的，那么现在剩下的问题就是将这个值显示出来了噢。

分析edit_goods方法我们可以知道这个方法调用的是member\store_goods_add_step2.php这个模板，然后我们一行行的看下去，你会发现一个地方很可疑

[php]
&lt;!--?php if ($output['goods']['province_id'] --&gt; 0){?&gt;
$('#province_id').val('&lt;!--?php echo $output['goods']['province_id'];?--&gt;');
$('#province_id').change();
$('#city_id').val('&lt;!--?php echo $output['goods']['city_id'];?--&gt;');
&lt;!--?php }?--&gt;
[/php]

这个代码的含义是只要省份有值则触发省地区的事件。那么，地区的触发在哪里？没有！所以我们的地区自然就不会显示了，所以很简单的我们就是修改代码，让他可疑触发区地区的事件，代码改为下面的：

[php]
&lt;!--?php if ($output['goods']['province_id'] --&gt; 0){?&gt;
$('#province_id').val('&lt;!--?php echo $output['goods']['province_id'];?--&gt;');
$('#province_id').change();
$('#city_id').val('&lt;!--?php echo $output['goods']['city_id'];?--&gt;');
$('#city_id').change();
$('#area_id').val('&lt;!--?php echo $output['goods']['area_id'];?--&gt;');
&lt;!--?php }?--&gt;
[/php]

刷新，然后你会看到我们的区选项出现了噢。

随便选择一个区域，然后你就会发现居然发生错误了！！

好吧，这个系统真是神奇~

出现下面的提示噢

[php]
Warning: Invalid argument supplied for foreach() in E:\PHPnow\127.0.0.4_ShopNC\framework\db\mysql.php on line 427
Db Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 1
[/php]

注意：一般错误提示都会告诉你在第几行，你只要按照提示去找到这个位置，然后根据这个位置来进行调试即可。

老办法，从地址栏获取处理的控制器跟方法：index.php?act=store_goods&amp;op=edit_save_goods

找到控制器store_goods里面的edit_save_goods方法，然后请通读一遍

读完之后，因为我们之前的分析是差不多的，所以在这个方法中我们有经验了，不用一个个的尝试，直接大胆的假设是模型操作错误，因为根据ShopNC的系统提醒：

Db Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '' at line 1

我们知道生成的SQL肯定有错误！

所以很容易就怀疑到是不是模型里面生成的SQl有问题呢？！

我们打开model\goods.model.php然后找到updateGoods方法，然后你会发现这个方法好简单噢，好吧，直接查看Db::update这个方法吧。

打开framework\db\mysql.php然后找到update方法，在方法开头直接写入

[php]
print_r($update_array);
exit;
[/php]

然后刷新刚才出现错误的页面，然后你会看到提交的信息。

检查一下，看起来很正常~

然后再找到

[php]
$sql = &quot;UPDATE `&quot;.DBPRE.$table_name.&quot;` AS `&quot;.$table_name.&quot;` SET &quot;.$string_value.&quot; &quot;.$where;
[/php]

改为

[php]
print_r($string_value);
exit;
$sql = &quot;UPDATE `&quot;.DBPRE.$table_name.&quot;` AS `&quot;.$table_name.&quot;` SET &quot;.$string_value.&quot; &quot;.$where;
[/php]

然后刷新一下看看提示吧

你会发现这个是一串的字符串是SQL更新语句的内容，那么初略看起来是没有错的，但是实际呢？

我们现在在把断点弄到下面来看看

[php]
$sql = &quot;UPDATE `&quot;.DBPRE.$table_name.&quot;` AS `&quot;.$table_name.&quot;` SET &quot;.$string_value.&quot; &quot;.$where;
print_r($sql);
exit;
[/php]

刷新吧~复制输出的SQL语句，然后打开phpmyadmin然后执行SQL语句。

然后，成功了？！

证明这个SQL语句是没有错的，那么继续跟踪吧~

根据下面的调用方法，我们找到了query方法，老办法，先下断点吧。。。然后你会发现这个方法内根本就没有错误！

这个是在我局域网内的服务器上调试出现的错误信息，结果找不到原因！然后我把程序放到我本地机子上，然后他就出现了上面的Warning提示，根据提示查找发现getCount方法内的

[php]if ( !empty( $condition ) || is_array( $condition ) )[/php]

这个逻辑是或者的关系，但是下断点调试时发现这个条件$condition是一个字符串，不是数组，所以。。。逻辑错误哈~~

把代码改为

[php]if ( !empty( $condition ) &amp;&amp; is_array( $condition ) )[/php]

然后问题解决。

造成这个原因的是因为我直接使用破解的版本的内核框架，结果破解的代码因为没有严谨的测试，所以就出现这种很奇怪的逻辑，这个是第二次了噢，所以在这边建议大家破解的程序只能当做参考而不能当做直接运行的代码，要不出现问题，调试都会很麻烦噢。