---
ID: 3974
post_title: >
  PHP-四种基本排序算法的代码实现
author: ChinaBUG
post_excerpt: |
  许多人都说算法是程序的核心，算法的好坏决定了程序的质量。
  作为一个初级phper，虽然很少接触到算法方面的东西。
  但是对于基本的排序算法还是应该掌握的，它是程序开发的必备工具。
  这里介绍冒泡排序，插入排序，选择排序，快速排序四种基本算法，分析一下算法的思路。
layout: post
permalink: >
  http://blog.ipodmp.com/2015/03/php-are-four-basic-sorting-algorithm-implementation.html
published: true
post_date: 2015-03-20 10:33:42
---
许多人都说算法是程序的核心，算法的好坏决定了程序的质量。作为一个初级phper，虽然很少接触到算法方面的东西。但是对于基本的排序算法还是应该掌握的，它是程序开发的必备工具。这里介绍冒泡排序，插入排序，选择排序，快速排序四种基本算法，分析一下算法的思路。

<a href="http://s8.51cto.com/wyfs02/M02/5B/7C/wKioL1UKe_-DG7AfAAIO2tnaCZo181.jpg" target="_blank"><img class="fit-image" src="http://s8.51cto.com/wyfs02/M02/5B/7C/wKioL1UKe_-DG7AfAAIO2tnaCZo181.jpg" alt="PHP 四种基本排序算法的代码实现" width="476" height="353" border="0" /></a>

前提：分别用冒泡排序法，快速排序法，选择排序法，插入排序法将下面数组中的值按照从小到大的顺序进行排序。

$arr(1,43,54,62,21,66,32,78,36,76,39);

<strong>1. 冒泡排序</strong>

思路分析：在要排序的一组数中，对当前还未排好的序列，从前往后对相邻的两个数依次进行比较和调整，让较大的数往下沉，较小的往上冒。即，每当两相邻的数比较后发现它们的排序与排序要求相反时，就将它们互换。
<ol class="dp-j">
	<li class="alt">$arr=array(<span class="number">1</span>,<span class="number">43</span>,<span class="number">54</span>,<span class="number">62</span>,<span class="number">21</span>,<span class="number">66</span>,<span class="number">32</span>,<span class="number">78</span>,<span class="number">36</span>,<span class="number">76</span>,<span class="number">39</span>);</li>
	<li>function bubbleSort($arr)</li>
	<li class="alt">{</li>
	<li>  $len=count($arr);</li>
	<li class="alt">  <span class="comment">//该层循环控制 需要冒泡的轮数</span></li>
	<li>  <span class="keyword">for</span>($i=<span class="number">1</span>;$i&lt;$len;$i++)</li>
	<li class="alt">  { <span class="comment">//该层循环用来控制每轮 冒出一个数 需要比较的次数</span></li>
	<li>    <span class="keyword">for</span>($k=<span class="number">0</span>;$k&lt;$len-$i;$k++)</li>
	<li class="alt">    {</li>
	<li>       <span class="keyword">if</span>($arr[$k]&gt;$arr[$k+<span class="number">1</span>])</li>
	<li class="alt">        {</li>
	<li>            $tmp=$arr[$k+<span class="number">1</span>];</li>
	<li class="alt">            $arr[$k+<span class="number">1</span>]=$arr[$k];</li>
	<li>            $arr[$k]=$tmp;</li>
	<li class="alt">        }</li>
	<li>    }</li>
	<li class="alt">  }</li>
	<li>  <span class="keyword">return</span> $arr;</li>
	<li class="alt">}</li>
</ol>
<strong>2. 选择排序</strong>

思路分析：在要排序的一组数中，选出最小的一个数与第一个位置的数交换。然后在剩下的数当中再找最小的与第二个位置的数交换，如此循环到倒数第二个数和最后一个数比较为止。
<ol class="dp-j">
	<li class="alt">function selectSort($arr) {</li>
	<li><span class="comment">//双重循环完成，外层控制轮数，内层控制比较次数</span></li>
	<li class="alt">$len=count($arr);</li>
	<li><span class="keyword">for</span>($i=<span class="number">0</span>; $i&lt;$len-<span class="number">1</span>; $i++) {</li>
	<li class="alt"><span class="comment">//先假设最小的值的位置</span></li>
	<li>$p = $i;</li>
	<li class="alt"></li>
	<li><span class="keyword">for</span>($j=$i+<span class="number">1</span>; $j&lt;$len; $j++) {</li>
	<li class="alt"><span class="comment">//$arr[$p] 是当前已知的最小值</span></li>
	<li><span class="keyword">if</span>($arr[$p] &gt; $arr[$j]) {</li>
	<li class="alt"><span class="comment">//比较，发现更小的,记录下最小值的位置；并且在下次比较时采用已知的最小值进行比较。</span></li>
	<li>$p = $j;</li>
	<li class="alt">}</li>
	<li>}</li>
	<li class="alt"><span class="comment">//已经确定了当前的最小值的位置，保存到$p中。如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可。</span></li>
	<li><span class="keyword">if</span>($p != $i) {</li>
	<li class="alt">$tmp = $arr[$p];</li>
	<li>$arr[$p] = $arr[$i];</li>
	<li class="alt">$arr[$i] = $tmp;</li>
	<li>}</li>
	<li class="alt">}</li>
	<li><span class="comment">//返回最终结果</span></li>
	<li class="alt"><span class="keyword">return</span> $arr;</li>
	<li>}</li>
