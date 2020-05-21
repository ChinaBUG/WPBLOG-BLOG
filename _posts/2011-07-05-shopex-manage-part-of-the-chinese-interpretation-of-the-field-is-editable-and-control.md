---
ID: 1484
post_title: >
  密码保护：ShopEX-后台管理-栏目的中文诠释与控制字段是否可编辑等
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-manage-part-of-the-chinese-interpretation-of-the-field-is-editable-and-control.html
published: true
post_date: 2011-07-05 22:36:35
---
<h3>1、后台栏目中文诠释及可编辑性</h3>
文件目录：core\schemas

二次开发：否

目录作用：

后台的栏目的解释，如会员内的name的中文解释是名字，点击是否可编辑等情况的定义均在这目录内的响应文件内定义。

参数说明：

//这个是字段属性，指明数据来源，一般都是自己手动填写，个别是能够提供选项选择

'type' =&gt; 'object:goods/productCat',

//是否必填项

'required' =&gt; true,

//默认的值

'default' =&gt; 0,

//中文翻译，你总不能把name直接显示出来吧，中国人要用名字

'label' =&gt; __('分类'),

//字段的宽度

'width' =&gt; 75,

//是否可在线编辑，即点击之后会让你立即修改

'editable' =&gt; true,