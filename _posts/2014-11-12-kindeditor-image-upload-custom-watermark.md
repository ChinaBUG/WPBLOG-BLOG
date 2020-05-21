---
ID: 3688
post_title: >
  KindEditor-图片上传如何自定义是否添加水印
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/kindeditor-image-upload-custom-watermark.html
published: true
post_date: 2014-11-12 14:52:02
---
最近新的ThinkPHP项目中使用原版的RBAC后台，该后台使用的可视化编辑器是KindEditor 3.5.1 (2010-07-18)版本的，正好项目中需要上传图片并添加水印功能，那么怎么办呢？

这边就说一下我的开发过程并展示部分代码噢。

<em><strong>第一篇 上传图片增加水印</strong></em>

因为本身编辑器就有上传的功能，那么我们只需要解决的问题就是如何添加水印。其实添加水印很简单，就是需要在上传接收程序里面对上传的图片给与处理一下就可以了，我们只需要在处理程序../../php/upload_json.php这个文件里面增加添加水印的代码即可。

具体如下：

修改程序文件：/Public/Js/editor/php/upload_json.php

具体代码如下：

随便在顶部找一个地方定义一下这个常量，具体还是需要根据自己的实际情况来定噢：

[php]
define('BASE_DIR', realpath(dirname(__FILE__).'/../../../../../'));
[/php]

然后，在增加下面代码，位置随便你

[php]
include BASE_DIR.'/Admin/App/Lib/ORG/Util/Image.class.php';
$Image = new Image();
[/php]

<blockquote>注：
如果这个上传的文件是标准的ThinkPHP的控制器或者啥的，那么就可以使用内置的方法调用了，如果不是，那么就只能自己载入了。
ThinkPHP内部载入方法如下：<span class="kwd">import</span><span class="pun">(</span><span class="str">'ORG.Util.Image'</span><span class="pun">);</span>
PS:具体可以查看使用手册的“图片添加水印”部分</blockquote>
然后找到下面代码处：

&nbsp;

[php]
if (move_uploaded_file($tmp_name, $file_path) === false) {

//......

}
[/php]

&nbsp;

接着写下面的代码：

[php]
//图片保存完毕就加水印
$wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
$Image-&gt;water($file_path, $wm_sy,$file_path,50);
[/php]

这行代码的意思是，如果文件移动成功，也就是上传成功之后，就对这个文件添加水印，并保存为文件，文件名跟上传保存的文件名一致。$wm_sy这个变量代表的是水印文件的地址，请使用PNG文件，要不是不能透明的。

完整的修改代码如下：

[php]

require_once 'JSON.php';
define('BASE_DIR', realpath(dirname(__FILE__).'/../../../../../'));
//2014.11.11 ChinaBUG 添加水印
include BASE_DIR.'/Admin/App/Lib/ORG/Util/Image.class.php';
$Image = new Image();
......

if (move_uploaded_file($tmp_name, $file_path) === false) {
alert(&quot;上传文件失败。&quot;);
}else{
//图片保存完毕就加水印
$wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
$Image-&gt;water($file_path, $wm_sy,$file_path,50);
}

[/php]

执行之后我们会发觉水印打在右下角右对齐，什么！没有水印？好吧，请先检查一下上传的图片的尺寸是不是小于水印的尺寸，如果是，水印是不起作用的噢。

人是不知足的，所以你可能觉得水印默认在右下角右对齐很不自由，想要上传的时候自己指定对吧？

现在就来搞一下怎么指定水印的位置。

<em><strong>第二篇 上传图片指定添加水印的位置</strong></em>

我们先在添加页面增加一个布局工具，我做的是一个九宫格，具体如下：

<a href="http://blog.ipodmp.com/wp-content/uploads/2014/11/QQ截图20141124152402.png"><img class="alignnone size-full wp-image-3721" src="http://blog.ipodmp.com/wp-content/uploads/2014/11/QQ截图20141124152402.png" alt="九宫格水印位置" width="66" height="64" /></a>

这个九宫格，只能选择一个，其他都是不能选中的噢。具体的JS代码很简单就自己写吧。
<blockquote><em>提示：有不会的童靴就给个提示吧，要做到有且只能有一个被选中，那么需要先获取选中的多选框，然后赋值给中间变量，然后获取全部的多选框，把checked属性全部都置为否，然后对比当前的多选框是不是我们选中的那个，是就置为是即可。</em></blockquote>
这个九宫格的名字我们就取做name="wm_show_pos[]"，这个可以自行修改，不过改了代码要记得修改就对了。

然后，我们怎么把这个值传入KindEditor中，这个比较大的学问了，咱也是看了好久的文档才发现原来人家有预留接口的。

我是先用JS获取上面多选框的具体值，然后将这个值指定给KindEditor的参数，然后剩下的事情就是写判断逻辑了~

先来看看，我们是怎么传递参数给KindEditor的。查看文档会发现KE有一个方法叫做：imageUploadJson就是用来指定上传的处理程序的路径的。

[code lang="javascript"]

&lt;script type=&quot;text/javascript&quot;&gt;
KE.show({
id : '&lt;span style=&quot;color: #ff0000;&quot;&gt;content_1&lt;/span&gt;', //TEXTAREA输入框的ID
imageUploadJson : '../../php/upload_json.php?wmpos=7'
});
&lt;/script&gt;

[/code]

然后，我们要修改这个的值怎么办呢？其实很简单，查看源码或者手册你就知道，其实就只使用它的全局对象来获取即可。代码如下：

[code lang="javascript"]

&lt;script type=&quot;text/javascript&quot;&gt;
KE.g['&lt;span style=&quot;color: #ff0000;&quot;&gt;content_1&lt;/span&gt;'].imageUploadJson = '../../php/upload_json.php?wmpos='+pos_default;
&lt;/script&gt;

