---
ID: 1331
post_title: >
  ShopEX-二次开发之前台会员菜单与后台管理菜单的定义与修改
author: ChinaBUG
post_excerpt: |
  $menu['setting']['items'][]之中setting的取值范围为下面任意一个：array( "goods", "order", "member", "sale", "site", "analytics", "setting", "tools")，各自对应后台的8大项目，其中还有一类，不过是跟系统后台设置控制的，这边不列出来。
  下面是整个的调用代码噢：
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/shopex-member-menu-background-menu-definition-and-management.html
published: true
post_date: 2011-06-15 16:43:30
---
$menu['setting']['items'][]之中setting的取值范围为下面任意一个：array( "goods", "order", "member", "sale", "site", "analytics", "setting", "tools")，各自对应后台的8大项目，其中还有一类，不过是跟系统后台设置控制的，这边不列出来。

下面是整个的调用代码噢：

这个是后台的菜单，在商店设置里面找哈~

新建/core2/include/customSchema.php文件，然后写下代码

$menu['setting']['items'][] = array(
"type" =&gt; "group",
"label" =&gt; "特殊设置",
"items" =&gt; array(
array( "type" =&gt; "menu", "label" =&gt; "配货方式限制", "link" =&gt; "index.php?ctl=shopex2/delivery&amp;act=setting" )
)
);

当然，你也可以在做插件时增加这个菜单的，那么就需要使用下面格式：

function getMenu(&amp;$menu){
$menu['setting']['items'][] = array(
"type" =&gt; "group",
"label" =&gt; "特殊设置",
"items" =&gt; array(
array( "type" =&gt; "menu", "label" =&gt; "配货方式限制", "link" =&gt; "index.php?ctl=shopex2/delivery&amp;act=setting" )
)
);
}

进阶：

问：如果你问，我不新开一个项，想要把新开发的项目放在原先已经存在的栏目里面怎么办？

答：这个要做到很简单的，因为菜单数组，既然是数组，那就能访问呗，来吧，我们加在第一项的后面吧，把上面的代码修改一下：

$menu['setting']['items']<span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">[0]['items'][]</span></span> = array( "type" =&gt; "menu", "label" =&gt; "配货方式限制", "link" =&gt; "index.php?ctl=shopex2/delivery&amp;act=setting" );

现在配货方式限制这个菜单就跑第一大项里面的最后面一个了。

这个是前台会员中心的菜单哈，在最后面一个，定义在cct.member.php的主函数内即可：

$this-&gt;map[] = array('label'=&gt;'邀请注册',
'items'=&gt;array(
array('label'=&gt;'邀请链接','link'=&gt;'link'),
array('label'=&gt;'下线会员','link'=&gt;'ermembers'),
array('label'=&gt;'开始抽奖','link'=&gt;'choujiang'),
array('label'=&gt;'抽奖记录','link'=&gt;'choujiangjilu')
));