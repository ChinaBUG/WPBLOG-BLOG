---
ID: 2570
post_title: 'ShopEX-前台出现产品链接图片地址修改多了多级目录的链接乱码怎么解决(&#8230;/index.php/&#8230;)？'
author: ChinaBUG
post_excerpt: |
  在使用ShopEX的过程中，我们有时候就会出现一个很神奇的问题，那就是会发现，你的shopex的主页里面的链接、图片等资源的地址都变成一个莫名其妙的地址，然后图片显示不了，链接打开不了。这种情况出现较多的是开启了伪静态之后才发生的。
  主要特征就是在正常的链接地址上增加如下莫名其妙的字符：
  如我的商城站http://shopex326.ipodmp.com/product-72.html着个是正确的地址，但是会随机加上一个莫名其妙的字符，变成如http://shopex326.ipodmp.com/index.php/guest/huang/1/cn/11061/images/goods/20130116/product-72.html这样子的链接，而图片的话http://shopex326.ipodmp.com/images/goods/20130917/69a64a71f524342a.jpg就会变成http://shopex326.ipodmp.com/index.php/guest/huang/1/cn/11061/css/css/plugins/widgets/duceflash/images/images/goods/20130124/images/goods/20130917/69a64a71f524342a.jpg。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/09/shopex-front-address-product-images-appear-more-multi-level-directory-of-links-garbled-how-to-solve.html
published: true
post_date: 2013-09-27 11:08:45
---
在使用ShopEX的过程中，我们有时候就会出现一个很神奇的问题，那就是会发现，你的shopex的主页里面的链接、图片等资源的地址都变成一个莫名其妙的地址，然后图片显示不了，链接打开不了。这种情况出现较多的是开启了伪静态之后才发生的。

而且，最最神奇的在于，这个链接乱码问题居然只发生在主页index.php上，而内页的都没受影响！

主要特征就是在正常的链接地址上增加如下莫名其妙的字符：

如我的商城站http://shopex326.ipodmp.com/product-72.html着个是正确的地址，但是会随机加上一个莫名其妙的字符，变成如

http://shopex326.ipodmp.com/index.php/guest/huang/1/cn/11061/images/goods/20130116/product-72.html

这样子的链接，而图片的话http://shopex326.ipodmp.com/images/goods/20130917/69a64a71f524342a.jpg就会变成

http://shopex326.ipodmp.com/index.php/guest/huang/1/cn/11061/css/css/plugins/widgets/duceflash/images/images/goods/20130124/images/goods/20130917/69a64a71f524342a.jpg。

当然，上面这个链接不是唯一的，有时是这样子有时又是另外一种样子。神奇不？！

附录：相关问题的一些文章介绍
<ol>
	<li><a href="http://www.hnqss.cn/help/help-122.html">ShopEx后台不能上传图片的原因分析</a>：清风的文章都是不错的，值得大家都去看看的。这个文章分析了文件不能上传的原因，虽然跟本文关联不大，但里面对问题的分析还是值得参考的。</li>
	<li><a href="http://www.wisge.com/Article/detail/fid/53/id/20.html">shopex网址路径自动添加了index.php/是什么意思</a>：为了案例查找到的网站，不过里面的下载包无效</li>
</ol>
下面是我自己站出现的部分字符串：

'index.php/guest/huang/1/cn/11061/images/goods/20130116/'
'index.php/guest/huang/1/cn/11061/css/css/plugins/widgets/duceflash/images/images/goods/20130124/'
'index.php/guest/huang/1/cn/11061/css/css/plugins/widgets/duceflash_g/images/'
'index.php/guest/huang/1/cn/11061/css/css/images/goods/20130121/plugins/widgets/duceflash_g/images/url/'
'index.php/images/gift/20130225/plugins/widgets/duceflash/images/plugins/widgets/duceflash/images/'
'index.php/images/gift/20130225/plugins/widgets/duceflash/images/url/images/link/20130427/'

补充：

