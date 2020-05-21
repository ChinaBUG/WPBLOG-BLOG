---
ID: 2520
post_title: >
  PHP-写一个迷你版Smarty模板引擎，对认识模板引擎原理非常好
author: ChinaBUG
post_excerpt: |
  前些时间在看 创智博客韩顺平的Smarty模板引擎教程，再结合自己跟李炎恢第二季开发中CMS系统写的tpl模板引擎。今天就写一个迷你版的Smarty引擎，虽然 说我并没有深入分析过Smarty的源码，但是对模板引擎的原理，还是有深刻的理解的。如果有什么还需要改进的地方，记得提出来。
  一、什么是Smarty模板引擎：
  Smarty是一个使用PHP写出来的模板引擎，是目前业界最著名的PHP模板引擎之一。它分离了逻辑代码和外在的内容，提供了一种易于管理和使用的方法，用来将原本与HTML代码混杂在一起PHP代码逻辑分离。简单的讲，目的就是要使PHP程序员同前端人员分离，使程序员改变程序的逻辑内容不会影响到前端人员的页面设计，前端人员重新修改页面不会影响到程序的程序逻辑，这在多人合作的项目中显的尤为重要。（来自百度百科）
  自己的理解是：
  第一，有利于把前端开发与后台开发工作分离，利于分工合作；
  第二，其缓存机制，有利于加快网站的访问速度；
  第三，模板标签还可以一次编写，到处调用，便利和简洁性好；
  ......
layout: post
permalink: >
  http://blog.ipodmp.com/2013/07/php-write-a-mini-smarty-template-engine.html
published: true
post_date: 2013-07-30 15:00:46
---
虫曰：今天看到此文觉得非常不错，对于认识模板引擎原理真的有帮助，很清晰，完整的将一个模板引擎的工作原理给写出来了，转摘与大家分享之。

&nbsp;
<div id="cnblogs_post_body">　　前些时间在看 创智博客韩顺平的Smarty模板引擎教程，再结合自己跟李炎恢第二季开发中CMS系统写的tpl模板引擎。今天就写一个迷你版的Smarty引擎，虽然 说我并没有深入分析过Smarty的源码，但是对模板引擎的原理，还是有深刻的理解的。如果有什么还需要改进的地方，记得提出来。<strong>一、什么是Smarty模板引擎：</strong>

Smarty是一个使用PHP写出来的模板引擎，是目前业界最著名的PHP模板引擎之一。它分离了逻辑代码和外在的内容，提供了一种易于管理和使用的方法，用来将原本与HTML代码混杂在一起PHP代码逻辑分离。简单的讲，目的就是要使PHP程序员同前端人员分离，使程序员改变程序的逻辑内容不会影响到前端人员的页面设计，前端人员重新修改页面不会影响到程序的程序逻辑，这在多人合作的项目中显的尤为重要。（来自百度百科）

&nbsp;
<div>

自己的理解是：

第一，有利于把前端开发与后台开发工作分离，利于分工合作；

第二，其缓存机制，有利于加快网站的访问速度；

第三，模板标签还可以一次编写，到处调用，便利和简洁性好；

&nbsp;

二、下面就一起开发迷你版模板引擎吧

（1）首先先把已经做好的模板引擎给大家看一看，先使用，再开发

① 迷你版Smarty模板引擎目录结构如下：

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13204543-4982e996a23d403792196dd5dccd9183.png" />

&nbsp;

源代码里面有很详细的说明，请看下面

&nbsp;

① 要开发一个模板引擎，最主要的有两个类，分别是模板引擎入口类和模板解析类。

A.首先创建MiniSmarty目录，然后新建一个文件名为MiniSmarty.class.php

其代码如下：
<div>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
<pre>/**
 * MiniSmarty模板引擎
 * @link http://www.cnblogs.com/isuhua/
 * @author 华仔_suhua &lt;<a href="http://weibo.com/suhua123" target="_blank">weibo.com/suhua123</a>&gt;
 * @package MiniSmarty
 * @version 0.0.0.1
 */
class MiniSmarty {
    //模板文件
    public $template_dir = 'templates';
    //编译文件
    public $compile_dir = 'templates_c';
    //缓存文件
    public $cache_dir = 'cache';
    //模板变量
    public $_tpl_var = array();
    //是否开启缓存
    public $caching = false;

    public function __construct() {
        $this-&gt;checkDir();
    }

    //检查目录是否建好
    private function checkDir() {
        if (!is_dir($this-&gt;template_dir)) {
            exit('模板文件目录templates不存在！请手动创建');
        }
        if (!is_dir($this-&gt;compile_dir)) {
            exit('编译文件目录templates_c不存在！请手工创建！');
        }
        if (!is_dir($this-&gt;cache_dir)) {
            exit('缓存文件目录'.$this-&gt;cache_dir.'不存在！请手工创建！');
        }
    }

