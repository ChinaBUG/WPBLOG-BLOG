---
ID: 3692
post_title: PHP-八大安全函数解析
author: ChinaBUG
post_excerpt: |
  1. mysql_real_escape_string()
  2. addslashes()
  3. htmlentities()
  4. htmlspecialchars()
  5. strip_tags()
  6. md5()
  7. sha1()
  8. intval()
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/php-eight-security-function.html
published: true
post_date: 2014-11-13 10:50:26
---
<strong>1. mysql_real_escape_string()</strong>

这个函数对于在PHP中防止SQL注入攻击很有帮助，它对特殊的字符，像单引号和双引号，加上了“反斜杠”，确保用户的输入在用它去查询以前已经是安全的了。但你要注意你是在连接着数据库的情况下使用这个函数。

但现在mysql_real_escape_string()这个函数基本不用了，所有新的应用开发都应该使用像PDO这样的库对数据库进行操作，也就是说，我们可以使用现成的语句防止SQL注入攻击。

<strong>2. addslashes()</strong>

这个函数和上面的mysql_real_escape_string()很相似。但要注意当 设置文件php.ini中的magic_quotes_gpc 的值为“on”时，不要使用这个函数。默认情况 下， magic_quotes_gpc 为 on，对所有的 GET、POST 和 COOKIE 数据 自动运行 addslashes()。不要对 已经被 magic_quotes_gpc 转义过的字符串使用 addslashes()，因为这样会导致 双层转义。你可以通过PHP中 get_magic_quotes_gpc()函数检查这个变量的值。

<strong>3. htmlentities()</strong>

这个函数对过滤用户输入数据非常有用，它可以把字符转换为 HTML 实体。比如，当用户输入字符“&lt;”时，就会被该函数转化为HTML实体&lt;，因此防止了XSS和SQL注入攻击。

<strong>4. htmlspecialchars()</strong>

HTML中的一些字符有着特殊的含义，如果要体现这样的含义，就要被转换为HTML实体，这个函数会返回转换后的字符串，比如，‘&amp;’amp会转为‘&amp;’。

<strong>5. strip_tags()</strong>

这个函数可以去除字符串中所有的HTML，JavaScript和PHP标签，当然你也可以通过设置该函数的第二个参数，让一些特定的标签出现。

<strong>6. md5()</strong>

一些开发者存储的密码非常简单，这从安全的角度上看是不好的，md5()函数可以产生给定字符串的32个字符的md5散列，而且这个过程不可逆，即你不能从md5()的结果得到原始字符串。

<strong>7. sha1()</strong>

这个函数和上面的md5()相似，但是它使用了不同的算法，产生的是40个字符的SHA-1散列（md5产生的是32个字符的散列）。

<strong>8. intval()</strong>

不要笑，我知道这不是一个和安全相关的函数，它是在将变量转成整数类型。但是，你可以用这个函数让你的PHP代码更安全，特别是当你在解析id，年龄这样的数据时。