[/code]

其中，pos_default的值是点击九宫格之后，赋予的中间变量噢。这样子的话，wmpos这个值就传递到处理程序中，我们就能获取位置信息，并对这个参数做处理了。下面再来看看怎么处理的呗~

我们修改一下第一步的代码噢。

[php]
//2014.11.11 ChinaBUG 添加水印
if((isset($_POST['wmpos']) &amp;&amp; is_numeric($_POST['wmpos']))||(isset($_GET['wmpos']) &amp;&amp; is_numeric($_GET['wmpos']))){
    $wmpos = $_POST['wmpos']?$_POST['wmpos']:$_GET['wmpos'];
    include BASE_DIR.'/Admin/App/Lib/ORG/Util/Image.class.php';
    $Image = new Image();
}
[/php]

增加获取传入的wmpos的值，如果没有传入值，则为默认方式不做水印处理，这样子，在应用的时候不需要修改太多的代码噢，特别是以前已经写过的代码就不需要全部修改一次了。

我们再找到

[php]

......
if (move_uploaded_file($tmp_name, $file_path) === false) {
alert(&quot;上传文件失败。&quot;);
}else{
//图片保存完毕就加水印
$wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
$Image-&gt;water($file_path, $wm_sy,$file_path,50,$wmpos);
}

[/php]

改为如下：

[php]
    ......
    if (move_uploaded_file($tmp_name, $file_path) === false) {
        alert(&quot;上传文件失败。&quot;);
    }elseif(isset($wmpos)){
        //图片保存完毕就加水印
        $wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
        //加水印
        $Image-&gt;water($file_path, $wm_sy,$file_path,100,$wmpos);
    }
[/php]

接着我们再来修改$Image-&gt;water方法，增加一个参数，指定水印的位置。

打开Image.class.php文件，找到water方法处，再找到

[php]

$posY = $sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;];
$posX = $sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;];

[/php]

改为如下代码：

[php]

......

//图像位置,默认为右下角右对齐
//$posY = $sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;];
//$posX = $sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;];
//2014.11.11 ChinaBUG
//图像位置
switch($pos)
{
case 1://1为顶端居左
$posX = 0;
$posY = 0;
break;
case 2://2为顶端居中
$posX = ($sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;]) / 2;
$posY = 0;
break;
case 3://3为顶端居右
$posX = $sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;];
$posY = 0;
break;
case 4://4为中部居左
$posX = 0;
$posY = ($sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;]) / 2;
break;
case 5://5为中部居中
$posX = ($sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;]) / 2;
$posY = ($sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;]) / 2;
break;
case 6://6为中部居右
$posX = $sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;];
$posY = ($sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;]) / 2;
break;
case 7://7为底端居左
$posX = 0;
$posY = $sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;];
break;
case 8://8为底端居中
$posX = ($sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;]) / 2;
$posY = $sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;];
break;
case 9://9为底端居右
$posX = $sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;];
$posY = $sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;];
break;
default://随机
$posX = rand(0,($sInfo[&quot;width&quot;] - $wInfo[&quot;width&quot;]));
$posY = rand(0,($sInfo[&quot;height&quot;] - $wInfo[&quot;height&quot;]));
break;
}

[/php]

然后就可以了噢，现在只要传入$wmpos参数值，就可以按照设置的位置添加水印了。

当然，有的同学还不满足这个功能，他们会遇上，水印很小，但是图很大，水印打上去，不是很美观，想要将上传的图片给缩放一定的比例，怎么办呢？第三篇就是告诉你怎么修改噢。

<em><strong>第三篇 上传的图片怎么进行缩放</strong></em>

缩放的功能Image.class.php这个类有提供给我们直接调用，现在我们来修改一下上面的代码，上传的图片如果小于300像素则不做处理，大于600则缩放为600像素。

我们打开upload_json.php文件，找到

[php]
    ......
    if (move_uploaded_file($tmp_name, $file_path) === false) {
        alert(&quot;上传文件失败。&quot;);
    }elseif(isset($wmpos)){
        //图片保存完毕就加水印
        $wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
        //加水印
        $Image-&gt;water($file_path, $wm_sy,$file_path,100,$wmpos);
    }
[/php]

修改为

[php]
    ......
    if (move_uploaded_file($tmp_name, $file_path) === false) {
        alert(&quot;上传文件失败。&quot;);
    }elseif(isset($wmpos)){
        //图片保存完毕就加水印
        $wm_sy = BASE_DIR.'/uploadfile/ganxu.png';
        //设置水印：小于300不处理大于600则缩放
                //图片信息
        $sInfo = $Image-&gt;getImageInfo($file_path);
        if ($sInfo[&quot;width&quot;] &lt; 300 ){
            //不加水印
        }elseif($sInfo[&quot;width&quot;]&gt;600){
            //缩小加水印
            //缩小
            $Image-&gt;thumb($file_path, $file_path,'', $maxWidth=600, $maxHeight=600);
            //加水印
            $Image-&gt;water($file_path, $wm_sy,$file_path,100,$wmpos);
        }else{
            //加水印
            $Image-&gt;water($file_path, $wm_sy,$file_path,100,$wmpos);
        }
    }
[/php]

需求完美解决了。

其实整个的修改其实最难的是Image.class.php这个文件的引入，因为路径的问题是很容易出错的。另外一个难点就是KindEditor的参数调用。不知道是不是我看漏了，居然没有地方告知如何获取参数，我还是从源码中分析得出需要那样子访问全局的。

汗~~可能当时急没认真看文档吧。有人知道在哪里的话可以与我交流一下哈。