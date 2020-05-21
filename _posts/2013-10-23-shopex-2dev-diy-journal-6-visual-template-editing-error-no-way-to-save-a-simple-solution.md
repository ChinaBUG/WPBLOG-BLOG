---
ID: 2670
post_title: >
  ShopEX二次开发DIY日记6之可视化模板编辑出错没办法保存简单解决
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  今天帮客户处理一个商品详情页的挂件位，需要增加一个图片广告位(ad_pic)，方便客户自己添加更换活动图片。结果发现：添加了挂件点击保存，却一点反应也没有，难道是哪里冲突了，试试看编辑，点击一个已经存在的挂件，点击编辑，保存，居然可以保存！
  怎么办？
  一个小需求而已却遇上这种莫名其妙的问题肯定会疯了，难不成要全面检查是不是哪里冲突啦？
  不用！这边提供一个小小的办法，可以临时，快速的帮你解决这个问题，当然，这个办法可能有点稍微高一点难度，有点风险啦。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-6-visual-template-editing-error-no-way-to-save-a-simple-solution.html
published: true
post_date: 2013-10-23 23:32:30
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

今天帮客户处理一个商品详情页的挂件位，需要增加一个图片广告位(ad_pic)，方便客户自己添加更换活动图片。结果发现：添加了挂件点击保存，却一点反应也没有，难道是哪里冲突了，试试看编辑，点击一个已经存在的挂件，点击编辑，保存，居然可以保存！

怎么办？

一个小需求而已却遇上这种莫名其妙的问题肯定会疯了，难不成要全面检查是不是哪里冲突啦？

不用！这边提供一个小小的办法，可以临时，快速的帮你解决这个问题，当然，这个办法可能有点稍微高一点难度，有点风险啦。

请看下面做法。

既然，我们知道可以编辑跟保存，证明整个的保存机制是没有坏的，当然，坏的话，我们也是可以用这个办法解决的，只是更加高难度哈。

那么，要怎样很快填好这个挂件坑呢？

对我这个对ShopEX内核有点研究的人来说，第一个想到的肯定是数据库操作了，因为我们做的编辑都保存在数据库之中。

