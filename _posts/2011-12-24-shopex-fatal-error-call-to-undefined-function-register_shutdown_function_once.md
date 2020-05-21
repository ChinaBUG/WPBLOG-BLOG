---
ID: 1875
post_title: 'ShopEX-Fatal error: Call to undefined function register_shutdown_function_once()'
author: ChinaBUG
post_excerpt: |
  Fatal error: Call to undefined function register_shutdown_function_once() in D:\PHPnow\htdocs\core\include_v5\AloneDB.php on line 192
  最近为了设计定时调价的功能，对底层的操作做了好多的修改于研究，在办公环境没什么问题，但是今天，平安夜，靠，一点也不平安，拷贝回家的代码尽然不能使用，出现上面显示的提示了。
  经过查找register_shutdown_function_once() 所在的文件func_ext.php与kernel.php文件中，不应该在AloneDB.php会出现错误噢，因为在AloneDB.php里面192行压根就没看到调用这个方法的地方，相应的在231行有在调用。
  怎么还是会出现这个问题呢？
  不知道，那就替换吧，把原版的代码覆盖，问题解决。
  后面经过查找，才发觉一个问题所在，我把一段代码调用提前了，就出现这个情况了噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/12/shopex-fatal-error-call-to-undefined-function-register_shutdown_function_once.html
published: true
post_date: 2011-12-24 23:30:27
---
<strong>Fatal error</strong>: Call to undefined function register_shutdown_function_once() in <strong>D:\PHPnow\htdocs\core\include_v5\AloneDB.php</strong> on line <strong>192</strong>

最近为了设计定时调价的功能，对底层的操作做了好多的修改于研究，在办公环境没什么问题，但是今天，平安夜，靠，一点也不平安，拷贝回家的代码尽然不能使用，出现上面显示的提示了。

经过查找register_shutdown_function_once() 所在的文件func_ext.php与kernel.php文件中，不应该在<strong>AloneDB.php</strong>会出现错误噢，因为在AloneDB.php里面192行压根就没看到调用这个方法的地方，相应的在231行有在调用。

怎么还是会出现这个问题呢？

不知道，那就替换吧，把原版的代码覆盖，问题解决。

后面经过查找，才发觉一个问题所在，我把一段代码调用提前了，就出现这个情况了噢。

具体的出错代码如下：

在kernel.php中：
$this-&gt;__metadata = unserialize(file_get_contents(HOME_DIR.'/fdata.php'));
if(isset($this-&gt;__metadata['GTASK_REMINDER']) &amp;&amp; $this-&gt;__metadata['GTASK_REMINDER']&gt;0 &amp;&amp; time()&gt;$this-&gt;__metadata['GTASK_REMINDER']){
$goods = &amp;$this-&gt;loadModel('trading/goods');
$goods-&gt;flush_gtask();
}

我仅仅是将if内的代码给移到if外面噢，形成如下代码：

$this-&gt;__metadata = unserialize(file_get_contents(HOME_DIR.'/fdata.php'));
if(isset($this-&gt;__metadata['GTASK_REMINDER']) &amp;&amp; $this-&gt;__metadata['GTASK_REMINDER']&gt;0 &amp;&amp; time()&gt;$this-&gt;__metadata['GTASK_REMINDER']){
$goods = &amp;$this-&gt;loadModel('trading/goods');
$goods-&gt;flush_gtask();
}

$goods = &amp;$this-&gt;loadModel('trading/goods');
$goods-&gt;flush_gtask();

就是上面的，结果就出现错误了，恩，错误的可能是我的定时作用的功能未开，而去调用吧，谁也不知道哈~~反正从这个得知SHOPEX的定时上下架存在问题。希望有解决方案吧。

PS，昨天查统计，顺便也查了一下这个案例的相关文章，发现，官方博客那边有解决方案，下载回来发现，修改了里面的几行代码哈，在231行判断是不是存在register_shutdown_function_once()方法，不存在则包含，代码如下：

if ( !function_exists( "register_shutdown_function_once" ) ) {
require_once( CORE_DIR."/func_ext.php" );
}

&nbsp;

2012.04.23补充：

不知道最近又是什么症状，本地的ShopEX商城每次打开都会出现这个提示，最诡异的是，已登录后台之后就又正常了~~还真是够神奇的。