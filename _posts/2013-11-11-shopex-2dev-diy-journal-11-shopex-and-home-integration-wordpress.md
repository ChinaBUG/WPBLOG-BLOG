---
ID: 2782
post_title: >
  ShopEX二次开发DIY日记11之shopex和wordpress的首页整合
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  哎，又是今天论坛看到的，这个需求有够奇葩，参考网站的效果有够丑，不知道为什么要这么弄？！
  下面是在猪八戒上的需求
  具体要求：
  shopex和wordpress的首页整合，仅仅是首页的整合，其他的页面都不需要整合，进入首页点击shopex的链接 出现的是单独的shopex页面，点击wordpress上的链接，出现的是wordpress中的内容，首页的页面最上方是shopex，最下方是 wordpress不要框架，不要用什么插件引用之类的，整合后达到的效果看http://www.meiliwang.org，现在http://www.meiliwang.org最上方只是shopex的图片，能做到的联系我
  看完之后我第一想法是，这个需求很简单，不知道为什么要这么做，难道是为了SEO的需要？但是，看了参考站，哇咧，想吐了~难道发布者是挣IP数？好吧，不管了，分析一下顺便解决吧~
  思路分析：
  从需求跟案例来看，要完成这个需求的话需要懂两个要点：
  1、ShopEX的主页代码要怎么确定。
  因为他说了只需要首页，而且不能使用框架，那就只有程序直接输出了呗，而且不能是直接把代码加在index.php入口文件内，因为这样子的效果，还有判断会比较麻烦。
  2、Wordpress程序怎么在主页上调用数据。
  这个就涉及到wordpress的主页设计了加上API的调用了，我正好懂，我的主页就是这样子做的，给大家看看www.ipodmp.com哈，我懂了，也就会让你也懂对吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-11-shopex-and-home-integration-wordpress.html
published: true
post_date: 2013-11-11 17:45:40
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

哎，又是今天论坛看到的，这个需求有够奇葩，参考网站的效果有够丑，不知道为什么要这么弄？！

下面是在猪八戒上的需求
<blockquote>具体要求：
shopex和wordpress的首页整合，仅仅是首页的整合，其他的页面都不需要整合，进入首页点击shopex的链接 出现的是单独的shopex页面，点击wordpress上的链接，出现的是wordpress中的内容，首页的页面最上方是shopex，最下方是 wordpress不要框架，不要用什么插件引用之类的，整合后达到的效果看http://www.meiliwang.org，现在http://www.meiliwang.org最上方只是shopex的图片，能做到的联系我</blockquote>
看完之后我第一想法是，这个需求很简单，不知道为什么要这么做，难道是为了SEO的需要？但是，看了参考站，哇咧，想吐了~难道发布者是挣IP数？好吧，不管了，分析一下顺便解决吧~

思路分析：

从需求跟案例来看，要完成这个需求的话需要懂两个要点：

1、ShopEX的主页代码要怎么确定。

因为他说了只需要首页，而且不能使用框架，那就只有程序直接输出了呗，而且不能是直接把代码加在index.php入口文件内，因为这样子的效果，还有判断会比较麻烦。

2、Wordpress程序怎么在主页上调用数据。

这个就涉及到wordpress的主页设计了加上API的调用了，我正好懂，我的主页就是这样子做的，给大家看看www.ipodmp.com哈，我懂了，也就会让你也懂对吧。

~.-.~   ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~  ~.-.~

上面的两点，我先来说明第二点吧，毕竟wordpress比较简单，因为它本身就提供了API接口调用了，只要懂得调用，就不需要太多废话的。
<pre>&lt;?php
    define('WP_USE_THEMES', false);
    require('blog/wp-load.php');
?&gt;</pre>
在主页头部增加一下引用，注意wp-load.php在哪里就怎么引用噢，可不要错了，我这边是二级blog目录内的。

然后就可以查询我们需要的文章了噢，具体如下：
<pre>    &lt;?php query_posts('showposts=50'); ?&gt;
    &lt;?php if (have_posts()) : ?&gt;
        &lt;?php while (have_posts()) : the_post(); ?&gt;
            &lt;li&gt;
            &lt;div&gt;
            &lt;a href="&lt;?php the_permalink() ?&gt;" rel="bookmark" title="&lt;?php the_title(); ?&gt;"&gt;&lt;?php the_title('&lt;span&gt;','&lt;/span&gt;')?&gt;&lt;/a&gt;
            &lt;/div&gt;
            &lt;div&gt;
            &lt;?php the_excerpt();?&gt;
            &lt;/div&gt;
            &lt;/li&gt;
        &lt;?php endwhile; ?&gt;
    &lt;?php endif;?&gt;</pre>
参考资料：<a href="http://codex.wordpress.org/zh-cn:%E5%87%BD%E6%95%B0%E5%8F%82%E8%80%83/query_posts">函数参考/query posts</a>

其他的语法都是跟做模板时是一样的，没差别，就是多了一步要引用。

按照这个需求来说，完美一点的话，最好将wordpress给打包成一个博客挂件，然后在ShopEX里面调用，这样子，绝对能做到在主页，而且可以省事，还能搞得很好看，要是按照客户的要求的话，那就惨不忍睹，没办法轻松呀，只能是继续下一步的修改了。

从上面的思路分析，我们需要判断目前是不是在主页，是在主页就要输出博客内容，那么很明显就是需要去修改ShopEX的内核，要不是没办法借用他里面的方法来做判断的，我们也只能通过网址来判断了，这个相当麻烦。

那么ShopEX的内核加密了，怎么办？

没关系~ShopEX支持二次开发，这个是全部商城系统中，令我唯一比较满意的地方，虽然他的二次开发经常会奔溃。

请先在config/config.php里面定义：

define('CUSTOM_CORE_DIR', BASE_DIR.'/core_agri');

然后新建core_agri目录，进入再新建include目录（core_agri\include），然后新建一文本文件，名为：customCore.php，然后打开文件，录入如下代码：
<pre>&lt;?php
require_once(CORE_DIR.'/func_ext.php');
require_once(CORE_INCLUDE_DIR.'/shopCore.php');
//
class customCore extends shopCore{
    function customCore(){
        parent::shopCore();        
    }
}
?&gt;</pre>
然后要去修改一下入口文件噢，具体修改代码如下：

先找到new shopCore()所在的地方按照实际情况做修改，下面的仅供参考：
<pre>if (file_exists(CUSTOM_CORE_DIR.'/include/customCore.php')){
    require(CUSTOM_CORE_DIR.'/include/customCore.php');
    new customCore();
}else{
    require(CORE_INCLUDE_DIR.'/shopCore.php');
    new shopCore();
}</pre>
接着，打开商城，你会发现可以运行噢，然后我们来测试一下我们修改的是不是有效果。

请打开customCore.php找到 function customCore(){}方法，在里面写一个输出吧~随便些什么都可以，然后你打开商城，你会发现，你写的文字显示出来了。

然后我们就来判断一下当前是主页还是什么页面噢。新建parseRequest()方法
<pre>function parseRequest(){
    $request= parent::parseRequest();
    ......
}</pre>
然后省略号的就自己写吧。

不懂？请先输出$request看看它的值，你就知道怎么去判断了，多判断几次噢。

拓展：

其实，这个需求真的不难，就是发布者搞得很复杂，复杂算了，只要好看也就无所谓了，结果，还很难看。可怜的孩子~~~~500元飞走了~~~~