---
ID: 4294
post_title: PCRE-Unknown modifier
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/11/pcre-unknown-modifier.html
published: true
post_date: 2015-11-10 18:49:51
---
下午用PHP使用正则的时候，写好正则执行后出现这个错误：

Warning: preg_match_all() [function.preg-match-all]: Unknown modifier '(' in E:WEBshanmai_sourcetest.php on line 5

难道不知道括号？不会呀，以前不是这样子写的嘛~

出错的正则是：

‘(q|wd)=([^&amp;s]*)’

额，好像忘记了什么，加一下界定符吧：

‘/(q|wd)=([^&amp;s]*)/’

执行，然后就可以了，哎，经常性的会忘记这个忘记那个~