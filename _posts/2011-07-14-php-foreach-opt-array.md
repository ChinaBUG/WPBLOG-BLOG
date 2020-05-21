---
ID: 1618
post_title: PHP-用foreach来操作数组
author: ChinaBUG
post_excerpt: |
  foreach()有两种用法：
  1	foreach(array_name as $value)
  2	{
  3	    statement;
  4	}
  这里的array_name是你要遍历的数组名，每次循环中，array_name数组的当前元素的值被赋给$value,并且数组内部的下标向下移一 步，也就是下次循环回得到下一个元素。
  1	foreach(array_name as $key => $value)
  2	{
  3	    statement;
  4	}
  这里跟第一种方法的区别就是多了个$key,也就是除了把当前元素的值赋给$value外，当前元素的键值也会在每次循环中被赋给变量$key。键值可以 是下标值，也可以是字符串。比如book[0]=1中的"0"，book[id]="001"中的"id".
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/php-foreach-opt-array.html
published: true
post_date: 2011-07-14 21:20:08
---
<div id="mainContent">
<h1 align="center">多用foreach来操作数组</h1>
<p align="center">2011-06-07</p>

<div>

foreach()有两种用法：
<div id="highlighter_203969">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>1</code></td>
<td><code>foreach</code><code>(array_name </code><code>as</code> <code>$value</code><code>)     </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>2</code></td>
<td><code>{        </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>statement;     </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>4</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
这里的array_name是你要遍历的数组名，每次循环中，array_name数组的当前元素的值被赋给$value,并且数组内部的下标向下移一 步，也就是下次循环回得到下一个元素。
<div id="highlighter_531684">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>1</code></td>
<td><code>foreach</code><code>(array_name </code><code>as</code> <code>$key</code> <code>=&gt; </code><code>$value</code><code>)     </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>2</code></td>
<td><code>{         </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>statement;       </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>4</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
这里跟第一种方法的区别就是多了个$key,也就是除了把当前元素的值赋给$value外，当前元素的键值也会在每次循环中被赋给变量$key。键值可以 是下标值，也可以是字符串。比如book[0]=1中的"0"，book[id]="001"中的"id".

程序实例：

1
<div id="highlighter_273201">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>&lt;?php </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>/*-------------------------------------------------------------------------*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>/* foreach example 1: value only */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code>echo</code> <code>"foreach example 1: value only "</code><code>.</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code>$a</code> <code>= </code><code>array</code><code>(1, 2, 3, 17); </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code>foreach</code> <code>(</code><code>$a</code> <code>as</code> <code>$v</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>   </code><code>echo</code> <code>"Current value of "</code><code>.</code><code>$a</code><code>.</code><code>":"</code><code>. </code><code>$v</code><code>.</code><code>"&lt;br /&gt;"</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>?&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code>// 运行结果 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code>foreach</code> <code>example 1: value only  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code>Current value of </code><code>$a</code><code>: 1 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code>Current value of </code><code>$a</code><code>: 2 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code>Current value of </code><code>$a</code><code>: 3 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td><code>Current value of </code><code>$a</code><code>: 17</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
2
<div id="highlighter_298240">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>/*-------------------------------------------------------------------------*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>/* foreach example 2: value (with key printed for illustration) */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>echo</code> <code>'&lt;br /&gt;'</code><code>.</code><code>'&lt;br /&gt;'</code><code>.</code><code>"foreach example 2: value (with key printed for illustration) "</code><code>.</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code>$a</code> <code>= </code><code>array</code><code>(1, 2, 3, 17); </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code>$i</code> <code>= 0; </code><code>/* for illustrative purposes only */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>foreach</code> <code>(</code><code>$a</code> <code>as</code> <code>$v</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>echo</code> <code>""</code><code>$a</code><code>[</code><code>$i</code><code>] =&gt; </code><code>$v</code><code>".</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>$i</code><code>++; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code>// 程序运行结果 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code>foreach</code> <code>example 2: value (with key printed </code><code>for</code> <code>illustration)  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code>$a</code><code>[0] =&gt; 1 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code>$a</code><code>[1] =&gt; 2 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td><code>$a</code><code>[2] =&gt; 3 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>19</code></td>
<td><code>$a</code><code>[3] =&gt; 17</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
3
<div id="highlighter_48369">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>/*-------------------------------------------------------------------------*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>/* foreach example 3: key and value */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>echo</code> <code>'&lt;br /&gt;'</code><code>.</code><code>'&lt;br /&gt;'</code><code>.</code><code>"foreach example 3: key and value "</code><code>.</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code>$a</code> <code>= </code><code>array</code><code>( </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>"one"</code> <code>=&gt; 1, </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code>    </code><code>"two"</code> <code>=&gt; 2, </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code>    </code><code>"three"</code> <code>=&gt; 3, </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>    </code><code>"seventeen"</code> <code>=&gt; 17 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>); </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code>foreach</code> <code>(</code><code>$a</code> <code>as</code> <code>$k</code> <code>=&gt; </code><code>$v</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>echo</code> <code>""</code><code>$a</code><code>[</code><code>$k</code><code>] =&gt; </code><code>$v</code><code>".</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code>// 程序运行结果 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code>foreach</code> <code>example 3: key </code><code>and</code> <code>value  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td><code>$a</code><code>[one] =&gt; 1 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>19</code></td>
<td><code>$a</code><code>[two] =&gt; 2 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>20</code></td>
<td><code>$a</code><code>[three] =&gt; 3 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>21</code></td>
<td><code>$a</code><code>[seventeen] =&gt; 17</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
4
<div id="highlighter_680932">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>/*-------------------------------------------------------------------------*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>/* foreach example 4: multi-dimensional arrays */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>echo</code> <code>'&lt;br /&gt;'</code><code>.</code><code>'&lt;br /&gt;'</code><code>.</code><code>"foreach example 4: multi-dimensional arrays "</code><code>.</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code>$a</code> <code>= </code><code>array</code><code>(); </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code>$a</code><code>[0][0] = </code><code>"a"</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code>$a</code><code>[0][1] = </code><code>"b"</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code>$a</code><code>[1][0] = </code><code>"y"</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>$a</code><code>[1][1] = </code><code>"z"</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>foreach</code> <code>(</code><code>$a</code> <code>as</code> <code>$v1</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code>    </code><code>foreach</code> <code>(</code><code>$v1</code> <code>as</code> <code>$v2</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code>        </code><code>echo</code> <code>"$v2"</code><code>n"; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code>    </code><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>16</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>17</code></td>
<td><code>// 程序运行结果 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>18</code></td>
<td><code>foreach</code> <code>example 4: multi-dimensional arrays  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>19</code></td>
<td><code>a b y z</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
5
<div id="highlighter_108667">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>/*-------------------------------------------------------------------------*/</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code>/* foreach example 5: dynamic arrays */</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>echo</code> <code>'&lt;br /&gt;'</code><code>.</code><code>'&lt;br /&gt;'</code><code>.</code><code>"foreach example 5: dynamic arrays "</code><code>.</code><code>'&lt;br /&gt;'</code><code>; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code>foreach</code> <code>(</code><code>array</code><code>(1, 2, 3, 4, 5) </code><code>as</code> <code>$v</code><code>) { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>echo</code> <code>"$v"</code><code>n"; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code>} </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>// 程序运行结果 </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>foreach</code> <code>example 5: dynamic arrays  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>1 2 3 4 5</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
也可以这么用：
<div id="highlighter_681998">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>01</code></td>
<td><code>$messageNav</code><code>[</code><code>'首页'</code><code>] =ROOT_PATH; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>02</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>03</code></td>
<td><code>$messageNav</code><code>[</code><code>'人才交流'</code><code>] =</code><code>"#"</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>04</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>05</code></td>
<td><code>$messageNav</code><code>[</code><code>'动态专栏'</code><code>] =</code><code>"hragent/cn/"</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>06</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>07</code></td>
<td><code>&lt;?php </code><code>$i</code> <code>= 0;</code><code>foreach</code> <code>(</code><code>$messageNav</code> <code>as</code> <code>$key</code><code>=&gt;</code><code>$value</code><code>):?&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>08</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>09</code></td>
<td><code>&lt;?php </code><code>if</code> <code>(</code><code>$i</code> <code>!= </code><code>count</code><code>(</code><code>$messageNav</code><code>) - 1):?&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>10</code></td>
<td><code>&lt;a href=</code><code>"&lt;?=$value?&gt;"</code><code>&gt;&lt;?=</code><code>$key</code><code>?&gt;&lt;/a&gt;&gt;  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>11</code></td>
<td><code>&lt;?php </code><code>else</code><code>:?&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>12</code></td>
<td><code> </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>13</code></td>
<td><code>&lt;a href=</code><code>"&lt;?=$value?&gt;"</code> <code>class</code><code>=</code><code>"onlink"</code><code>&gt;&lt;?=</code><code>$key</code><code>?&gt;&lt;/a&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>14</code></td>
<td><code>&lt;?php </code><code>endif</code><code>;?&gt; </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>15</code></td>
<td><code>&lt;?php </code><code>$i</code><code>++;</code><code>endforeach</code><code>;?&gt;</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
</div>
</div>
<div id="bottom">

注：如需转载本文，请注明出处（原文链接），谢谢。更多精彩内容，请进入<a href="../index.php" target="_blank">简明现代魔法</a>首页。

2012.02.27备

在循环内定义变量并直接使用，如定义$itemid他的值就是$aItems.item_id的值。

&lt;{assign var="itemid" value=$aItems.item_id}&gt;

</div>