现在，请各位小伙伴们一起来打开数据库，找到sdb_widgets_set表，请随便找一行，这边以默认的模板来说明噢。
<table id="table_results">
<thead>
<tr>
<th>widgets_id</th>
<th>base_file</th>
<th>base_slot</th>
<th>base_id</th>
<th>widgets_type</th>
<th>widgets_order</th>
<th>title</th>
<th>domid</th>
<th>border</th>
<th>classname</th>
<th>tpl</th>
<th>params</th>
<th>modified</th>
<th>vary</th>
</tr>
</thead>
<tbody>
<tr class="odd marked">
<td class=" nowrap" align="right">1</td>
<td>user:1354864820/block/footer.html</td>
<td class=" nowrap" align="right">0</td>
<td>foot_a</td>
<td>links</td>
<td class=" nowrap" align="right">32</td>
<td>友情链接：</td>
<td></td>
<td>borders/border7.html</td>
<td></td>
<td>default.html</td>
<td>a:8:{s:5:"limit";s:2:"30";s:6:"colums";s:2:"10";s:...</td>
<td align="right"><i>NULL</i></td>
<td><i>NULL</i></td>
</tr>
</tbody>
</table>
上面的字段所代表的含义如下：
<table id="table_results" width="1198">
<thead>
<tr>
<th>widgets_id</th>
<th>base_file</th>
<th>base_slot</th>
<th>base_id</th>
<th>widgets_type</th>
<th>widgets_order</th>
<th>title</th>
<th>domid</th>
<th>border</th>
<th>classname</th>
<th>tpl</th>
<th>params</th>
<th>modified</th>
<th>vary</th>
</tr>
</thead>
<tbody>
<tr class="odd marked">
<td class=" nowrap" align="right">挂件ID,自动</td>
<td>此挂件所在的模板文件</td>
<td class=" nowrap" align="right">坑位顺序</td>
<td>挂件坑位的ID名称</td>
<td>挂件名字</td>
<td class=" nowrap" align="right">挂件排序</td>
<td>挂件的标题</td>
<td> 我也不知道</td>
<td>边框文件</td>
<td> 类名</td>
<td>挂件模板</td>
<td>关键设置的参数</td>
<td align="right"><i>NULL</i></td>
<td><i>NULL</i></td>
</tr>
</tbody>
</table>
其中base_file与tpl两个字段一定要分清楚，tpl指的是关键的模板就是在挂件配置界面上的“版块模板”。而base_file指的是挂件所在的模板文件，这个需要注意的是，虽然你在编辑index.html首页模板，但是那个挂件有可能在foot.html里面而不一定是在index.html里面，这个时候只能根据base_id来协助判断。

PS：特别提醒，如果您使用单独页的话那么base_file这个字段内则不再是代表模板文件，而是单独页的名字！不明白什么意思的话，最好新建一个单独页面，然后查看一下。

知道这些字段的含义我们就来说说，怎么操作数据库为我们的模板文件增加一个挂件噢。

我们先假定一些值：我们需要增加挂件的页面是“商品详细页”，我们的挂件所在的模板是product-product.html文件（页面怎么来？在模板编辑页面右上角有一个“创建新页面”，下拉框选择“商品详情页”，页面名称随便你填，我填写“product-widgets”，文件名要特别注意，这个就是真实的html文件的名字，我填写“product”，则系统自动在模板目录下面新建一个“product-product.html”的文件，这个文件很重要，在未来将填写进数据库的<strong>base_file</strong>字段。）

我们点击“product-product.html”这个项目的右边的“源码编辑”按钮进入源码模式，你会看到很多的代码，其中很多类似于如下形式的东西噢：

&lt;{widgets id="xxxxxxxxxx"}&gt;

这样子的东西，我们称之为坑位，就是放挂件的地方噢。那个xxxxxxxxxx就是这个坑位的ID名称，将来要填入上面数据库中的<strong>base_id</strong>字段。

按照正常流程，我们新建完成模板文件之后就可以使用可视化编辑来增加我们所需要的挂件了，但是，我们这边就是为了说明那个万分之一的不正常~~

我们使用数据库加入一个挂件，我们要增加一个广告图片的挂件，名字叫做ad_pic，那么我们又得到一个数据库的<strong>widgets_type</strong>字段的值了。

根据上面的字段说明我们可以得到我们需要插入的数据的SQL语句：

INSERT INTO `sdb_widgets_set`
(
`base_file`,
`base_slot`,
`base_id`,
`widgets_type`,
`widgets_order`,
`title`,
`domid`,
`border`,
`classname`,
`tpl`,
`params`,
`modified`,
`vary`
) VALUES(
'user:myTheme/product-product.html',
0,
'xxxxxxxxxx',
'ad_pic',
0,
'新手上路',
'',
'borders/border1.html',
'',
'default.html',
'a:0:{}',
NULL,
NULL
)

执行之后将新建一行新的记录，然后你打开product-product.html可视化编辑，就会发现那个本来空的坑位上出现了一个新的ad_pic挂件，只是没有任何的内容，需要配置哈~

（怎么添加挂件？点击可视化编辑，在页面上方有一个“添加模块”，点击之后选择需要的挂件，按照类型分类好了，请根据类型找到需要的挂件，点击需要放置的坑位即可出现配置界面。）

（怎么编辑挂件？鼠标移到需要编辑的挂件所在的坑位上，将会出现一个蓝色的外框，右边有“删除”，“编辑”选项，请点击编辑即可进入挂件配置界面，点击删除可以删除挂件。）

配置好挂件的内容之后记得点击上方的“保存修改”按钮。刷新页面即可看到效果了。

当然，大家想偷懒的话怎么办呢？

最简单的就是在可以编辑保存的系统上编辑好，将SQL记录导出，根据需要做一下适当的修改就可以了噢。这个就留给大家自由发挥~~

<strong><span style="text-decoration: underline;">最关键要提醒的是：修改之前请记得备份！！！</span></strong>