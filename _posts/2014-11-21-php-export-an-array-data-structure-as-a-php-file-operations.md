---
ID: 3712
post_title: >
  PHP-导出数组数据结构存为.php文件的操作
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/php-export-an-array-data-structure-as-a-php-file-operations.html
published: true
post_date: 2014-11-21 12:05:01
---
最近用ThinkPHP开发一个小型旅游站，然后要将分类，友情链接等常用的信息给缓存起来，生成.php文件调用。然后，就产生这篇文章了，这边记录一下我的做法吧，有高深的方法不要忘记推荐噢。

我的做法有两种，一种是自己完全的自己输出，一种是使用已经存在的内置方法结合输出。具体的优劣就看自己的需要了，详细代码如下，这个不需要做讲解的噢。

[php]

//缓存文件处理
ob_start();
echo '&lt;?php'.&quot;\n&quot;;
//方法1：自己根据数据结构生成输出的结构体
echo '$LinksOBJ = array('.&quot;\n&quot;;
foreach($Links as $k3=&gt;$v3){
    echo '    &quot;'.$k3.'&quot;=&gt;array('.&quot;\n&quot;;
    foreach($v3 as $k4=&gt;$v4){
        echo '        &quot;'.$k4.'&quot;=&gt;'.(is_numeric($v4)?$v4:'&quot;'.$v4.'&quot;').&quot;,\n&quot;;
    }
    echo '    ),'.&quot;\n&quot;;
}
echo ');'.&quot;\n&quot;;
//方法2：使用内置方法var_export做输出
echo '$LinksOBJ = ';
echo var_export($Links,false);
echo ';'.&quot;\n&quot;;
//
$data = ob_get_contents();
ob_end_clean();
file_put_contents($filename,$data) or die('ERROR');

[/php]