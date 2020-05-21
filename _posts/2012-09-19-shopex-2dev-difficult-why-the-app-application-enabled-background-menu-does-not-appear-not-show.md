---
ID: 2175
post_title: >
  ShopEX-疑难:为什么APP应用程序启用后后台菜单不出现没显示
author: ChinaBUG
post_excerpt: |
  当你后台的应用程序开启超过20个时，你会发现，菜单不显示了，只能显示部分的菜单，但是你通过网址链接时却发现程序是可以执行操作的。
  之所以会出现这种情况是因为程序对这个做了限制，不管有意无意，我们想要多一点应用程序就没办法了吗？二次开发一下就行。
layout: post
permalink: >
  http://blog.ipodmp.com/2012/09/shopex-2dev-difficult-why-the-app-application-enabled-background-menu-does-not-appear-not-show.html
published: true
post_date: 2012-09-19 11:45:04
---
当你后台的应用程序开启超过20个时，你会发现，菜单不显示了，只能显示部分的菜单，但是你通过网址链接时却发现程序是可以执行操作的。

之所以会出现这种情况是因为程序对这个做了限制，不管有意无意，我们想要多一点应用程序就没办法了吗？二次开发一下就行。

控制方法所在文件：core\include_v5\shop\admin.menu_filter.php

二次开发c/m/s/mdl.addons.php文件，重写getlist方法，将最小值改大一点即可，本例修改为50

&lt;?php

class cmd_addons extends mdl_addons{

function getList($cols,$filter='',$start=0,$limit=<span style="color: #ff0000;">50</span>,$orderType=null){

$ident=md5(var_export(func_get_args(),1));

if(!$this-&gt;_dbstorage[$ident]){

if(!$cols){

$cols = $this-&gt;defaultCols;

}

if(!empty($this-&gt;appendCols)){

$cols.=','.$this-&gt;appendCols;

}

$orderType = $orderType?$orderType:$this-&gt;defaultOrder;

$sql = 'SELECT '.$cols.' FROM '.$this-&gt;tableName.' WHERE '.$this-&gt;_filter($filter);

&nbsp;

if($orderType)$sql.=' ORDER BY '.(is_array($orderType)?implode($orderType,' '):$orderType);

$this-&gt;_dbstorage[$ident]=$this-&gt;db-&gt;selectLimit($sql,$limit,$start);

}

return $this-&gt;_dbstorage[$ident];

}

}?&gt;