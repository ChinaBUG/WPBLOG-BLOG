---
ID: 2984
post_title: >
  WEB-怎么设计精美的视差效果网页效果
author: ChinaBUG
post_excerpt: |
  对于视差滚动(Parallax Scrolling)，关注网页设计的朋友都不会陌生。在网页设计中，视差滚动是一种很特别的网页设计技术，通过让多层背景以不同的速度或者不同的方向移动形成 3D 运动效果，有很强的视觉冲击力。
  前阵子视差被用来提供一个漂浮在屏幕周围的元素，有很多的网站使用这种效果，有点势不可挡的趋势。如今，“少即是多”的概念也被应用于视差效果中。
  ---以上文字摘自《梦想天空 ◆ 关注前端开发技术 ◆ 分享网页设计资源：激发你的灵感：16个精美的视差效果网页设计作品》
  英文来源：17 Inspiring Example of Parallax Scrolling Site
  这边分享一下一个小效果，从中我们可以看到视差效果网页效果的制作原理及设计制作过程噢
  其实原理很简单：就是利用两个DIV层的背景图，设置背景的位置造成视差~~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/web-how-to-design-the-parallax-effects-with-jquery.html
published: true
post_date: 2013-12-06 12:54:23
---
对于视差滚动(Parallax Scrolling)，关注网页设计的朋友都不会陌生。在网页设计中，视差滚动是一种很特别的网页设计技术，通过让多层背景以不同的速度或者不同的方向移动形成 3D 运动效果，有很强的视觉冲击力。

前阵子视差被用来提供一个漂浮在屏幕周围的元素，有很多的网站使用这种效果，有点势不可挡的趋势。如今，“少即是多”的概念也被应用于视差效果中。

---以上文字摘自《<a href="http://www.cnblogs.com/lhb25/" target="_blank">梦想天空 ◆ 关注前端开发技术 ◆ 分享网页设计资源</a>：<a href="http://www.cnblogs.com/lhb25/p/17-inspiring-examples-of-parallax-scrolling-sites.html">激发你的灵感：16个精美的视差效果网页设计作品</a>》

英文来源：<a href="http://webdesignledger.com/inspiration/17-inspiring-examples-of-parallax-scrolling-sites" target="_blank">17 Inspiring Example of Parallax Scrolling Site</a>

&nbsp;

这边分享一下一个小效果，从中我们可以看到视差效果网页效果的制作原理及设计制作过程噢

<em><span style="text-decoration: underline;">其实原理很简单：就是利用两个DIV层的背景图，设置背景的位置造成视差~~</span></em>

Explanation

<figure><figcaption><img alt="schema" src="http://www.franckmaurin.com/wp-content/uploads/2011/01/nike-schema.png" width="220" height="190" />Note that there are visually 3 movements: The window scroll, the first shoes pair and the second one.
Shoes pair movements are independent, each one has a single coefficient for the scrolling movement

</figcaption></figure>
<h2>2HTML &amp; CSS code</h2>
<figure>
<pre><code>/* CSS */
.section {        width:540px; height:500px;
                  background:url(images/bg_freext.jpg) no-repeat #000; }
.section .inner { width:540px; height:500px;
                  background:url(images/fg_freext.png) no-repeat; }

&lt;!-- html --&gt;
&lt;div class="section"&gt;
    &lt;div class="inner"&gt;
        &lt;!-- your content here --&gt;
    &lt;/div&gt;
&lt;/div&gt;</code></pre>
<figcaption>So we need two div with background for the parent, and a transparent foreground for the child. </figcaption></figure>
<h2>3 JQuery code</h2>
<figure>
<pre><code>// The plugin code
(function($){
    $.fn.parallax = function(options){
        var $$ = $(this);
        offset = $$.offset();
        var defaults = {
            "start": 0,
            "stop": offset.top + $$.height(),
            "coeff": 0.95
        };
        var opts = $.extend(defaults, options);
        return this.each(function(){
            $(window).bind('scroll', function() {
                windowTop = $(window).scrollTop();
                if((windowTop &gt;= opts.start) &amp;&amp; (windowTop &lt;= opts.stop)) {
                    newCoord = windowTop * opts.coeff;
                    $$.css({
                        "background-position": "0 "+ newCoord + "px"
                    });
                }
            });
        });
    };
})(jQuery);

// call the plugin
$('.section').parallax({ "coeff":-0.65 });
$('.section .inner').parallax({ "coeff":1.15 });</code></pre>
</figure>&nbsp;

PS：下面的原文站非常不错，看了他的教程，基本的原理就懂了噢~

原文：《<a href="http://www.franckmaurin.com/the-parallax-effects-with-jquery/">The parallax effects with jQuery</a>》