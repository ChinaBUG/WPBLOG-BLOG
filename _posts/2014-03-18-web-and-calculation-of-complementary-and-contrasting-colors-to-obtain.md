---
ID: 3280
post_title: >
  WEB-互补色与对比色的计算与获取
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/web-and-calculation-of-complementary-and-contrasting-colors-to-obtain.html
published: true
post_date: 2014-03-18 21:30:52
---
今天在做界面设计是需要互补色凸显一下效果，发现手上没有工具啦，好吧，找一下计算方式吧，起码没有应手的工具之前，可以先应付一下。

<strong>互补色</strong>

摘自：<a href="http://www.blueidea.com/tech/graph/2003/616.asp">互补色简单计算法 </a>

这个是色相色谱，位于180度夹角的颜色就是互补色

<img alt="" src="http://www.blueidea.com/articleimg/2003/08/616/dcps_2_18.jpg" width="112" height="112" align="0" border="0" />

要计算某种颜色的互补色，首先取得这个颜色的RGB数值，再用255分别减去你现有的RGB值即可。

比如纯黄色：r255 g255 b0

那么通过计算 r(255-255) g(255-255) b(255-0)

互补色为：r0 g0 b255

就是纯蓝色

注：

只有在8位通道模式下使用255（2的8次方）

若是16位通道下则要用65535（2的16次方）来减

Photoshop默认使用8位通道模式

该模式可以显示1677万色（256 x 256 x 256）

已经超出人眼所能分辨的色彩总数了（故又称真彩色）

因此更高的通道级别就肉眼看来没有区别。

<header>
<h2>如何计算颜色对比色</h2>
</header>网页元素的颜色对比度依赖于颜色的明亮度和色调。如果两种颜色的亮度和色调差异超过一定阈值， 那么它们之间就能形成强对比。如果这两个度量值中有一个或两个指标均下于阈值，那么W3C建议使用 别的颜色。如果你知道两个颜色的RGB值，那么你可以计算这两个颜色的对比度。让网页中的背景和文字 具有强对比，往往能改进用户体验。
<h1>计算颜色亮度差异步骤</h1>

<hr />

<ul>
	<li>令R1, G1, B1代表文本颜色的红色，绿色和蓝色。令R2, G2, B2代表背景颜色</li>
	<li>计算(299*R1 + 587*G1 + 114*B1)/1000. 这是文本颜色的亮度，假设你使用 暗绿色，R1=20, G1=100和B1=20，那么文本颜色的亮度是66.96，因为(299*20 + 587*100 + 114*20)/1000 = 66.96</li>
	<li>计算 (299*R2 + 587*G2 + 114*B2)/1000。这是背景色的亮度。例如，你使用浅粉色 R2=255, G2=220和B2=240，那么背景色的亮度是232.75，因为(299*255 + 587*220 + 114*240)/1000 = 232.75.</li>
	<li>计算亮度值的差异。如果亮度值差异小于125，那么需要调整其中一个或者两个来增加亮度差异。例如 232.75-66.96=165.79，因此我们不需要改变这两个颜色</li>
</ul>
<h1>计算色调差异</h1>
<ul>
	<li>计算两个颜色红色值的差异。还是上面的例子，我们计算可得红色值差异为255-20=235</li>
	<li>重复前面一个步骤，绿色和蓝色。还是上面的例子， 绿色差异值为120（因为220-100=120）, 蓝色差异值为220(因为240-20=220)</li>
	<li>把上述三个分量的差异值相加得到的是色调差异。如果色调差异小于500，那么你需要调整颜色。 例如，因为235+120+220=575，所以你不需要改变颜色。</li>
</ul>
附录：

怎么用Javascript获取对比色，互补色？

[code lang="javascript"]
&lt;script type=&quot;text/javascript&quot; language=&quot;javascript&quot;&gt;// &lt;![CDATA[
    var asHex=new Array(16);
    asHex[0]=&quot;0&quot;;
    asHex[1]=&quot;1&quot;;
    asHex[2]=&quot;2&quot;;
    asHex[3]=&quot;3&quot;;
    asHex[4]=&quot;4&quot;;
    asHex[5]=&quot;5&quot;;
    asHex[6]=&quot;6&quot;;
    asHex[7]=&quot;7&quot;;
    asHex[8]=&quot;8&quot;;
    asHex[9]=&quot;9&quot;;
    asHex[10]=&quot;a&quot;;
    asHex[11]=&quot;b&quot;;
    asHex[12]=&quot;c&quot;;
    asHex[13]=&quot;d&quot;;
    asHex[14]=&quot;e&quot;;
    asHex[15]=&quot;f&quot;;

    function lightColor(sColor, iStep)        // eg. sColor=&quot;#23a3df&quot;, iStep=-4
    {
            var sRslt=&quot;#&quot;;
            var iTemp;
            for(var i=1; i&lt;7; i++)             {
                     iTemp=parseInt(&quot;0x&quot;+sColor.substr(i,1))+iStep;
                     if(iTemp&gt;15)
                            iTemp=15;
                    if(iTemp&lt;0)
                            iTemp=0;
                    sRslt+=asHex[iTemp];
            }
            return sRslt;
    }
// ]]&gt;&lt;/script&gt;&lt;/pre&gt;
&lt;div id=&quot;mybutton&quot; style=&quot;text-align: center; padding-top: 2px; color: white; height: 20px; width: 100px; font-size: 9pt; cursor: default;&quot;&gt;Button&lt;/div&gt;
&lt;div id=&quot;mybutton1&quot; style=&quot;text-align: center; padding-top: 2px; color: white; height: 20px; width: 100px; font-size: 9pt; cursor: default;&quot;&gt;Button&lt;/div&gt;
&lt;pre&gt;
&lt;script type=&quot;text/javascript&quot; language=&quot;javascript&quot;&gt;// &lt;![CDATA[
    Chang(&quot;#6699CC&quot;);
    function Chang(color)
    {
            var lcolor = lightColor(color,4);
            var dcolor = lightColor(color,-4);
            var bcolor = lightColor(color,-2);
            mybutton.style.backgroundColor = color;
            mybutton.style.borderLeft = &quot;1px solid &quot; + lcolor;
            mybutton.style.borderTop = &quot;1px solid &quot; + lcolor;
            mybutton.style.borderRight = &quot;1px solid &quot; + dcolor;
            mybutton.style.borderBottom = &quot;1px solid &quot; + dcolor;

            mybutton1.style.backgroundColor = bcolor;
            mybutton1.style.borderLeft = &quot;1px solid &quot; + dcolor;
            mybutton1.style.borderTop = &quot;1px solid &quot; + dcolor;
            mybutton1.style.borderRight = &quot;1px solid &quot; + lcolor;
            mybutton1.style.borderBottom = &quot;1px solid &quot; + lcolor;
    }

// ]]&gt;&lt;/script&gt;
[/code]