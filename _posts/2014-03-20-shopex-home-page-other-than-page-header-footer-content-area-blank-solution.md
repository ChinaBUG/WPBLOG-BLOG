---
ID: 3302
post_title: >
  ShopEX-主页除页头页尾之外系统内容区域空白的解决方案
author: ChinaBUG
post_excerpt: |
  昨天，同事修改了一下商城的广告，然后首页就再也不能显示出内容了，表现的情况是：页头，页尾的内容正常显示，但是中间的内容部分却一点也不显示。
  但是，其他页面的内容却是正常的，也就是只有主页的内容是错误的。
  看到这个现象第一个感觉就是page控制器出现问题了，因为ShopEX的主页其实就是一个单页面，然后点开其他的单页面发现都是正常显示。
  经过排查，让同事小王同学发现了问题所在了，在调用模板输出的时候，主页不知道为啥，调用错误的模板文件！
  SQL：select template_name from sdb_template_relation where source_id="PAGE:index" and template_type ="page"
  正常的时候这个执行应该是获取正确的页面模板才对，结果，这个指向模板包内的一个空文件内（其实该模板文件是以前设置的单页模板，暂时里面只有{main}标签）。结果~自然就是没内容了。
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/shopex-home-page-other-than-page-header-footer-content-area-blank-solution.html
published: true
post_date: 2014-03-20 14:41:31
---
昨天，同事修改了一下商城的广告，然后首页就再也不能显示出内容了，表现的情况是：页头，页尾的内容正常显示，但是中间的内容部分却一点也不显示。 但是，其他页面的内容却是正常的，也就是只有主页的内容是错误的。 看到这个现象第一个感觉就是page控制器出现问题了，因为ShopEX的主页其实就是一个单页面，然后点开其他的单页面发现都是正常显示。 经过排查，让同事小王同学发现了问题所在了，在调用模板输出的时候，主页不知道为啥，调用错误的模板文件！

[code lang="sql"]
select template_name from sdb_template_relation where source_id=&quot;PAGE:index&quot; and template_type =&quot;page&quot;
[/code]

正常的时候这个执行应该是获取正确的页面模板才对，结果，这个指向模板包内的一个空文件内（其实该模板文件是以前设置的单页模板，暂时里面只有{main}标签）。结果~自然就是没内容了。
<table id="table_results">
<thead>
<tr>
<th>template_name</th>
</tr>
</thead>
<tbody>
<tr>
<td>page-sp.html</td>
</tr>
</tbody>
</table>
删除这个记录或者修改这个记录的source_id值，主页得到恢复。 备注：根据shopPage.php内output()方法内的逻辑分析，这个主页获取到的值应该是空的，这样系统将会使用一个默认的模板，而这个模板就是我们编辑时指定的默认模板哈。 PS：从这个案例来说，估计我们是特例，不可重复哈，如果出现同样的问题的话，请按照上面的代码获取记录删除或者修改即可。 现在在来说一个比较神奇的事情，如果小王同学没有提醒我，或许我都是直接删除即可哈，也就不会有下面的更深入的研究哈，使用上面的SQL语句获取到的这条记录，不是我们所需要的！什么意思？就是使用上面的语句修改成下面的语句：

[code lang="sql"]
select * from sdb_template_relation where source_id=&quot;PAGE:index&quot; and template_type =&quot;page&quot;
[/code]

运行之后得到的结果是：
<table id="table_results">
<thead>
<tr>
<th>template_relation_id</th>
<th>source_type</th>
<th>source_id</th>
<th>template_name</th>
<th>template_type</th>
</tr>
</thead>
<tbody>
<tr>
<td align="right">296</td>
<td>page</td>
<td align="right">0</td>
<td>page-sp.html</td>
<td>page</td>
</tr>
</tbody>
</table>
请仔细看上面表格的source_id值！获取到的数据值是0噢，但是我们在查询语句中的值却是字符串PAGE:index噢！

神奇的地方就在于此，如果你将这个值该为1、2、3......的话那么获取的事别的数据，但是如果你将PAGE:index改为其他任何的非数字的话，那么得到的结果一定是这条记录。

为什么噢？ 所以，一开始就是怀疑MySQL会自动转换类型，找了一下还真的有这方面的资料噢，附上链接吧，到此这个神奇的问题解决了~~~
<ol>
	<li><a href="http://cqjava.iteye.com/blog/1042182">mysql中sql查询字符串到int自动转化</a></li>
	<li><a href="http://soft.chinabyte.com/database/271/11693771.shtml">带您深入了解MySQL字符串连接</a></li>
</ol>