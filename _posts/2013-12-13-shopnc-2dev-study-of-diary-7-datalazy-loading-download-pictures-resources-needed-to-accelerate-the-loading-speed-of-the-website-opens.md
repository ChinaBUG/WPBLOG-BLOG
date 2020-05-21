---
ID: 3053
post_title: >
  ShopNC二次开发研究日记7:懒加载-按需下载图片资源加快网站打开载入速度
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  现在的懒加载技术真的是应用的很泛滥，但是ShopNC的首页竟然没有，但是，我们进入商品列表页会发现ShopNC其实是有懒加载的，那么我们能不能在首页上使用呢？
  答案是肯定的，要不就不会写这个日记啦~
  DIY开始了~修改一个文件实现懒加载噢
  请找到templates/default/home/index.php文件，然后搜索<?php echo $output['web_html'];?>这行字符串，然后修改为下面的代码即可，轻松加愉快呀~~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopnc-2dev-study-of-diary-7-datalazy-loading-download-pictures-resources-needed-to-accelerate-the-loading-speed-of-the-website-opens.html
published: true
post_date: 2013-12-13 10:08:43
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

现在的懒加载技术真的是应用的很泛滥，但是ShopNC的首页竟然没有，但是，我们进入商品列表页会发现ShopNC其实是有懒加载的，那么我们能不能在首页上使用呢？

答案是肯定的，要不就不会写这个日记啦~

DIY开始了~修改一个文件实现懒加载噢

请找到templates/default/home/index.php文件，然后搜索&lt;?php echo $output['web_html'];?&gt;这行字符串，然后修改为下面的代码即可，轻松加愉快呀~~
<pre>    &lt;!--ChinaBUG 2013.12.13--&gt;
    &lt;script src="&lt;?php echo RESOURCE_PATH;?&gt;/js/jquery.datalazyload.js" type="text/javascript"&gt;&lt;/script&gt;
    &lt;div id="lazyload"&gt;
        &lt;textarea style="display: none;"  <span style="color: #ff0000;">class="text-lazyload"</span>&gt;
            &lt;?php echo $output['web_html'];?&gt;
        &lt;/textarea&gt;
    &lt;/div&gt;
    &lt;script&gt;
    $(function(){if($("#lazyload").length&gt;0){$('#lazyload').datalazyload({dataItem: '.nc-home-pattern', loadType: 'item', effect: 'fadeIn', effectTime: 1000 });}});
    &lt;/script&gt;
    &lt;!--ChinaBUG 2013.12.13--&gt;</pre>
保存之后执行以下首页你会发现，哈哈哈~~懒加载的效果出来了噢~~~

下面附录是datalazyload插件的信息，有兴趣研究这个插件开发的可以去看~单独想要测试这个功能到其他系统中的朋友也可以下载。

修订：

2013.12.22 写博文时漏了一个参数，是的按照上面的代码使用会提示'请先包装延迟加载的容器.'，解决办法是增加一个类名即可，内容为 class="text-lazyload"，具体加载上方代码的红线处。

附录：

作者博客：<a href="http://www.cnblogs.com/liping13599168/">Leepy's Blogs</a>

脚本来源：<a href="http://www.cnblogs.com/liping13599168/archive/2010/05/16/1736620.html">【项目分析】设计一种前端数据延迟加载的jQuery插件(2)</a>

云盘下载：<a href="http://url.cn/T9gJIq">微云</a>