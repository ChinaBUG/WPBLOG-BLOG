---
ID: 3544
post_title: WEB-图片优化的那些工具
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/07/web-image-optimization-tools.html
published: true
post_date: 2014-07-30 14:24:37
---
图片作为页面的一个主要因素，它的大小直接影响了页面的加载速度，这一点在移动端尤显突出。

怎么让图片的大小更小？除了选择合适的格式（jpeg、gif、png），我们还可以利用网上的应用（如smushit、tinypng、imagemin、imageOptim）对图片进行压缩。在这里我想给大家介绍一下上述应用主要使用到了哪些命令行工具以及它们的使用方法。<span id="more-3582"></span>

<a href="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim.jpg"><img class="size-full wp-image-3585 aligncenter" src="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim.jpg" alt="imageoptim" width="635" height="285" /></a>

<strong>Jpegtran</strong>

JPEG的压缩工具有jpegtran和jpegoptim，这两款工具的压缩效果几乎没有区别，在这里我们推荐使用jpegtran，相比后者，jpegtran可以进行progressive编码，使图片渐进式的展现，先显示模糊的图片，再逐步清晰。

推荐命令行参数：

jpegtran –copy none –optimize -progressive -outfile out.jpg in.jpg

想知道这些参数的具体作用，可使用命令“jpegtran –h”了解，下同。

&nbsp;

<strong>Gifsicle</strong>

Gif动画可使用gifsicle来优化，它会剥离不同帧中重复的像素来优化gif动画，对于单帧gif我们推荐还是使用png8来替代。

推荐命令行参数：

gifsicle –interlace -O3 –careful –no-comments –no-names –same-delay –same-loopcount –no-warnings -o out.gif in.gif

&nbsp;

<strong>pngcrush</strong><strong>、optipng</strong><strong>、pngout</strong>

PNG压缩可分为无损压缩和有损压缩，以上三款可以说是现在比较主流的无损压缩工具了。从ImageOptim的压缩效果可以看出，optipng和pngcrush对于色彩比较单一、大小比较小的图片压缩效果好于pngout，而对于色彩比较丰富、透明渐变的图片来说pngout的压缩比明显更高。另外，经测试，Google的PageSpeed上提供的可压缩比是按照optipng给出的。

<a href="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim_1.jpg"><img class="size-full wp-image-3583 aligncenter" src="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim_1.jpg" alt="imageoptim_1" width="326" height="125" /></a>

<a href="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim_2.jpg"><img class="size-full wp-image-3584 aligncenter" src="http://ued.ctrip.com/blog/wp-content/uploads/2014/06/imageoptim_2.jpg" alt="imageoptim_2" width="513" height="400" /></a>

推荐命令行参数：

pngcrush -brute -rem alla -nofilecheck -bail -blacken -reduce -cc in.png out.png

optipng -strip all -quiet -clobber -o3 -i0 in.png -out out.png

pngout -k1 -r -v in.png out.png

&nbsp;

<strong>pngquant</strong><strong>、pngnq</strong>

两款PNG的有损压缩工具，基本都能将图片压缩掉40%以上，它们会将PNG转换成alpha透明的PNG8，由于PNG8最多支持256色，所以内容丰富的图片压缩后会看出些许差异，但属于可接受范围内，而纯色图片基本能保持原图的质量。另外，这种alpha透明的PNG8图片在IE6以上及其他标准浏览器可以显示正常的alpha透明度，在IE6中则会忽略掉有alpha透明度的颜色，作为全透明处理（边缘稍有锯齿但影响不大），而不像png32那样alpha透明区域在IE6下显示灰色。

推荐命令行参数：

pngnq –s 1 –d outdir/ in.png

pngquant -s1 –o out.png in.png

PS：pngquant的-s是speed参数，可选值1-10，默认为3，在经过多图测试后发现1的压缩比和效果都是最佳的，其他的参数或多或少存在缺陷，这里推荐选1。

&nbsp;

总结

如果您已经学会如何使用这些命令行软件对自己的图片进行压缩优化，那么恭喜您，您的图片瘦身成功。如果您觉得命令行一行一行的压缩图片太麻烦，那么除了去使用文章开头所说到的那几款应用外，感兴趣的同学也可以整合它们去开发一套自己的应用，利用php的exec、nodejs的child_process.exec(cmd, [options], callback)等等方法执行shell命令，再配上一些交互，一款好用的图片优化应用就此诞生。最后希望这篇文章对大家有所帮助。

本文作者：晓辉　转载请注明来自：<a href="http://ued.ctrip.com/blog"><strong>携程UED</strong></a>