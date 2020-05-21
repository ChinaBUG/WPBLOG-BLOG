---
ID: 1633
post_title: >
  密码保护：ShopEX-二次开发-后台管理-统计报表销售统计销售量(额)排名的扩展:按照日期查询
author: ChinaBUG
post_excerpt: |
  案例描述：
  ShopEX统计报表下的销售统计之销售量(额)排名只有两种查询模式，一种是按照查询的订单号或者是用户名来统计，一种是全部显示到至今位置的全部销售量（额）统计。那么要查询当天或者任意一个时间段的订单销售额怎么办呢？
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-manage-statistical-reports-sales-statistics-sales-turnover-ranking-of-expansion-query-by-date.html
published: true
post_date: 2011-07-15 13:51:44
---
案例描述：

ShopEX统计报表下的销售统计之销售量(额)排名只有两种查询模式，一种是按照查询的订单号或者是用户名来统计，一种是全部显示到至今位置的全部销售量（额）统计。那么要查询当天或者任意一个时间段的订单销售额怎么办呢？

&nbsp;

没办法，自己二次开发吧。

具体操作如下：

第一步 修改视图文件

请先找到core\admin\view\sale\count\salescountall.html文件，找到

关键字 &lt;{input name="dosearch" size=15 required="true"}&gt;

这段代码，将之修改为下面的代码

关键字 &lt;{input name="dosearch" size=15}&gt;

然后把这个文件保存到你的二次开发目录之中的相应目录之中，可以为core2\admin\view\sale\count\salescountall.html这样子的，只要路径正确即可。

说明：就是把代码里面的required="true"给去掉，这个代码是验证是否为空的所在。

第二步 修改程序文件

在二次开发的相应目录之中，新建core2\admin\controller\sale\cct.salescount.php文件（若存在直接打开）然后，写下下面的代码

&lt;?php
require(CORE_DIR.'/admin/controller/sale/ctl.salescount.php');
class cct_salescount extends ctl_salescount{
var $workground ='analytics';
function countall(){

if($_GET['ordertype'] &amp;&amp; $_GET['method']){
$order['order']=intval($_GET['ordertype']);
$order['method']=intval($_GET['method']);
}
$sales=&amp;$this-&gt;system-&gt;loadModel('utility/salescount');
if($_GET['dosearch']){
$dateFrom=strtotime($_GET['searchfrom']);
$dateTo=strtotime($_GET['searchto']);
$search=$_GET['dosearch'];
$item=$_GET['searchitem'];
$result=$sales-&gt;count_all($dateFrom,$dateTo,$item,$search,$order);
$this-&gt;pagedata['plug']='&amp;dosearch='.$_GET['dosearch'].'&amp;searchitem='.$_GET['searchitem'].'&amp;searchfrom='.$_GET['searchfrom'].'&amp;searchto='.$_GET['searchto'];
}<span style="color: #ff0000;">elseif(isset($_GET['searchfrom'])&amp;&amp;$_GET['searchto']){//扩展：按照日期统计 @ ChinaBUG 2011.07.15</span>
<span style="color: #ff0000;">            $dateFrom=strtotime($_GET['searchfrom']);</span>
<span style="color: #ff0000;">            $dateTo=strtotime($_GET['searchto']);</span>
<span style="color: #ff0000;">            $result=$sales-&gt;count_all($dateFrom,$dateTo,'','',$order);</span>
}else {
$dateFrom=strtotime(date("Y").'-01-01');
$dateTo=strtotime("+1 year",$dateFrom);
$result=$sales-&gt;count_all($dateFrom,$dateTo,'','',$order);
}
if($_GET['genexml']==1){
$dataio = $this-&gt;system-&gt;loadModel('system/dataio');
$io="xls";

$name=array('商品名称','销售量','销售额');
$dataio-&gt;export_begin("xls",$name,'商品销售量(额)',count($result));
$dataio-&gt;export_rows($io,$result);
$dataio-&gt;export_finish('xls');
exit();
}
$this-&gt;pagedata['data']=$result;
$this-&gt;pagedata['method']=$_GET['method'];
$this-&gt;page('sale/count/salescountall.html');
}
}
?&gt;

说明：注意红色部分，这个就是我添加的功能，作用就是一个不用输入订单号跟用户就可以查询指定时间内的数量统计。