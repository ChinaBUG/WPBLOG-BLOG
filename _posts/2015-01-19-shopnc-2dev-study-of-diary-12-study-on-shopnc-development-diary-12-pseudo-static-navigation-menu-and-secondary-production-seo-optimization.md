---
ID: 3889
post_title: >
  ShopNC二次开发研究日记12:导航菜单伪静态及二级目录制作SEO优化
author: ChinaBUG
post_excerpt: |
  序
  话说，很久没有来写ShopNC开发的内容了，正好刚忙完推广部同事的一个需求就拿来分享一下开发的过程及如何解决的吧。
  同事的需求是：
  ShopNC每个分类都是以ID的形式存在，如/index.php?act=search&cate_id=621这样子的形式，这样子对SEO不友好^_^想要友好一点的方式，比如可以让人访问/sxgr/sg/pg这样的形式来访问分类“生鲜果蔬\水果\苹果”。
  简单吧，一看感觉好简单，其实好费时，忙了我好几天才折腾完毕。
  闲话不说了，就分享一下我怎么解决这个需求的吧，还是老规矩废话一点，慢慢分析我的解决思路噢。
  正文
  要想实现这样子的方式，不用说肯定是需要伪静态支持的，然后需要写相关的重写规则来处理了，除了写规则之外就是需要修改程序，让程序输出我们需要的格式了。
  ......
layout: post
permalink: >
  http://blog.ipodmp.com/2015/01/shopnc-2dev-study-of-diary-12-study-on-shopnc-development-diary-12-pseudo-static-navigation-menu-and-secondary-production-seo-optimization.html
published: true
post_date: 2015-01-19 16:54:03
---
序

话说，很久没有来写ShopNC开发的内容了，正好刚忙完推广部同事的一个需求就拿来分享一下开发的过程及如何解决的吧。
<blockquote>同事的需求是：
ShopNC每个分类都是以ID的形式存在，如/index.php?act=search&amp;cate_id=621这样子的形式，这样子对SEO不友好^_^想要友好一点的方式，比如可以让人访问/sxgr/sg/pg这样的形式来访问分类“生鲜果蔬\水果\苹果”。</blockquote>
简单吧，一看感觉好简单，其实好费时，忙了我好几天才折腾完毕。

闲话不说了，就分享一下我怎么解决这个需求的吧，还是老规矩废话一点，慢慢分析我的解决思路噢。

正文

要想实现这样子的方式，不用说肯定是需要伪静态支持的，然后需要写相关的重写规则来处理了，除了写规则之外就是需要修改程序，让程序输出我们需要的格式了。

第一步：伪静态规则的编写

我们期望的访问路径是：

http://www.ipodmp.com/sxgr/sg/pg/index.html

这样子的形式，那么一般我们的规则都是从sxgr/sg/pg/index.html开始做匹配的。
<blockquote>为啥？
因为我们在写htaccess的时候基本上最前面都会作一个设定：
<em>RewriteBase /</em>
注：如果你没有看到这行的话，那么请加上，要不跟着做可能有错误。具体是：
<em>RewriteEngine On</em>
<em>RewriteBase /</em></blockquote>
记得一个规则，字母+数字+特定符号用\w表示（或者[a-zA-Z0-9_]）；数字用\d表示（或者[0-9]）；^代表的是行开头；$代表行结束。

所以我们就想到我们的匹配规则应该是这样子写的：

^(\w*)/(\w*)/(\w*)/index.html$

完整的规则是：

RewriteRule ^(\w*)/(\w*)/(\w*)/index.html$ /index.php?act=search&amp;cate_id=$3

其中很多人会问为什么是$3这个是啥东西？其实只要看到$开头的变量都是代表规则的选择区域，这行的意思就是将第三个括号中代表的内容设置为cate_id的值。
<blockquote>为什么是$N是由1开始的，不是从0开始的吗，最大可以设置多少个？
这个有点编程基础的人有时就会搞混，其实正则表达式中就是从1开始的。最大可以保存99个，这个是正则中的，具体htaccess可以保存多少个，还真的没有测试，但是可以保证的是10个肯定是可以的。^_^
有的人会迷茫，要怎么计算这个区域？这个区域学名叫作“反向引用”，凡是你在正则表达式中看到括号括起来的，从左边到右边依次是$1、$2、$.........</blockquote>
现在来测试一下，我们的规则写的对不对噢。

