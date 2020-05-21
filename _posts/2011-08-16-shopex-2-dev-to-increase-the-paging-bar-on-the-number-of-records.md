---
ID: 1730
post_title: >
  密码保护：ShopEX-二次开发-分页栏上增加记录条数(分页函数修改扩展)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/shopex-2-dev-to-increase-the-paging-bar-on-the-number-of-records.html
published: true
post_date: 2011-08-16 12:32:54
---
ShopEX-二次开发-分页栏上增加记录条数

core/admin/smartyplugin/function.pager.php
------------------------------------------------
return &lt;&lt;&lt;EOF
&lt;table cellpadding="0" cellspacing="0" style="width:auto"&gt;&lt;tr&gt;
&lt;td style="padding-right:20px"&gt;{$prev}&amp;nbsp;{$links}&amp;nbsp;{$next}&lt;/td&gt;
&lt;td style="padding-right:20px;padding-left:20px;"&gt;共&lt;strong style="padding-left:5px"&gt;{$total_rs}&lt;/strong&gt;条记录符合条件&lt;/td&gt;
&lt;/tr&gt;&lt;/table&gt;
EOF;




调用时增加一个参数：'total_rs'=&gt;count($list_count)

$this-&gt;pagedata['pager'] = array(
'current'=&gt; $pageIndex,
'total'=&gt; $pageCount,
'link'=&gt; 'javascript:go2p(_PPP_)',
'token'=&gt; '_PPP_',
'total_rs'=&gt;count($list_count)
);