---
ID: 2833
post_title: PHP-Access forbidden!Error 403
author: ChinaBUG
post_excerpt: |
  Access forbidden!
  You don't have permission to access the requested object. It is either read-protected or not readable by the server.
  If you think this is a server error, please contact the webmaster.
  Error 403
  127.0.0.2
  Apache/2.4.4 (Win32) OpenSSL/0.9.8y PHP/5.4.19
  前些日子重新安装一键PHP环境WAMP，然后发现没办法多站点（其实是我不知道吧），按照PHPNow的经验修改了一下配置文件：
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/php-access-forbidden-error-403.html
published: true
post_date: 2013-11-27 10:45:09
---
<h1>Access forbidden!</h1>
You don't have permission to access the requested object. It is either read-protected or not readable by the server.

If you think this is a server error, please contact the <a href="mailto:webmaster@dummy-host2.example.com">webmaster</a>.
<h2>Error 403</h2>
<address><a href="http://127.0.0.2/">127.0.0.2</a>
Apache/2.4.4 (Win32) OpenSSL/0.9.8y PHP/5.4.19</address>
<pre>前些日子重新安装<a href="http://blog.ipodmp.com/archives/one-click-php/">一键PHP环境</a>WAMP，然后发现没办法多站点（其实是我不知道吧），按照PHPNow的经验修改了一下配置文件：

&lt;VirtualHost *:80&gt;
ServerAdmin webmaster@dummy-host2.example.com
DocumentRoot "E:/PHPnow/127.0.0.2_phpmyadmin"
ServerName 127.0.0.2
ErrorLog "logs/dummy-host2.example.com-error.log"
CustomLog "logs/dummy-host2.example.com-access.log" common
&lt;/VirtualHost&gt;</pre>
然后就会发现出现上面的错误提示了，经过查找，哇咧，终于被我解决了，只要把代码修改为下面的就可以了~~
<pre>&lt;VirtualHost *:80&gt;
ServerAdmin webmaster@dummy-host2.example.com
DocumentRoot "E:/PHPnow/127.0.0.2_phpmyadmin"
ServerName 127.0.0.2
ErrorLog "logs/dummy-host2.example.com-error.log"
CustomLog "logs/dummy-host2.example.com-access.log" common
    &lt;Directory "E:/PHPnow/127.0.0.2_phpmyadmin"&gt;
        AllowOverride All
        Options None
        Require all granted
    &lt;/Directory&gt;
&lt;/VirtualHost&gt;</pre>
记录一下留作纪念~~