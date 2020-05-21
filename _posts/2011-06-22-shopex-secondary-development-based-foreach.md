---
ID: 1397
post_title: ShopEX-二次开发基础之foreach
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-secondary-development-based-foreach.html
published: true
post_date: 2011-06-22 21:09:31
---
虫神曰：仅供参考。

说明：

&lt;{foreach}&gt; 用于像循环访问一个数字索引数组一样循环访问一个关联数组，与仅能访问数字索引数组的{section}不同，&lt;{foreach}&gt;的语法比{section}的语法简单得多，但是作为一个折衷方案也仅能用于单个数组。每个&lt;{foreach}&gt;标记必须与关闭标记&lt;{/foreach}&gt;成对出现。

<a></a>
<div>
<table border="0">
<thead>
<tr>
<th align="center" valign="middle">Attribute Name属性名称</th>
<th align="center" valign="middle">Type类型</th>
<th align="center" valign="middle">Required必要</th>
<th align="center" valign="middle">Default默认值</th>
<th align="left" valign="middle">Description描述</th>
</tr>
</thead>
<tbody>
<tr>
<td align="center" valign="middle">from</td>
<td align="center" valign="middle">array数组</td>
<td align="center" valign="middle">Yes必要</td>
<td align="center" valign="middle"><em>n/a</em></td>
<td align="left" valign="middle">The array you are looping through
循环访问的数组</td>
</tr>
<tr>
<td align="center" valign="middle">item</td>
<td align="center" valign="middle">string字符串</td>
<td align="center" valign="middle">Yes必要</td>
<td align="center" valign="middle"><em>n/a</em></td>
<td align="left" valign="middle">The name of the variable that is the current element
当前元素的变量名</td>
</tr>
<tr>
<td align="center" valign="middle">key</td>
<td align="center" valign="middle">string字符串</td>
<td align="center" valign="middle">No可选</td>
<td align="center" valign="middle"><em>n/a</em></td>
<td align="left" valign="middle">The name of the variable that is the current key
当前键名的变量名</td>
</tr>
<tr>
<td align="center" valign="middle">name</td>
<td align="center" valign="middle">string字符</td>
<td align="center" valign="middle">No可选</td>
<td align="center" valign="middle"><em>n/a</em></td>
<td align="left" valign="middle">The name of the foreach loop for accessing foreach properties
用于访问foreach属性的foreach循环的名称</td>
</tr>
</tbody>
</table>
</div>
<div>语法：</div>
<div>&lt;{foreach from=数组变量 item=数据变量别称 key=循环的序号 name=循环的名称}&gt;
......
&lt;{/foreach}&gt;</div>
例子：

1、

&lt;{foreach from=$filter.areas item=items name="_s"}&gt;
&lt;option&lt;{if $smarty.foreach._s.iteration is odd}&gt;&lt;{/if}&gt; value="&lt;{$items.name}&gt;"&gt;&lt;{$items.name}&gt;&lt;/option&gt;
&lt;{/foreach}&gt;

2、       

&lt;{foreach from=$data item=data key=key}&gt;
          &lt;tr&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$key+1}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.name}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.saleTimes}&gt;&lt;/div&gt;&lt;/td&gt;
            &lt;td&gt;&lt;div align="center"&gt;&lt;{$data.salePrice}&gt;&lt;/div&gt;&lt;/td&gt;
          &lt;/tr&gt;
&lt;{/foreach}&gt;

3、多维数组的循环输出

  $sqlList = "SELECT distinct no_id,meno from sdb_rcomp2p_choujiangjilu order by no_id";
  $noid = $db-&gt;select($sqlList);
  foreach ($noid as $k=&gt;$m){
            $noid[$k]['det'] = $db-&gt;select("select count(no_id) rcount,meno from sdb_rcomp2p_choujiangjilu where no_id=".$m['no_id']);                                                                                                     
        }
  $this-&gt;pagedata['noid'] = $noid;

在html文件中

      &lt;table border="0" cellpadding="0" cellspacing="0" width="450"&gt;
        &lt;tr&gt;
          &lt;td&gt;奖项&lt;/td&gt;
          &lt;td&gt;获奖数&lt;/td&gt;
        &lt;/tr&gt;
  &lt;{foreach from=$<span style="background-color: #ff0000;"><strong>noid</strong></span> item=m key=key}&gt;
        &lt;{foreach from=$m.<span style="background-color: #ff0000;">det</span> item=rm}&gt;
        &lt;tr&gt;
          &lt;td align="left"&gt;&lt;{$rm.meno}&gt;&lt;/td&gt;
          &lt;td&gt;&lt;b style="color:#F00;"&gt;&lt;{$rm.rcount}&gt;&lt;/b&gt;&lt;/td&gt;
        &lt;/tr&gt;
        &lt;{/foreach}&gt;
  &lt;{/foreach}&gt;
      &lt;/table&gt;

4.PS~~抽空再来说明吧，最近服务器问题，都不怎么爱写了

    &lt;{foreach from=$goods.prototype.ordernum item=propord key=key}&gt;
      &lt;{if $goods.prototype.props.$propord.show}&gt;
      &lt;{assign var="pkey" value="p_{$propord}"}&gt;
      &lt;{assign var="pcol" value=$goods.$pkey}&gt;
      &lt;{if trim($pcol) !== ''}&gt;
      &lt;li&gt;
      &lt;span&gt;&lt;{$goods.prototype.props.$propord.name}&gt;：&lt;/span&gt;
      &lt;{if $goods.prototype.props.$propord.type == 'select'}&gt;
        &lt;{if $goodsproplink}&gt;&lt;a href="&lt;{selector type=$goods.type_id key=$propord value=$pcol}&gt;" target="_blank"&gt;&lt;{$goods.prototype.props.$propord.options.$pcol}&gt;&lt;/a&gt;&lt;{else}&gt;&lt;{$goods.prototype.props.$propord.options.$pcol}&gt;&lt;{/if}&gt;
      &lt;{else}&gt;
        &lt;{$pcol}&gt;
      &lt;{/if}&gt;
          &lt;/li&gt;
      &lt;{/if}&gt;
      &lt;{/if}&gt;
    &lt;{/foreach}&gt;