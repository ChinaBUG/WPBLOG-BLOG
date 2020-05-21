---
ID: 3062
post_title: >
  ShopNC二次开发研究日记8:增加主页团购推荐模块的推荐数量
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  首页团购推荐模块里面只有一个推荐商品，不够用怎么办呢？DIY呗~下面就是打招多推荐模块的方法。
  首先请打开control/index.php文件，你会很明显的看到下面的代码：
  //团购专区
  Language::read('member_groupbuy');
  $param = array();
  $param['recommended'] = 1;
  $param['state'] = 3;
  $param['in_progress'] = time();
  $param['limit'] = 1;
  $model_group = Model('goods_group');
  $group_list = $model_group->getList($param);
  Tpl::output('group_list',$group_list[0]);
  Tpl::output('count_down',$group_list[0]['end_time'] - time());
  ShopNC系统个人觉得很开发也好，浏览体验也好，都比较糟糕的，但是唯一觉得欣赏的是这个，他们家的程序员很可爱，经常性的写注释，然后还很明确，这个习惯非常棒！
  虽然这段代码写的感觉有点神经质了一点，为什么？请看进阶版的即可知道啦。
  唠叨说完，继续来做DIY吧~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopnc-2dev-study-of-diary-8-home-buy-recommendation-to-increase-the-number-of-recommended-modules.html
published: true
post_date: 2013-12-17 13:06:42
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

首页团购推荐模块里面只有一个推荐商品，不够用怎么办呢？DIY呗~下面就是打招多推荐模块的方法。

首先请打开control/index.php文件，你会很明显的看到下面的代码：
<pre>//团购专区
Language::read('member_groupbuy');
$param = array();
$param['recommended'] = 1;
$param['state'] = 3;
$param['in_progress'] = time();
$param['limit'] = 1;
$model_group = Model('goods_group');
$group_list = $model_group-&gt;getList($param);
Tpl::output('group_list',$group_list[0]);
Tpl::output('count_down',$group_list[0]['end_time'] - time());</pre>
ShopNC系统个人觉得很开发也好，浏览体验也好，都比较糟糕的，但是唯一觉得欣赏的是这个，他们家的程序员很可爱，经常性的写注释，然后还很明确，这个习惯非常棒！

虽然这段代码写的感觉有点神经质了一点，为什么？请看进阶版的即可知道啦。

唠叨说完，继续来做DIY吧~

上面的代码有点经验的人会发现他有一个limit属性，然后正常我们就是修改一下然后他就会输出了对吧~恩，那就修改吧，我们这边修改为2（或者你希望的团购数量），然后清除缓存之后你会发现，哇咧，怎么还是一个~然后我们再仔细一看，噢，原来如此~

亮点就是在：
<pre>Tpl::output('group_list',$group_list[0]);
Tpl::output('count_down',$group_list[0]['end_time'] - time());</pre>
有经验的朋友一看就知道，这边是单单输出一个记录，就是你上面查询到不管多少条记录，永远只输出最前面的第一条记录。

怎么修改呢？这边DIY的我将提供两种修改的方法，一种比较复杂，一种比较简单，至于要用那种看你自己了噢。

<span style="text-decoration: underline;"><strong>简单版本</strong></span>

反正我只要输出两个团购推荐的模块，很少，那就来个简单的吧~请复制上面的并修改为下面的代码：
<pre>Tpl::output('group_list<strong><span style="color: #ff0000;">_1</span></strong>',$group_list[<strong><span style="color: #ff0000;">1</span></strong>]);
Tpl::output('count_down<span style="color: #ff0000;"><strong>_1</strong></span>',$group_list[<strong><span style="color: #ff0000;">1</span></strong>]['end_time'] - time());</pre>
看到了没有？重点在红色标记的“<span style="color: #ff0000;">1”</span>与“<span style="color: #ff0000;">_1”</span>上面，原先的是0代表的是查询到的数据的第一条记录，那么1代表的就是查询到的记录的第二条了噢，如果三条呢？自然就是2啦，以此类推......

改好了就保存吧，现在这个文件暂时就不修改了，我们需要修改一下这个对应的模板文件了~

请打开templates/default/home/index.php文件，然后找到大概238行，当然你也可以搜索：

