---
ID: 3214
post_title: >
  ShopEX-4.8.5.78660集成Discuz时会出现错误，无法启用插件
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/shopex-4-8-5-78660-errors-occur-when-integrated-discuz-not-enable-plug-in.html
published: true
post_date: 2014-03-10 11:43:56
---
最近在做Discuz论坛集成的时候，发现这个版本参数设置完毕之后，启用状态一直是错误的，查看数据库发现值是正确的，但是前台一直无法正确的会员同步~

查看一下代码，重新写了一个获取的方法，结果问题好了~~~

将下面代码保存为cmd.passport.php文件放在二开目录的model目录的member内
<blockquote>class cmd_passport extends mdl_passport {
//2014.03.10 无法保存，不知道为什么
function getCurrentPlugin() {
return $this-&gt;system-&gt;getConf('plugin.'.$this-&gt;plugin_name.'.config.current_use');
}
}</blockquote>
我猜测可能是这个文件之前是被人修改过的吧~具体没有去破解mdl.passport.php文件所以不确定哈~