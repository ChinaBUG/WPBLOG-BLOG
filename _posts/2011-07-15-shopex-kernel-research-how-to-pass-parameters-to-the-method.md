---
ID: 1638
post_title: >
  密码保护：ShopEX-内核程序研究-如何在方法之间传递参数的？
author: ChinaBUG
post_excerpt: |
  在做ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)二次开发时，不懂得怎么获取方法参数值，思考了一下，既然这些cct/ctl文件都是由ShopEX自动载入，那么肯定有存在一个机制，如何调用，那么必然有如何保存方法参数的办法。
  一开始，我猜想这个保存方法参数的途径，可能将各个方法与相应的参数保存在那个文件里面，需要时直接对比获取吧，按照这个思路，我调整了一下方法index的参数顺序（在这边以ctl.product.php文件的index方法为例说明）。
  结果，发现这个参数的顺序改变了，但是神奇的，第一个参数的位置的值还是照旧不变的。那么猜想有个文件参照的想法是错误的，怎么办了？
  没什么想法，还是乖乖的从底层开始研究起吧，根据反编译的文件，终于看到了一点点的来历~！
  具体查找过程就不写了，直接上代码吧
  $this->system = $GLOBALS['system'];
  $gid = $this->system->request['action']['args'][0];
  我们需要的东西都是保存在这个全局对象里面了~
  根据输出的文件跟踪，我们知道需要的参数值就是['args']段，只要我们取出来即可。
  同样的这个全局对象里面好东西很多，只要你想要的都能找得到哈。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-kernel-research-how-to-pass-parameters-to-the-method.html
published: true
post_date: 2011-07-15 20:30:27
---
在做<a title="ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)" href="http://blog.ipodmp.com/archives/shopex-2-dev-ctl-product-php-2-cct-product-php/">ShopEX-二次开发-ctl.product.php能不能二次开发(cct.product.php能否正常运行)</a>二次开发时，不懂得怎么获取方法参数值，思考了一下，既然这些cct/ctl文件都是由ShopEX自动载入，那么肯定有存在一个机制，如何调用，那么必然有如何保存方法参数的办法。

一开始，我猜想这个保存方法参数的途径，可能将各个方法与相应的参数保存在那个文件里面，需要时直接对比获取吧，按照这个思路，我调整了一下方法index的参数顺序（在这边以ctl.product.php文件的index方法为例说明）。

结果，发现这个参数的顺序改变了，但是神奇的，第一个参数的位置的值还是照旧不变的。那么猜想有个文件参照的想法是错误的，怎么办了？

没什么想法，还是乖乖的从底层开始研究起吧，根据反编译的文件，终于看到了一点点的来历~！

具体查找过程就不写了，直接上代码吧

$this-&gt;system = $GLOBALS['system'];
$gid = $this-&gt;system-&gt;request['action']['args'][0];

我们需要的东西都是保存在这个全局对象里面了~

根据输出的文件跟踪，我们知道需要的参数值就是['args']段，只要我们取出来即可。

同样的这个全局对象里面好东西很多，只要你想要的都能找得到哈。