请打开\control\search.php搜索

public function indexOp(){

然后修改代码为下面代码

public function indexOp(){
print_r($_GET);
exit;

然后我们打开网站，就会发现主页访问正常噢，这个是明显的，因为我们没有修改主页，请点击分类菜单，随便点一个，然后你就会看到，输出一些所谓的乱码了。

现在按照我说的来测试噢。要不，没办法确定你的结果是不是跟我一样噢。

http://localhost/index.php?act=search

将上面的http://localhost/换成你的地址，一般不用改或者127.0.0.1就可以了。

打开之后你会看到一些提示，查看源代码，就会看到下面的字样：

Array
(
[act] =&gt; search
[op] =&gt; index
)

如果你出现的是其它的东西，那么就自行检查你的服务器吧。这个提示其实就是告诉你目前你运行的是search控制器的index方法，也就是上面我们添加代码的方法了。

关于控制器C，模块M，视图V的概念，请自行查找MVC阅读吧。

上面的测试仅仅是证明我们的ShopNC是安装成功，可以使用的，现在来测试我们的规则对错噢。

请打开下面的网址：

http://localhost/sxgr/sg/pg/index.html

然后，你会发现，提示变化了~变成下面的样子了：
Array
(
[act] =&gt; search
[cate_id] =&gt; pg
[op] =&gt; index
)

想想，cate_id等于pg是不是我们需要的值呢？明显是，OK证明我们的规则写的是正确的。

现在，你再将上面的网址改为下面的：

http://localhost/sxgr/sg/pg/

将index.html删除，然后在打开，你看到什么了？

是不是告诉你找不到这个文件呢？

为什么呢？

其实是我们的规则写的有问题，因为我们的规则写的匹配内容是包含index.html的，现在却没有这个字符，所以匹配不到东西，而这个规则在整个的htaccess中并没有定义其它的，所以会这样子的。

现在在把最后面的那个斜杠也给去了，变成下面的形式：

http://localhost/sxgr/sg/pg

试试看，应该还是提示找不到文件吧？

这两种情况证明我们没有相关的规则在处理它，所以他找不到文件了。

现在在来改进这个规则，从上面的两个例子来看，我们发现其实

http://localhost/sxgr/sg/pg

应该等于

http://localhost/sxgr/sg/pg/index.html

也就是说/index.html可有可无的，那么可有可无的要怎么处理呢？

可有可无，我们翻译成正则的语法就是：0次，1次或者多次。

那么在这个地方，他是那种情况呢？

对！0次或者1次！

可能是多次吗？这个不符合实际哈。

在正则规则中，我们用？、+、*三种符号来代表次数的，当然也可以用{}来设定哈，具体自己看参考手册去。
<blockquote>?       0~1   {0,1}
+      1-多   {1,}
*       多      {0,}</blockquote>
所以我们的规则可以修改一下，如下：

RewriteRule ^(\w*)/(\w*)/(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$3

这个含义就是/index.html两个都可以只出现0次或者1次。

然后，运行上面的测试，就会发现，显示的提示就是正确的了。

那么，ShopNC的分类是有三级的，那么相应的，我们需要再写两组的规则噢，直接公布结果呗，各位同学自己思考一下跟测试一下。

# 1. /sxgs/ OR /sxgs/index.html
RewriteRule ^(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$1
# 2. /sxgs/sg/ OR /sxgs/sg/index.html
RewriteRule ^(\w*)/(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$2
# 3. /sxgs/sg/pg/index.html
RewriteRule ^(\w*)/(\w*)/(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$3

但是，这样子写，你要是访问其他页面的时候会出现问题！特别是开启了为静态的时候，你会发现原先的规则写的有点奇怪，加上我们的代码就被截取，结果发现其他的页面访问不正常了，全部都是导向查询页面噢。

这个，我也没有特别好的办法，只能是给他一个标示了，因为我们使用的是\w全部匹配的，所以什么goods拉，什么的都会被匹配到，那么我们需要将我们需要匹配的给特殊隔离开，比如，我的导航一级分类就是8大类，那么我就很容易的就区分出来了，当然你可以根据自己的实际需要设定不同的区分点。

最终修改的代码是：

# 1. /sxgs/ OR /sxgs/index.html
RewriteRule ^(sxgs|lytw|xxls|fwmc|jksp|mjcy|zbys|yybj)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$1
# 2. /sxgs/sg/ OR /sxgs/sg/index.html
RewriteRule ^(sxgs|lytw|xxls|fwmc|jksp|mjcy|zbys|yybj)/(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$2
# 3. /sxgs/sg/pg/index.html
RewriteRule ^(sxgs|lytw|xxls|fwmc|jksp|mjcy|zbys|yybj)/(\w*)/(\w*)(/)?(index.html)?$ /index.php?act=search&amp;cate_id=$3

运行，很完美哈~

当你回到主页的时候你会发现导航菜单的地址还是那种ID的形式，怎么变成我们需要的格式呢？

一般的商城都是有专门的方法来输出自己路径的，但是ShopNC我发现没有针对这个模块来特别设计，也就几个常用的会被设置，其他的，就要自己动手了。

好吧~留着么大的空间给我们，没道理偷懒吧，进入第二步的操作了~

第二步：分类的网址别名怎么弄？

很多同学可能不理解，我们一开始前面的/sxgr/sg/pg这样的形式来访问分类“生鲜果蔬\水果\苹果”那么这个拼音的形式是怎么来的？

很多没经验的朋友会在这边迷糊，特别是我早期，还不懂开发的时候看到这类文章的时候都会想，这玩意从哪里冒出来的，是作者随便写的还是真的存在呢？

在这边就描述一下这玩意是怎么出来的：其实是我，开发者随便写的，但是他可以存在的。

上面的意思就是我作为开发者根据我自己的开发需求，设计出来的，那个名称可能是这个也可能不是这个，但是这个随便写的东西，他是真是存在的！而不是随便写没意义的。

就拿我这次开发的来说吧，我在后台每一个分类都增加一个字段，用来保存这个值，在使用的时候直接读出来，然后，组装成网址即可。

当然，也可以不要后台录入，可以改为直接自动转换成拼音，然后直接组装成网址，反正，细心查看我们的重写规则会发现，起作用的只是我们的后面的参数与中间的内容是无关的。

但是，个人开发不喜欢弄得这么自动，因为必然会有其他的缺陷产生，所以建议还是弄一个字段来保存比较好。

所以，现在我们就来设计这个保存的字段呗。

2.1 保存分类别名

请使用phpmyadmin或者其他的MySQL管理工具，打开ShopNC的shopnc_goods_class表，执行如下SQL命令：

ALTER TABLE `shopnc_goods_class` ADD `gc_urlname` VARCHAR( 255 ) NULL AFTER `gc_isfood`

然后就增加一个字段了，这个字段要使用的话，还需要增加程序代码的，现在请打开\admin\templates\goods_class.index.php文件找到：

&lt;th&gt;&lt;?php echo $lang['nc_sort'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['goods_class_index_name'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['goods_class_add_type'];?&gt;&lt;/th&gt;
&lt;th class="align-center"&gt;&lt;?php echo $lang['nc_display'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['nc_handle'];?&gt;&lt;/th&gt;

然后改为如下

&lt;th&gt;&lt;?php echo $lang['nc_sort'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['goods_class_index_name'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['goods_class_add_type'];?&gt;&lt;/th&gt;
&lt;th class="align-center"&gt;地址简称&lt;/th&gt;
&lt;th class="align-center"&gt;&lt;?php echo $lang['nc_display'];?&gt;&lt;/th&gt;
&lt;th&gt;&lt;?php echo $lang['nc_handle'];?&gt;&lt;/th&gt;

然后，继续找到下面代码处，这次要认真噢，很多人在这个地方错误，结果......

&lt;td&gt;&lt;?php echo $v['type_name'];?&gt;&lt;/td&gt;

然后改为下面的代码噢

&lt;td&gt;&lt;?php echo $v['type_name'];?&gt;&lt;/td&gt;
&lt;td class="w48 sort"&gt;&lt;span title="可编辑分类地址简称" ajax_branch="gc_urlname" required="1" fieldid="&lt;?php echo $v['gc_id'];?&gt;" fieldname="gc_urlname" nc_type="inline_edit" class="editable tooltip"&gt;&lt;?php echo $v['gc_urlname'];?&gt;&lt;/span&gt;&lt;/td&gt;

保存，打开商品、分类管理，效果如下图（截图环境因为改的比较多，所以有的显示的东西没有在代码中显示，这是正常现象）

<a href="http://blog.ipodmp.com/wp-content/uploads/2015/01/shopnc_seo_goods_class_1.png"><img class="alignnone wp-image-3896 size-medium" src="http://blog.ipodmp.com/wp-content/uploads/2015/01/shopnc_seo_goods_class_1-300x57.png" alt="shopnc_seo_goods_class_1" width="300" height="57" /></a>

我们可以看到效果出来了噢，但是，如果点击一下最左边的加号的话，会发现，下级菜单里面并没有显示出来这个项目！接着就来解决这个问题。

请打开/resource/js/jquery.goods_class.js这个文件，你会发现，这里面都是控制商品分类的脚本内容噢。

//显示
src += "&lt;td class='align-center power-onoff'&gt;";

找到上面的代码出，在上方添加：

//伪静态识别地址简称
src += " &lt;td class='w48 sort'&gt;&lt;span title='可编辑下级分类地址简称' ajax_branch='gc_urlname' fieldid='"+data[i].gc_id+"' fieldname='gc_urlname' nc_type='inline_edit' class='editable tooltip'&gt;"+data[i].gc_urlname+"&lt;/span&gt;&lt;/td&gt;";
//显示
src += "&lt;td class='align-center power-onoff'&gt;";

保存，然后打开之后查看就发现，下级目录的输入框也出来了噢。

现在来测试一下呗，请在文本框里面填写你需要的内容，比如sxgs然后点击其他地方，数据就自动保存了。有没有噢？！

输入的数据就这么的保存了，有的同学很喜欢恶作戏，就会问，要是我在下面的另外一个文本框内重复填写 sxgs怎么办？可以保存吗？

答案是肯定的，可以保存！

但是！但是！在这个案子中，我们是不允许他这个值可以重复，也就是gc_urlname这个字段的值是不能重复填写的，怎么办？！

这个时候就需要高深的知识了，要认真听讲噢，我们打开\admin\control\goods_class.php文件，找到如下位置：

/**
* ajax操作
*/
public function ajaxOp(){

再往下找，你会看到

case 'goods_class_index_show':

然后增加下面的代码，完整代码如下：

case 'goods_class_index_show':
case 'gc_urlname':
/*2015.01.19 ChinaBUG 二级目录 后台行内编辑需要检查重复。*/
$model_class = Model('goods_class');
$exist = $model_class-&gt;getClassList(array('gc_urlname'=&gt;$_GET['value']));
if(count($exist)&gt;0) exit('false');
$update_array = array();
$update_array['gc_id'] = $_GET['id'];
$update_array[$_GET['column']] = $_GET['value'];
$model_class-&gt;goodsClassUpdate($update_array);
echo 'true';exit;
break;

很好，保存。

也许你急着去测试，结果你会发现，这段代码不起作用，其实不是不起作用，而是这个代码获取的值永远是数据不存在，原因就在于getClassList这个方法内部的逻辑我们没有修改。

请打开\model\goods_class.model.php文件，找到：

/**
* 构造检索条件
*
* @param int $id 记录ID
* @return string 字符串类型的返回结果
*/
private function _condition($condition){

然后增加一个逻辑：

if ($condition['gc_name'] != ''){
$condition_str .= " and gc_name = '". $condition['gc_name'] ."'";
}
//2015.01.16 ChinaBUG 二级目录开发
if ($condition['gc_urlname'] != '') {
$condition_str .= " and gc_urlname= '{$condition['gc_urlname']}'";
}

然后保存。现在去测试一下，你会发现，重复的会提醒你噢。

我个人比较不喜欢这种方式，感觉提醒的很延迟了，但是也不想改进了，没啥好的办法哈。

2.2 前台显示分类的别名

现在就需要前台调用我们的数据了噢。

这边需要提前提醒大家一件事情，那就是<span style="color: #ff00ff;"><del>后台修改完商品分类，之后请一定要记得清空缓存</del></span>。如果不清空的话，看不到效果的话，不要撞墙噢。

请打开\templates\default\layout\home_layout.php文件，这个文件就是输出前台菜单导航的地方。

找到下面代码处：

&lt;?php
if(is_array($output['show_goods_class']) &amp;&amp; count($output['show_goods_class']) != 0){ $sign = 1;

然后将里面涉及到链接的地址，比如：

index.php?act=search&amp;cate_id=&lt;?php echo $val['gc_id'];?&gt;

直接用：

&lt;?php echo ncrwURL($val,$output['show_goods_class']);?&gt;

代替。其他的地方替代后的完整代码如下：

&lt;?php
if(is_array($output['show_goods_class']) &amp;&amp; count($output['show_goods_class']) != 0){ $sign = 1;
foreach ($output['show_goods_class'] as $tkey=&gt;$val){
if ($val['gc_parent_id'] != '0') break;
?&gt;
&lt;li id="cat_&lt;?php echo $sign;?&gt;" &lt;?php /*修改商品分类*/ if($sign&gt;8){?&gt;style="display:none;"&lt;?php }?&gt;&gt;
&lt;h3&gt;&lt;a href="&lt;?php echo ncrwURL($val,$output['show_goods_class']);?&gt;" title="&lt;?php echo $val['gc_name']?&gt;"&gt;&lt;?php echo $val['gc_name']?&gt;&lt;/a&gt;&lt;/h3&gt;
&lt;div class="cat-menu" id="cat_&lt;?php echo $sign;?&gt;_menu" &gt;
&lt;?php
if($val['child'] != ''){
foreach(explode(',',$val['child']) as $k){
?&gt;
&lt;dl&gt;
&lt;dt&gt;&lt;a href="&lt;?php echo ncrwURL($output['show_goods_class'][$k],$output['show_goods_class']);?&gt;" title="&lt;?php echo $output['show_goods_class'][$k]['gc_name']?&gt;" &gt;&lt;?php echo $output['show_goods_class'][$k]['gc_name']?&gt;&lt;/a&gt;&lt;/dt&gt;
&lt;dd&gt;
&lt;?php
if($output['show_goods_class'][$k]['child'] != ''){
foreach(explode(',',$output['show_goods_class'][$k]['child']) as $key){
?&gt;
&lt;a href="&lt;?php echo ncrwURL($output['show_goods_class'][$key],$output['show_goods_class']);?&gt;" title="&lt;?php echo $output['show_goods_class'][$key]['gc_name']?&gt;" &gt;&lt;?php echo $output['show_goods_class'][$key]['gc_name']?&gt;&lt;/a&gt;
&lt;?php } } ?&gt;
&lt;/dd&gt;
&lt;/dl&gt;
&lt;?php } } ?&gt;
&lt;/div&gt;
&lt;/li&gt;
&lt;?php $sign++; } } ?&gt;

其中，我们定义了一个自定义的方法，统一输出路径，这样子方便未来修改规则后的调整噢，你要手动修改也是可以的，咱不多说噢。

请打开\framework\function\core.php文件，然后再文件末尾黏贴下面的方法：

/**
* 2015.01.19 ChinaBUG 二级目录
*/
function ncrwURL($val,$show_goods_class)
{
//一级
if( !empty($val['gc_urlname']) &amp;&amp; ($val['gc_parent_id']==0) ) return '/'.$val['gc_urlname'].'/';
//二级 OR 多级
if( !empty($val['gc_urlname']) &amp;&amp; ($val['gc_parent_id']!=0) &amp;&amp; !empty($show_goods_class[ $val['gc_parent_id'] ]['gc_urlname']) ){
$url = '/'.$show_goods_class[ $val['gc_parent_id'] ]['gc_urlname'].'/'.$val['gc_urlname'].'/';
if($show_goods_class[ $show_goods_class[ $val['gc_parent_id'] ]['gc_parent_id'] ]['gc_urlname'])
$url = '/'.$show_goods_class[ $show_goods_class[ $val['gc_parent_id'] ]['gc_parent_id'] ]['gc_urlname'].$url;
return $url;
}
//没有自定义网址则返回默认
return '/index.php?act=search&amp;cate_id='.$val['gc_id'];
}

呵呵，懒得优化了，你们自行优化一下吧。

然后就会发现前台的分类网址就变了。

如果没变的话，那么请检查后台是不是没有设定地址简称，另外就算设置了，还需要满足两个条件噢。一个就是本身要有地址简称，一个是上级，上上级都要有简称，具体逻辑请看上面的ncrwURL方法。

修改之后会发现，完美完成我们的预期噢。

不过要是细心一点还是会发现我们的搜索页面的左边的分类还是没有转换地址噢。下面就来设置一下呗。

2.3 搜索页面的相关位置修改

2.3.1 分类列表

打开\templates\default\home\category_goods.php文件，将里面的代码涉及到地址的都改了。

具体修改如下：

&lt;?php foreach(explode(',',$gc_list['childchild']) as $key=&gt;$child){?&gt;
&lt;a href="&lt;?php echo ncrwURL($output['gc_list'][$child],$output['gc_list']);?&gt;"&gt;&lt;?php echo $output['gc_list'][$child]['gc_name'];?&gt;&lt;/a&gt;
&lt;?php }?&gt;

保存。

2.3.2 搜索列表左边

打开\templates\default\home\search.php，找到下面所在位置：

&lt;dl nc_type="ul_category"&gt;
&lt;dt&gt;&lt;?php echo $output['goods_class_array']['gc_name'];?&gt;&lt;/dt&gt;

改为：

&lt;dl nc_type="ul_category"&gt;
&lt;dt&gt;&lt;?php echo $output['goods_class_array']['gc_name'];?&gt;&lt;/dt&gt;
&lt;?php if(is_array($output['goods_class_array']['child']) &amp;&amp; !empty($output['goods_class_array']['child'])){?&gt;
&lt;?php
/* 2015.01.19 ChinaBUG 二级目录*/
foreach ($output['goods_class_array']['child'] as $val){?&gt;
&lt;dd&gt;&amp;nbsp;&amp;nbsp;&lt;a href="&lt;?php if(!empty($output['show_goods_class'][$val['gc_id']]['gc_urlname'])){echo ncrwURL($output['show_goods_class'][$val['gc_id']],$output['show_goods_class']);
}else{?&gt;javascript:replaceParam('cate_id','&lt;?php echo $val['gc_id'];?&gt;')&lt;?php }?&gt;"&gt;&lt;?php echo $val['gc_name'];?&gt;&lt;/a&gt;&lt;/dd&gt;
&lt;?php }?&gt;
&lt;?php }?&gt;
&lt;/dl&gt;

2.3.3 其他

不懂哪里还没改到，你要是自己看到就自己修改呗，总的原理就是涉及到网址的地方全部用我们自定义的方法输出即可。

现在修改完毕之后发现这个是很完美的事情，都改写好了。

或许我们还遗忘一个地址还没改噢......

2.4 当前所在所在

打开\model\goods_class.model.php文件，找到：

/**
* 取指定分类ID的导航链接
*
* @param int $id 父类ID/子类ID
* @return array $nav_link 返回数组形式类别导航连接
*/
//2015.01.19 ChinaBUG 二级目录 目前所在的处理
public function getGoodsClassNav($id = 0){

然后将里面的那个循环整个换成下面的代码即可：

//最多循环4层
for($i=1;$i&lt;5;$i++){
if ($data[$id]['gc_parent_id'] == '0') break;
$id = $data[$id]['gc_parent_id'];
$nav_link[$i]['title'] = $data[$id]['gc_name'];
$nav_link[$i]['link'] = ncrwURL($data[$id],$data);
}

保存即可。