    //模板变量注入方法
    public function assign($tpl_var, $var = null) {
        if (isset($tpl_var) &amp;&amp; !empty($tpl_var)) {
            $this-&gt;_tpl_var[$tpl_var] = $var;
        } else {
            exit('模板变量名没有设置好');
        }
    }

    //文件编译
    public function display($file) {
        //模板文件
        $tpl_file  = $this-&gt;template_dir.'/'.$file;
        if (!file_exists($tpl_file)) {
            exit('ERROR:模板文件不存在！');
        }
        //编译文件
        $parse_file = $this-&gt;compile_dir.'/'.md5($file).$file.'.php';

        //只有当编译文件不存在或者是模板文件被修改过了
        //才重新编译文件
        if (!file_exists($parse_file) || filemtime($parse_file) &lt; filemtime($tpl_file)) {
            include 'smarty_compile.class.php';
            $compile = new Smarty_Compile($tpl_file);
            $compile-&gt;parse($parse_file);
        }

        //开启了缓存才加载缓存文件，否则直接加载编译文件
        if ($this-&gt;caching) {
            //缓存文件
            $cache_file = $this-&gt;cache_dir.'/'.md5($file).$file.'.html';
            //只有当缓存文件不存在，或者编译文件已被修改过
            //重新生成缓存文件
            if (!file_exists($cache_file) || filemtime($cache_file) &lt; filemtime($parse_file)) {
                //引入缓存文件
                include $parse_file;
                //缓存内容
                $content = ob_get_clean();
                //生成缓存文件
                if (!file_put_contents($cache_file, $content)) {
                    exit('缓存文件生成出错！');
                }
            }
            //载入缓存文件
            include $cache_file;
        } else {
            //载入编译文件
            include $parse_file;
        }
    }
}</pre>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
</div>
&nbsp;

B.然后再新建一个MiniSmarty模板引擎解析器类文件：MiniSmarty_Compile.class.php

其代码如下：
<div>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
<pre>&lt;?php
/**
 * MiniSmarty模板引擎
 * @link http://www.cnblogs.com/isuhua/
 * @author 华仔_suhua &lt;<a href="http://weibo.com/suhua123" target="_blank">weibo.com/suhua123</a>&gt;
 * @package MiniSmarty
 * @version 0.0.0.1
 */
class MiniSmarty_Compile {
    //模板内容
    private $content = '';

    //构造函数
    public function __construct($tpl_file) {
        $this-&gt;content = file_get_contents($tpl_file);
    }

    //解析普通变量，如把{$name}解析成$this-&gt;_tpl_var['name']
    public function parseVar() {
        $pattern = '/\{\$([\w\d]+)\}/';
        if (preg_match($pattern, $this-&gt;content)) {
            $this-&gt;content = preg_replace($pattern, '&lt;?php echo \$this-&gt;_tpl_var["$1"]?&gt;', $this-&gt;content);
        }
    }

    //这里可以自定义其他解析器...

    //模板编译
    public function parse($parse_file) {
        //调用普通变量解析器
        $this-&gt;parseVar();
        //这里可以调用其他解析器...

        //编译完成后，生成编译文件
        if (!file_put_contents($parse_file, $this-&gt;content)) {
            exit('编译文件生成出错！');
        }
    }
}
?&gt;</pre>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
</div>
&nbsp;

C.最后，还必须新建几个目录，分别是模板文件目录templates、编译文件目录 template_c、缓存文件目录cache。

