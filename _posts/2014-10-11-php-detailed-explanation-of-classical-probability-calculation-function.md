---
ID: 3663
post_title: >
  PHP-经典概率计算方法函数详解两例
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/10/php-detailed-explanation-of-classical-probability-calculation-function.html
published: true
post_date: 2014-10-11 11:29:33
---
经常在开发游戏的时候会用到概率计算，然后就会满网搜索，麻烦，直接转摘收藏吧，省的还要找，特别是，有时候真的找不到啦！

。~。~。~

<strong>方法一：</strong>

先上函数：
<pre lang="php"><span class="php__keyword"><strong><span style="color: #000080;">function</span></strong></span> get_rand(<span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proArr</span></span>) { 
    <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">result</span></span> = <span class="php__string1"><span style="color: #800080;">''</span></span>;  
    <span class="php__com"><span style="color: #008000;">//概率数组的总概率精度</span></span> 
    <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proSum</span></span> = array_sum(<span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proArr</span></span>); 
    <span class="php__com"><span style="color: #008000;">//概率数组循环</span></span> 
    <span class="php__keyword"><strong><span style="color: #000080;">foreach</span></strong></span> (<span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proArr</span></span> <span class="php__keyword"><strong><span style="color: #000080;">as</span></strong></span> <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">key</span></span> =&gt; <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proCur</span></span>) { 
        <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">randNum</span></span> = mt_rand(<span class="php__number"><span style="color: #ff0000;">1</span></span>, <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proSum</span></span>); 
        <span class="php__keyword"><strong><span style="color: #000080;">if</span></strong></span> (<span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">randNum</span></span> &lt;= <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proCur</span></span>) { 
            <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">result</span></span> = <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">key</span></span>; 
            <span class="php__keyword"><strong><span style="color: #000080;">break</span></strong></span>; 
        } <span class="php__keyword"><strong><span style="color: #000080;">else</span></strong></span> { 
            <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proSum</span></span> -= <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proCur</span></span>; 
        } 
    } 
    <span class="php__keyword"><strong><span style="color: #000080;">unset</span></strong></span> (<span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">proArr</span></span>); 
    <span class="php__keyword"><strong><span style="color: #000080;">return</span></strong></span> <span class="php__keyword"><strong><span style="color: #000080;">$</span></strong></span><span class="php__variable"><span style="color: #4040c2;">result</span></span>; 
}</pre>
函数原文详解，很清晰噢：
<blockquote><em>上述代码是一段经典的概率算法，$proArr是一个预先设置的数组，假设数组为：array(100,200,300，400)，开始是从1,1000这个概率范围内筛选第一个数是否在他的出现概率范围之内， 如果不在，则将概率空间，也就是k的值减去刚刚的那个数字的概率空间，在本例当中就是减去100，也就是说第二个数是在1，900这个范围内筛选的。</em>
<em>这样筛选到最终，总会有一个数满足要求。</em>
<em>就相当于去一个箱子里摸东西，第一个不是，第二个不是，第三个还不是，那最后一个一定是。</em>
<em>这个算法简单，而且效率非常高，关键是这个算法已在我们以前的项目中有应用，尤其是大数据量的项目中效率非常棒。</em></blockquote>
&nbsp;

然后就根据自己的需要定制<em>$proArr</em>变量的内容了，还是不懂得话请参考原文噢。

函数来源于《<a href="http://www.helloweba.com/view-blog-184.html">PHP+jQuery实现翻板抽奖</a>》一文，有什么不清楚者可以转道阅读。

方法二：
<pre lang="php">/**
 * 简单的抽奖概率函数
 * @param array $rewardArray 概率,如：$rewardArray = array(10, 20, 20, 30, 10, 10)，对应各奖品的概率
 * @return int    概率数组的下标
 */
function luckDraw($rewardArray){
    $sum = array_sum($rewardArray);
    if($sum !== 100) {
        return 'Error:The sum of values in $rewardArray Must be equal to 100!';
    }
    //获取随机数
    $rewardNum = mt_rand(0, $sum - 1);
    //echo $rewardNum . '&lt;br/&gt;';
    $totalnum = count($rewardArray);
    //echo $totalnum . '&lt;br/&gt;';
    for($i = 0; $i &lt; $totalnum; $i++) {
        if($i == 0) {
            if($rewardArray[$i] &gt; $rewardNum &amp;&amp; $rewardNum &gt;= 0) {
                return $i;
            }
        } else {
            $max = $min = 0;
            for($j = 0; $j &lt;= $i; $j++) {
                $max = $max + $rewardArray[$j];
            }
            for($k = 0; $k &lt; $i; $k++) {
                $min = $min + $rewardArray[$k];
            }
            if($max &gt; $rewardNum &amp;&amp; $rewardNum &gt;= $min) {
                return $i;
            }
        }
    }
}
</pre>
注意：<strong>该函数主要是判断产生的随机数所在的概率范围。需要注意的是，这个方法是返回概率的下标，告诉你是那个概率中奖了。比如返回3则证明是概率30中奖了，随机数范围是50-80之间。</strong>

实例如下：

$rewardArray = array(10, 20, 20, 30, 10, 10);

//各概率对应值范围
//10,:0-10
//20:10-30
//20:30-50
//30:50-80
//10:80-90
//10:90-100

echo luckDraw($rewardArray);

函数来源于《<a href="http://www.php230.com/a-simple-draw-probability-function.html">一个简单的抽奖概率函数</a>》有需要可以移步阅读。