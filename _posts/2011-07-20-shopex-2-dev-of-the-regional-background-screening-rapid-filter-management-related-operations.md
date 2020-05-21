---
ID: 1693
post_title: >
  密码保护：ShopEX-二次开发
  后台管理
  筛选区域快速筛选条件相关的操作
author: ChinaBUG
post_excerpt: |
  案例描述：
  我们在使用ShopEX系统时，在后台管理的列表页里面，在右边都有一个快速的筛选区域，另外就是筛选按钮。
  在筛选窗口里面的条件好多噢，不错，但是里面的筛选条件是不是都能满足你的要求呢？你有遇上想包含条件的时候却发现，死活没有这个选项呢？
  在快速筛选区域，这个不错，方便噢，但是，是不是觉得里面可以查询的功能少了一点呢？
  有以上两个问题，我们怎么解决呢？请看本文哈~
  我们的目标是：
  在快速查询区域增加一个条件：地区，且它的条件限制是包含关键字，只要出现关键字都匹配。~会不会跟下面的冲突呢？不会！
  扩展筛选窗口里面的条件的条件限制，本来只有大于、小于，等于，现在就让他变成啥都能包含的吧
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-of-the-regional-background-screening-rapid-filter-management-related-operations.html
published: true
post_date: 2011-07-20 21:46:45
---
案例描述：

我们在使用ShopEX系统时，在后台管理的列表页里面，在右边都有一个快速的筛选区域，另外就是筛选按钮。

在筛选窗口里面的条件好多噢，不错，但是里面的筛选条件是不是都能满足你的要求呢？你有遇上想包含条件的时候却发现，死活没有这个选项呢？

在快速筛选区域，这个不错，方便噢，但是，是不是觉得里面可以查询的功能少了一点呢？

有以上两个问题，我们怎么解决呢？请看本文哈~

我们的目标是：
<ol>
	<li>在快速查询区域增加一个条件：地区，且它的条件限制是包含关键字，只要出现关键字都匹配。~会不会跟下面的冲突呢？不会！</li>
	<li>扩展筛选窗口里面的条件的条件限制，本来只有大于、小于，等于，现在就让他变成啥都能包含的吧</li>
</ol>
修改中文诠释的文件，不懂得请看本博客另外一篇文章<a title="ShopEX-后台管理-栏目的中文诠释与控制字段是否可编辑等" href="http://blog.ipodmp.com/archives/shopex-manage-part-of-the-chinese-interpretation-of-the-field-is-editable-and-control/"> ShopEX-后台管理-栏目的中文诠释与控制字段是否可编辑等</a>

我们就以为会员列表增加一个地区筛选条件为例。请打开core\schemas\members.php文件，找到：

'area' =&gt;
array (
'label' =&gt; __('地区'),
'width' =&gt; 110,
'type' =&gt; 'region',
'editable' =&gt; false,
'filtertype'=&gt;'yes',
'filterdefalut'=&gt;'true',
),

并且修改成

'area' =&gt;
array (
'label' =&gt; __('地区'),
'width' =&gt; 110,
'type' =&gt; 'region',
'editable' =&gt; false,
'filtertype'=&gt;'yes',
'filterdefalut'=&gt;'true',
<span style="color: #ff0000;">'searchtype' =&gt; 'has',</span>
),

请注意红色代码部分，这是关键。下面解释一下各个关键字的作用

'filtertype' =&gt; 'number',//过滤器的字段类型，防止数字型的却用字符型的查询符号。
'filterdefalut'=&gt;'true',//设定在筛选条件的窗口是否默认显示
'searchtype' =&gt; 'head'//设定这个属性，则会在快速筛选区出现，按照这个属性来生成SQL语句进行查询。

到此就修改完毕了。具体参数请看附录

附录1：searchtype的取值

has    包含
tequal    等于
head    开头等于
foot    结尾等于
nohas    不包含
than    大于
lthan    小于
nequal    等于
sthan    小于等于
bthan    大于等于

具体的定义在/core/object.filter_parser.php文件里：

"than" =&gt; " &gt; ".$var,
"lthan" =&gt; " &lt; ".$var,
"nequal" =&gt; " = '".$var."'",
"tequal" =&gt; " = '".$var."'",
"sthan" =&gt; " &lt;= ".$var,
"bthan" =&gt; " &gt;= ".$var,
"has" =&gt; " like '%".$var."%'",
"head" =&gt; " like '".$var."%'",
"foot" =&gt; " like '%".$var."'",
"nohas" =&gt; " not like '%".$var."%'"

附录2：filtertype的取值

'filtertype' =&gt; 'normal|yes'

其他值在datatypes.php中定义。