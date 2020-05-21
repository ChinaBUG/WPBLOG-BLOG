---
ID: 4159
post_title: >
  phpmyadmin-如何设置配置文件config.inc.php让我们访问多个数据库服务器
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/phpmyadmin-how-to-set-the-config-file-config-inc-php-lets-us-access-to-multiple-database-servers.html
published: true
post_date: 2015-08-07 12:27:20
---
最近，在做项目的时候，因为要在本地测试服务器，线上测试服务器，线上正式服务器之间来回切换，用软件比较麻烦，怎么办呢？

用我熟悉的phpmyadmin吧，可是，它不是默认访问本地的服务器的嘛？

好吧，介绍一下吧，这个时候我们需要对phpmyadmin做一下配置，让他支持多个服务器。

在phpmyadmin根目录内你会看到一个config.inc.php文件，如果没有的话，请自己新建吧。

然后请写下下面的代码，然后根据具体修改你的参数吧。
<pre>/* Servers configuration */
$i = 0;

/* Server: 192.168.1.1 [1] */
$i++;
$cfg['Servers'][$i]['verbose'] = '192.168.1.1';
$cfg['Servers'][$i]['host'] = '192.168.1.1';
$cfg['Servers'][$i]['port'] = '';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysql';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['user'] = 'root';
$cfg['Servers'][$i]['password'] = '';

$i++;
$cfg['Servers'][$i]['verbose'] = '127.0.0.1';
$cfg['Servers'][$i]['host'] = '127.0.0.1';
$cfg['Servers'][$i]['port'] = '';
$cfg['Servers'][$i]['socket'] = '';
$cfg['Servers'][$i]['connect_type'] = 'tcp';
$cfg['Servers'][$i]['extension'] = 'mysql';
$cfg['Servers'][$i]['auth_type'] = 'cookie';
$cfg['Servers'][$i]['user'] = 'root';
$cfg['Servers'][$i]['password'] = '';

/* End of servers configuration */

$cfg['DefaultLang'] = 'zh_CN';
$cfg['ServerDefault'] = 1;
$cfg['blowfish_secret'] = '55adb965813297.35452261';
$cfg['UploadDir'] = '';
$cfg['SaveDir'] = '';
</pre>
然后，phpmyadmin的登陆界面就多了一个选择服务器的选项了~~里面显示了你定义的服务器。