如果你(ˇˍˇ） 想～一次性成功，就必须创建这几个目录，缺一不可。否则就会报错，然后要求你手动创建。

&nbsp;

D.来试试看吧，编写demo.php，测试一下自定义的迷你版MiniSmarty模板引擎吧！

demo.php代码如下：
<div>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
<pre>    //引入模板引擎
    require 'MiniSmarty.class.php';
    //实例化模板类
    $minismarty = new MiniSmarty();
    //缓存开关
    $minismarty-&gt;caching = true;

    //定义变量
    $webname = '迷你版Smarty测试';
    $author = 'suhua';
    $title = '这是一个测试标题';
    $content = '这是一段测试内容';

    //注入变量
    $minismarty-&gt;assign('webname', $webname);
    $minismarty-&gt;assign('author', $author);
    $minismarty-&gt;assign('title', $title);
    $minismarty-&gt;assign('content', $content);

    //启动编译模板文件
    $minismarty-&gt;display('demo.tpl');</pre>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
</div>
&nbsp;

<strong>测试前，</strong>请先看一下template、template_c 以及cache目录各自的状态，请看下图：

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13211549-de5712c38ac6418c9b278c64cbd01c44.png" />

&nbsp;

E：打开浏览器，输入http://localhost/MiniSmarty/demo.php，即可看到一下效果：

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13205503-48c19953d2df4cd3944c6cc73664559c.png" />

&nbsp;

<strong>测试后，</strong>请再次看一下各目录的状态：

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13211907-bd58ed9b09d641f2ab8627869a4357cc.png" />

在template_c目录和cache目录下都分别多了一个xxxx.tpl.php和xxx.tpl.html文件，为什么呢？

答：这就是模板引擎非常重要的一个作用，编译文件并生成静态文件。对于如何实现的，这里不做解析，源码已经给出，看看就懂。

&nbsp;

----&gt;至此表示自己开发的一个迷你版Smarty模板引擎成功！*\(^v^)/*

&nbsp;

<strong>再次测试</strong>一下缓存功能是否生效了，首先修改demo.php中的代码，改动如下：
<div>
<pre>把 $author = 'suhua'; 修改为 $author = 'xiwang';</pre>
</div>
改动过后，记得保存，然后再次刷新页面，看出现什么状况了？

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13205503-48c19953d2df4cd3944c6cc73664559c.png" />

&nbsp;

<strong>结果是：</strong>没有任何变化！这就正常了否则缓存功能没有实现。

？？？为什么没有任何变化呢

答：原因是我们在demo.php中开启了缓存功能
<div>
<pre>//缓存开关
$minismarty-&gt;caching = true;</pre>
</div>
请看代码。下面这段代码是MiniSmarty.class.php里面的，下面就是根据你是否开启缓存，决定是加载缓存文件还是编译文件。因为这里开启了，所以会直接加载缓存文件，所以就算你修改了原来的模板，依然没变化。
<div>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
<pre>        //开启了缓存才加载缓存文件，否则直接加载编译文件
        if ($this-&gt;caching) {
            //缓存文件
            $cache_file = $this-&gt;cache_dir.'/'.md5($file).$file.'.html';
            //只有当缓存文件不存在，或者编译文件已被修改过
            //重新生成缓存文件
            if (!file_exists($cache_file) || filemtime($cache_file) &lt; filemtime($parse_file)) {
                //引入缓存文件
                include $parse_file;
                //缓存内容
                $content = ob_get_clean();
                //生成缓存文件
                if (!file_put_contents($cache_file, $content)) {
                    exit('缓存文件生成出错！');
                }
            }
            //载入缓存文件
            include $cache_file;
        } else {
            //载入编译文件
            include $parse_file;
        }</pre>
<div><a title="复制代码"><img alt="复制代码" src="http://common.cnblogs.com/images/copycode.gif" /></a></div>
</div>
如果我在demo.php中把缓存功能关了呢，结果会如何？

即改动如下：
<div>
<pre>//缓存开关
 $minismarty-&gt;caching = false; //关闭缓存功能</pre>
</div>
&nbsp;

此时，当你再次刷新页面的时候，你会看到如下效果：

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13214043-1690879586104b048c9af3d5552e7f18.png" />

此时，作者一栏改变了，这说明了模板引擎此时并没有去加载缓存文件，而是直接加载了编译文件。所以会出现该效果。上面的代码也说明了这一点。

&nbsp;

其他细节在源码中都有较详细的注释，在这里就不多说了，说一下其原理。

&nbsp;

&nbsp;

★ MiniSmarty模板引擎原理：（非常重要）

其原理也比较简单

① 首先模板引擎会加载模板文件templates/demo.tpl，然后调用模板编译类对其进行编译解析（说白了就是变量替换或者标签替换），编译后就会生成编译文件xxxx.tpl.php；

② 然后判断缓存是否开启，来决定是否生成缓存文件。其生成过程是：直接把xxx.tpl.php编译文件加载进来，然后再从缓冲区取出所有内容，清空缓冲区，把内容写入到缓存文件中xxx.tpl.html文件。

③ 模板文件demo.tpl是一个同时具有html和引擎标签的复合文件，编译文件xxx.tpl.php是把引擎标签替换成php代码，是具有php和html标签的复合文件，缓存文件xxx.tpl.html文件就是一个纯html的静态文件；

<img alt="" src="http://images.cnitblog.com/blog/466624/201304/13221049-e5fdc5bdd8094480973bd58088907a8c.png" />

&nbsp;

至此，迷你版MiniSmarty模板引擎开发完成！

其中，如果还有其他不好的地方，希望各位指出！谢谢。

&nbsp;

最后附上：<a href="http://files.cnblogs.com/isuhua/MiniSmarty.rar">MiniSmarty源码.rar</a> (点击即可下载)

&nbsp;

</div>
</div>
<div id="MySignature">学而时习之，不亦说乎？有朋自远方来，不亦乐乎？*\(^v^)/*</div>