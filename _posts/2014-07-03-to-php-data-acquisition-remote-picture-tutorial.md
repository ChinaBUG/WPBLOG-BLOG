---
ID: 3520
post_title: 转:PHP采集远程图片详细教程
author: ChinaBUG
post_excerpt: |
  虫神曰：还是可以的，条理很清晰噢。适合初学者阅读噢。
  原文摘自《PHP采集远程图片详细教程》
  当我们需要采集网络上的某个网页内容时，如果目标网站上的图片做了防盗链的话，我们直接采集过来的图片在自己网站上是不可用的。那么我们使用程序将目标网站上的图片下载到我们网站服务器上，然后就可调用图片了。
  本文将使用PHP实现采集远程图片功能。基本流程：
  1、获取目标网站图片地址。
  2、读取图片内容。
  3、创建要保存图片的路径并命名图片名称。
  4、写入图片内容。
  5、完成。
  我们通过写几个函数来实现这一过程。
  函数make_dir()建立目录。判断要保存的图片文件目录是否存在，如果不存在则创建目录，并且将目录设置为可写权限。
layout: post
permalink: >
  http://blog.ipodmp.com/2014/07/to-php-data-acquisition-remote-picture-tutorial.html
published: true
post_date: 2014-07-03 09:47:20
---
虫神曰：还是可以的，条理很清晰噢。适合初学者阅读噢。

原文摘自《<a href="http://bbs.51cto.com/thread-1114148-1.html" target="_blank">PHP采集远程图片详细教程</a>》

当我们需要采集网络上的某个网页内容时，如果目标网站上的图片做了防盗链的话，我们直接采集过来的图片在自己网站上是不可用的。那么我们使用程序将目标网站上的图片下载到我们网站服务器上，然后就可调用图片了。

本文将使用PHP实现采集远程图片功能。基本流程：
1、获取目标网站图片地址。
2、读取图片内容。
3、创建要保存图片的路径并命名图片名称。
4、写入图片内容。
5、完成。

我们通过写几个函数来实现这一过程。

函数make_dir()建立目录。判断要保存的图片文件目录是否存在，如果不存在则创建目录，并且将目录设置为可写权限。

代码如下:

[php]function make_dir($path){
    if(!file_exists($path)){//不存在则建立
        $mk=@mkdir($path,0777); //权限
        @chmod($path,0777);
    }
    return true;
}[/php]

函数read_filetext()取得图片内容。使用fopen打开图片文件，然后fread读取图片文件内容。
代码如下:

[php]function read_filetext($filepath){
    $filepath=trim($filepath);
    $htmlfp=@fopen($filepath,&quot;r&quot;);
    //远程
    if(strstr($filepath,&quot;://&quot;)){
        while($data=@fread($htmlfp,500000)){
            $string.=$data;
        }
    }
    //本地
    else{
        $string=@fread($htmlfp,@filesize($filepath));
    }
    @fclose($htmlfp);
    return $string;
}[/php]

函数write_filetext()写文件，将图片内容fputs写入文件中，即保存图片文件。
代码如下:

[php]function write_filetext($filepath,$string){
    //$string=stripSlashes($string);
    $fp=@fopen($filepath,&quot;w&quot;);
    @fputs($fp,$string);
    @fclose($fp);
}[/php]

函数get_filename()获取图片名称，也可以自定义要保存的文件名。
代码如下:

[php]function get_filename($filepath){
    $fr=explode(&quot;/&quot;,$filepath);
    $count=count($fr)-1;
    return $fr[$count];
}[/php]

然后将几个函数组合，在函数save_pic()中调用，最后返回保存后的图片路径。
代码如下:

[php]function save_pic($url,$savepath=''){
    //处理地址
    $url=trim($url);
    $url=str_replace(&quot; &quot;,&quot;%20&quot;,$url);
    //读文件
    $string=read_filetext($url);
    if(empty($string)){
        echo '读取不了文件';exit;
    }
    //文件名
    $filename = get_filename($url);
    //存放目录
    make_dir($savepath); //建立存放目录
    //文件地址
    $filepath = $savepath.$filename;
    //写文件
    write_filetext($filepath,$string);
    return $filepath;
}[/php]

最后一步就是调用save_pic()函数保存图片，我们使用以下代码做测试。
代码如下:

[php]//目标图片地址
$pic = &quot;http://www.wannuoda.com /static/1205/06/2776119_end1_thumb.jpg&quot;;
//保存目录
$savepath = &quot;images/&quot;;
echo save_pic($pic,$savepath);[/php]

实际应用中，我们可能会采集某个站点的内容，比如产品信息，包括采集防盗链的图片保存到网站上服务器上。这时我们可以使用正则匹配页面内容，将页面中相匹配的图片都找出来，然后分别下载到网站服务器上，完成图片的采集。以下代码仅供测试：
代码如下:

[php]function get_pic($cont,$path){
    $pattern_src = '/&lt;[img|IMG].*?src=[\'|\&quot;](.*?(?:[\.gif|\.jpg]))[\'|\&quot;].*?[\/]?&gt;/';
    $num = preg_match_all($pattern_src, $cont, $match_src);
    $pic_arr = $match_src[1]; //获得图片数组
    foreach ($pic_arr as $pic_item) { //循环取出每幅图的地址
        save_pic($pic_item,$path); //下载并保存图片
        echo &quot;[OK]..!&quot;;
    }
}[/php]

然后我们通过分析页面内容，将主体内容找出来，调用get_pic()实现图片的保存。
代码如下:

[php]//我们采集济南万诺达网站建设公司网上上一篇关于网站建设内容页的图片
$url = &quot;http://www.wannuoda.com /321/3215791.html&quot;;

$content = file_get_contents($url);//获取网页内容
$preg = '#&lt;div class=&quot;art_con&quot;&gt;(.*)&lt;div class=&quot;ivy620 ivy620Ex&quot;&gt;&lt;/div&gt;#iUs';
preg_match_all($preg, $content, $arr);
$cont = $arr[1][0];  
get_pic($cont,'img/');[/php]

以上代码笔者亲测，可以采集图片，但是还有些场景没考虑进去，比如目标网站做了302多次跳转的，目标网站做了多种防采集的，留给喜欢折腾的同学去试试吧。

&nbsp;