</ol>
<strong>3.插入排序</strong>

思路分析：在要排序的一组数中，假设前面的数已经是排好顺序的，现在要把第n个数插到前面的有序数中，使得这n个数也是排好顺序的。如此反复循环，直到全部排好顺序。
<ol class="dp-j">
	<li class="alt">function insertSort($arr) {</li>
	<li>$len=count($arr);</li>
	<li class="alt"><span class="keyword">for</span>($i=<span class="number">1</span>, $i&lt;$len; $i++) {</li>
	<li>$tmp = $arr[$i];</li>
	<li class="alt"><span class="comment">//内层循环控制，比较并插入</span></li>
	<li><span class="keyword">for</span>($j=$i-<span class="number">1</span>;$j&gt;=<span class="number">0</span>;$j--) {</li>
	<li class="alt"><span class="keyword">if</span>($tmp &lt; $arr[$j]) {</li>
	<li><span class="comment">//发现插入的元素要小，交换位置，将后边的元素与前面的元素互换</span></li>
	<li class="alt">$arr[$j+<span class="number">1</span>] = $arr[$j];</li>
	<li>$arr[$j] = $tmp;</li>
	<li class="alt">} <span class="keyword">else</span> {</li>
	<li><span class="comment">//如果碰到不需要移动的元素，由于是已经排序好是数组，则前面的就不需要再次比较了。</span></li>
	<li class="alt"><span class="keyword">break</span>;</li>
	<li>}</li>
	<li class="alt">}</li>
	<li>}</li>
	<li class="alt"><span class="keyword">return</span> $arr;</li>
	<li>}</li>
</ol>
<strong>4.快速排序</strong>

思路分析：选择一个基准元素，通常选择第一个元素或者最后一个元素。通过一趟扫描，将待排序列分成两部分，一部分比基准元素小，一部分大于等于基准元素。此时基准元素在其排好序后的正确位置，然后再用同样的方法递归地排序划分的两部分。
<ol class="dp-j">
	<li class="alt">function quickSort($arr) {</li>
	<li><span class="comment">//先判断是否需要继续进行</span></li>
	<li class="alt">$length = count($arr);</li>
	<li><span class="keyword">if</span>($length &lt;= <span class="number">1</span>) {</li>
	<li class="alt"><span class="keyword">return</span> $arr;</li>
	<li>}</li>
	<li class="alt"><span class="comment">//选择第一个元素作为基准</span></li>
	<li>$base_num = $arr[<span class="number">0</span>];</li>
	<li class="alt"><span class="comment">//遍历除了标尺外的所有元素，按照大小关系放入两个数组内</span></li>
	<li><span class="comment">//初始化两个数组</span></li>
	<li class="alt">$left_array = array();  <span class="comment">//小于基准的</span></li>
	<li>$right_array = array();  <span class="comment">//大于基准的</span></li>
	<li class="alt"><span class="keyword">for</span>($i=<span class="number">1</span>; $i&lt;$length; $i++) {</li>
	<li><span class="keyword">if</span>($base_num &gt; $arr[$i]) {</li>
	<li class="alt"><span class="comment">//放入左边数组</span></li>
	<li>$left_array[] = $arr[$i];</li>
	<li class="alt">} <span class="keyword">else</span> {</li>
	<li><span class="comment">//放入右边</span></li>
	<li class="alt">$right_array[] = $arr[$i];</li>
	<li>}</li>
	<li class="alt">}</li>
	<li><span class="comment">//再分别对左边和右边的数组进行相同的排序处理方式递归调用这个函数</span></li>
	<li class="alt">$left_array = quick_sort($left_array);</li>
	<li>$right_array = quick_sort($right_array);</li>
	<li class="alt"><span class="comment">//合并</span></li>
	<li><span class="keyword">return</span> array_merge($left_array, array($base_num), $right_array);</li>
	<li class="alt">}</li>
</ol>
<div class="f12"></div>
<div class="f12"><span class="fl">原文：<a class="blue" href="http://developer.51cto.com/art/201503/468906.htm"><b>PHP 四种基本排序算法的代码实现</b></a></span></div>