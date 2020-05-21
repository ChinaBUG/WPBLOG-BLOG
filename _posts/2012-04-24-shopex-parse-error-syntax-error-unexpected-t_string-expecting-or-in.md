---
ID: 2036
post_title: 'ShopEX-Parse error: syntax error, unexpected T_STRING, expecting &#8216;,&#8217; or &#8216;;&#8217; in'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/04/shopex-parse-error-syntax-error-unexpected-t_string-expecting-or-in.html
published: true
post_date: 2012-04-24 15:37:43
---
Parse error: syntax error, unexpected T_STRING, expecting ',' or ';' in <strong>G:\webroot\htdocs\home\cache\front_tmpl\ef5b305454f798951818a39823a63143index.html-zh_CN.php</strong> on line <strong>1</strong>

悲剧的情况再次发生，为了新的活动修改了视图文件，然后就出现了上面的提示。明显，这是语法造成的错误。

经检查，发现，语法用错了。

错误的：&lt;{$rm.个性青年}&gt;

正确的：&lt;{$rm['个性青年']}&gt;