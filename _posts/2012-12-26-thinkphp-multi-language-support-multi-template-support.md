---
ID: 2235
post_title: >
  ThinkPHP-设置thinkphp多语言支持，多模板支持
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/12/thinkphp-multi-language-support-multi-template-support.html
published: true
post_date: 2012-12-26 17:33:05
---
ThinkPHP多语言支持:

config.php配置文件中添加

//多语言支持设置
'LANG_SWITCH_ON'=&gt;true,
'DEFAULT_LANG'=&gt;'zh-cn',
'LANG_AUTO_DETECT'=&gt;true,
'LANG_LIST'=&gt;'en-us,zh-cn,zh-tw',

Home/Lang/文件夹下建立三个文件夹，分别为zh-cn ，en-us ，zh-tw 分别代表简体中文，英文，繁体中文

文件夹下可以建立与模板对应的文件，或者公用文件common.php

zh-cn/common.php

&lt;?php
return array(
'welcome'=&gt;'你好',
'lan'=&gt;'简体中文',
);
?&gt;

en-us/common.php

&lt;?php
return array(
'welcome'=&gt;'how are you fine?',
'lan'=&gt;'english',
);
?&gt;

zh-tw/common.php

&lt;?php
return array(
'welcome'=&gt;'你好',
'lan'=&gt;'簡體中文',
);
?&gt;

模板index.php

欢迎：{$Think.lang.welcome} 语言：{$Think.lang.lan}

&lt;a href="?l=zh-cn"&gt;简体中文&lt;/a&gt;
&lt;a href="?l=en-us"&gt;english&lt;/a&gt;
&lt;a href="?l=zh-tw"&gt;繁體中文&lt;/a&gt;

或者在Action的方法里直接定义：L('demo','测试');这样，在模板里就可以直接应用了：{$Think.lang.demo}
对于在模型中，比如有：array('uname','require','用户名必填');可以这么用：array('uname','require','%name');


ThinkPHP多模板支持：

config.php配置文件中添加：

//多模板支持
'TMPL_SWITCH_ON'=&gt;true,
'TMPL_DETECT_THEME'=&gt;true,

/Home/Tpl/下建立其它皮肤文件夹，比如文件夹red，其中的文件与default文件中的一样。

在模板文件中添加:
&lt;a href="?t=red"&gt;红&lt;/a&gt;
&lt;a href="?t=default"&gt;默认&lt;/a&gt;