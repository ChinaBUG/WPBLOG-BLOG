---
ID: 4089
post_title: 'MySQL &#8211; 同一个页面内怎么使用两个数据库连接，连接两个数据库操作表'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/06/mysql-how-to-use-two-database-connections-within-the-same-page-connect-the-two-database-tables.html
published: true
post_date: 2015-06-23 15:01:09
---
最近修改一套系统，老板需要两个站的会员互相共享，好吧，两个网站放在同一个服务器上，这个需求比较简单的可以实现了。
<blockquote><em>简单的实现方法：
在A站的会员注册的处理方法里面增加B站的会员表的操作，记得检查用户名等信息的唯一性，方便直接对接。然后在B站同样如此处理。在A、B站会员中心，分辨针对资料修改做同步。
这样子，就省掉登录的多余判断。当然，如果，很不幸没有做好两站的会员唯一判断怎么办？那么就弄一个表记录会员信息的对应信息，然后.....</em></blockquote>
啰嗦了半天，离题了~

要实现上面的做法，我们避免不了在同一个页面内连接两个数据库，那么我们怎么做到这个需求呢？

我也是走了一段的弯路，被害死了~主要是基础不牢噢。

系统本身用一个mysql的类，我按照手册的说法修改了mysql_connect的第四个参数，允许新建连接，但是一直调试不过去~怎么回事呢？

找了大半天才发现，原来，漏掉一个了，除了这个方法需要增加第四个参数，同时mysql_query方法的第二个参数也不能是空的，因为它为空则会调用上一次的链接，就会造成数据是不正确的。

哎，用别人的类库方便，却经常会被一些东西影响了效率，我时常在想要不要自己弄一个~可是，怎么知道我会没有错误呢？呵呵

附录：

$this-&gt;link = @mysql_connect($host, $user, $pass,$newlink);

mysql_query($sql, $this-&gt;link);

END~