&lt;?php if(is_array($output['group_list'])) { ?&gt;

这个关键词。然后下面的都是这个团购推荐模块的代码啦，我们来修改一下噢，下面的要很注意很注意，因为一个不小心，可能就不是你需要的效果了噢。请将下面代码表示的区域复制一份，然后黏贴到你的记事本中，噢，你这个时候需要打开记事本。
<pre>&lt;?php if(is_array($output['group_list'])) { ?&gt;
&lt;dl&gt;
...//省略的代码也是要复制的噢，上面跟下面的代码是区域噢。
&lt;/dl&gt;
&lt;?php } ?&gt;</pre>
然后请在记事本里使用替换功能，替换如下内容：

group_list替换为group_list_1

count_down替换为count_down_1

然后替换完毕复制全部，回到刚才复制的文件里面，<strong><span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">在我们刚才复制的代码的下面，记住，是我们刚才复制的代码的下面</span></span></strong>，黏贴修改之后的代码！具体的形式为下面的情况：
<pre>&lt;?php if(is_array($output['group_list'])) { ?&gt;
&lt;dl&gt;
...//省略的代码也是要复制的噢，上面跟下面的代码是区域噢。
&lt;/dl&gt;
&lt;?php } ?&gt;
&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@S--&gt;
&lt;?php if(is_array($output['group_list_1'])) { ?&gt;
&lt;dl&gt;
...//同样省略代码
&lt;/dl&gt;
&lt;?php } ?&gt;
&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@E--&gt;</pre>
然后保存吧~重新打开网站你会发现，噢噢噢~~团购推荐模块居然变两个了噢，格式完美呀~

然后你会发现，倒计时居然不能用啦！为啥？呵呵其实我们是修改的不完整的原因啦，请将刚才修改的代码里面的下面的代码修改为下面的形式即可正常工作了噢：
<pre>&lt;dd&gt;
&lt;s&gt;&lt;/s&gt;
&lt;span&gt;
    &lt;span id="d<span style="color: #ff0000;">2</span>"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_tian'];?&gt;
    &lt;span id="h<span style="color: #ff0000;">2</span>"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_hour'];?&gt;
    &lt;span id="m<span style="color: #ff0000;">2</span>"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_minute'];?&gt;
    &lt;span id="s<span style="color: #ff0000;">2</span>"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_second'];?&gt;
&lt;/span&gt; 
&lt;script type="text/javascript"&gt;
tms[tms.length] = "&lt;?php echo $output['count_down_1'];?&gt;";
day[day.length] = "d<span style="color: #ff0000;">2</span>";
hour[hour.length] = "h<span style="color: #ff0000;">2</span>";
minute[minute.length] = "m<span style="color: #ff0000;">2</span>";
second[second.length] = "s<span style="color: #ff0000;">2</span>";
&lt;/script&gt; 
&lt;/dd&gt;</pre>
请注意红色部分的明明，这个是需要注意的，要是找不到调用的对象，那个计时器肯定是不正确的。

<span style="text-decoration: underline;"><strong>进阶版本</strong></span>

看到上面的效果是不是很兴奋？可是一想起如果你要做推荐3个、4个等等，很多的话，那么要复制多少次噢，脑细胞死多少都不知道呢，当然你觉得没关系的话，那就不用往下看了，这个解决方案不是给你看的。
<blockquote><em>PS：知道为什么说ShopNC的程序猿有点神经质了吧~好好的数组非要那样子处理，害的我们自己还要修改代码，不够灵活呀。</em></blockquote>
话说程序就是用来偷懒的，所以，会用程序的人就叫程序猿了~我们来进阶版本的吧，偷偷懒，让那些勤快的小伙伴看着眼热~

同样我们打开control/index.php文件，然后找到
<pre>Tpl::output('group_list',$group_list[0]);
Tpl::output('count_down',$group_list[0]['end_time'] - time());</pre>
然后修改为下面的代码：
<pre>//2013.12.17 ChinaBUG 增加团购推荐模块
foreach($group_list as $glkey=&gt;$glval){
    $count_down[$glkey] = $glval['end_time'] - time();
}                
Tpl::output('group_lists',$group_list);
Tpl::output('count_downs',$count_down);</pre>
然后保存。

请注意上面的代码跟原先的代码的不同，我们在这边是直接输出获取到的全部的推荐的团购商品，然后特别对倒计时的时间做了一个处理。

请打开templates/default/home/index.php文件，然后找到大概238行，当然你也可以搜索：

&lt;?php if(is_array($output['group_list'])) { ?&gt;

然后，要特别注意了噢，简单版本的我们是复制代码并做修改，现在进阶版的我们不需要复制了，但是还是需要做修改的哈。然后第一步老规矩，我喜欢在我修改的地方做一个注释，告诉自己修改是做什么用的，到时候需要的时候一搜索，哇咧都出来了噢。
<pre>&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@S--&gt;
<strong><span style="color: #ff0000;">区域A</span></strong>
&lt;?php if(is_array($output['group_list'])) { ?&gt;
&lt;dl&gt;
....<strong><span style="color: #ff0000;">区域C</span></strong>//继续省略
&lt;/dl&gt;
&lt;?php } ?&gt;
<strong><span style="color: #ff0000;">区域B</span></strong>
&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@E--&gt;</pre>
那个&lt;!----&gt;里面的时间，署名还有功能说明都是注释，方便自己哈，你不要的话，等一下代码乱了别跟我说没提醒你，因为除了注释说明之外，这个还是区分你要修改的代码与不修改代码的界限，要不有时候一糊涂，就捞过界了噢。

有颜色的区域A，B，C是为了等下说明方便加上去的，实际上这个文字是不能有的，等下要换成下面说到的相应的代码的。

然后请在区域A输入以下代码：

&lt;?php foreach($output['group_lists'] as $glkey=&gt;$glval) { ?&gt;

然后请在区域B输入以下代码：

&lt;?php } ?&gt;

然后，注意了，请将区域A与区域B之间的代码复制到记事本中，我们将对以下变量做替换：

$output['group_list']替换成$glval；

$output['count_down']替换成$output['count_downs'][$glkey]；

查找"d1"替换成"d&lt;?php echo $glkey;?&gt;"，注意引号是需要的，就是要替换的内容就是<span style="color: #ff0000;"><strong><span style="text-decoration: underline;">引号d1引号</span></strong></span>；

查找"h1"替换成"h&lt;?php echo $glkey;?&gt;"，注意引号是需要的，就是要替换的内容就是<span style="color: #ff0000;"><strong>引号h1引号</strong></span>；

查找"m1"替换成"m&lt;?php echo $glkey;?&gt;"，注意引号是需要的，就是要替换的内容就是<span style="color: #ff0000;"><strong>引号m1引号</strong></span>；

查找"s1"替换成"s&lt;?php echo $glkey;?&gt;"，注意引号是需要的，就是要替换的内容就是<span style="color: #ff0000;"><strong>引号s1引号</strong></span>；

替换完毕复制全部，黏贴回刚才的地方。
<blockquote><em>提醒：</em>
<em>刚才的地方是哪里？在区域A与区域B之间！</em></blockquote>
然后保存，然后运行，你会发现，刚才简单版的效果又出现了，最方便的是不需要修改倒计时的那个步骤了~然后你看看会发现代码量也减少许多噢。

最后的代码如下：<span style="color: #ff0000;"><em><span style="text-decoration: underline;">注意，下面的代码有省略所以不能直接用于执行~</span></em></span>
<pre>&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@S--&gt;
&lt;?php foreach($output['group_lists'] as $glkey=&gt;$glval) { ?&gt;
&lt;?php if(is_array($glval)) { ?&gt;
&lt;dl&gt;
    &lt;dt&gt;&lt;a href="&lt;?php echo ncUrl(array(...'group_id'=&gt;$glval['group_id'],'id'=&gt;$glval['store_id'])...&lt;/dt&gt;
    &lt;dd&gt;
        &lt;div&gt;&lt;a href="&lt;?php echo ncUrl(...'group_id'=&gt;$glval['group_id'],'id'=&gt;$glval['store_id'])...&lt;/div&gt;
    &lt;/dd&gt;
    ......
    &lt;dd&gt;
        &lt;s&gt;&lt;/s&gt;
        &lt;span&gt;
            &lt;span id="d&lt;?php echo $glkey;?&gt;"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_tian'];?&gt;
            &lt;span id="h&lt;?php echo $glkey;?&gt;"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_hour'];?&gt;
            &lt;span id="m&lt;?php echo $glkey;?&gt;"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_minute'];?&gt;
            &lt;span id="s&lt;?php echo $glkey;?&gt;"&gt;0&lt;/span&gt;&lt;?php echo $lang['text_second'];?&gt;
        &lt;/span&gt;
        &lt;script type="text/javascript"&gt;
        tms[tms.length] = "&lt;?php echo $output['count_downs'][$glkey];?&gt;";
        day[day.length] = "d&lt;?php echo $glkey;?&gt;";
        hour[hour.length] = "h&lt;?php echo $glkey;?&gt;";
        minute[minute.length] = "m&lt;?php echo $glkey;?&gt;";
        second[second.length] = "s&lt;?php echo $glkey;?&gt;";
        &lt;/script&gt; 
    &lt;/dd&gt;
    ........
&lt;/dl&gt;
&lt;?php } ?&gt;
&lt;?php } ?&gt;
&lt;!--#2013.12.17 ChinaBUG 增加团购推荐模块@E--&gt;</pre>
如果出现问题的话请对照一下上面的主要代码，查看一下是不是代码替换错误。