2013.10.14 http://shopex326.ipodmp.com/index.php/guest/huang/1/cn/11061/css/css/plugins/widgets/duceflash/images/images/goods/20130124/images/goods/20130917/product-30.html

下面是网上其他网友出现的字符串：
<ol>
	<li><a href="http://bbs.shopex.cn/read.php?tid-311889.html">shopex485网站总是隔一段时间网页图片就打不开了</a>
http://shopex326.ipodmp.com/index.php/archives/date/2006/product-1124.html</li>
	<li><a href="http://bbs.shopex.cn/read.php?tid-256582.html">[使用]SHOPEX 伪静态地址问题(/index.php/statics/)？</a>
http://shopex326.ipodmp.com/index.php/statics/script/...</li>
	<li><a href="http://bbs.shopex.cn/read.php?tid-306820.html">[使用]shopex商品连接成了：http://www.ABC.com/index.php/&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;/product-313.html
</a>http://shopex326.ipodmp.com/index.php/&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;/product-313.html</li>
	<li><a href="http://hubeidc.com/idc/201212/18490.html">华夏名网shopex主机出现fcgi-bin，导致网站打开错位的解决办法</a>
http://shopex326.ipodmp.com/fcgi-bin/themes/weiyi/images/css.css</li>
</ol>
解决方法：

我在网上找了很多的文章，按照很多的教程照做，结果是，一个问题也没解决！当然，我是不知道是他们的方法错误还是我的运气背，别人能解决的为什么我解决不了呢？还是造成的问题的原因不同？

所以，在老大的督促之下，我找呀找呀~~想呀想呀~~试呀试呀~~

然后终于找到办法了噢~

其实解决方法很简单，只要在我们的kernel.php文件内base_url方法里面返回我们自己正确的地址即可！

比如我的商城我则修改为：

$this-&gt;_base_url = 'http://shopex326.ipodmp.com/';
return 'http://shopex326.ipodmp.com/';

这样子保存之后就可以解决问题了~唯一不好的就是你的系统不能乱移动，这个硬编码下去就是全部的链接都是会变成这边指定的网址为开头的链接了~

聪明的小伙伴也是可以自己做一个自动化的呗~留给大家自己尝试吧~

PS：这个办法可以用于测试站之类的应用，服务器是我的，图片却是别人的~~哈
<blockquote>技术啐啐念：其实这个问题出现的不算很广泛，但是总是有一部分人遇上，而且最诡异的大部分是开启伪静态之后才发生，那么我们大家都知道这个明显是伪静态的问题的概率比较高，但是处理起来却很麻烦。我之前是怀疑因为伪静态开启，造成缓存文件的信息匹配错误，所以造成大家去清除缓存可以临时解决这个问题，一担缓存文件超过一定的容量之后问题又会出现了。但是，想想又不对，缓存是无辜的的，因为缓存内的代码怎么来的？程序自动输出的！如果程序不输出这些信息，那么缓存肯定也不会自作主张的显示这些信息了，这个就能很好的解决，为什么出现的那些乱七八糟的乱码挺有一定规律的，基本上都是网站内存在的一些文件名，目录名之类的！其实也是这个规律害的我之前判断是缓存可能存在问题，因为我把相关的文件名，目录名都给修改了，结果问题还是照旧！如果是程序输出的话，那么程序在哪里控制这个输出内容？又为什么会显示那几个特定的，如果出现这类乱码链接的，仔细看会发现，虽然会变化，但是变来变去，那些乱七八糟的东西其实挺固定的，能找到原因所在？不能！因为搜索之后你肯定是找不到这些东西的，除了缓存文件里面！最后分析到没办法，发觉出现问题的基本上都是跟base_url方法都有或多或少的联系，剩下的你就知道了，不断的尝试，终于解决这个问题。</blockquote>
目前按照这个思路修改之后，之前是三四天会发生一次的问题得到解决，从2013.09.18 17.55到目前~接近两个月的时间里，问题不在重复发作！