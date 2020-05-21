---
ID: 4351
post_title: >
  UBuntu-我的Linux入门日志03-常用技巧及命令
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2016/02/ubuntu-getting-started-with-my-linux-log-03-common-skills-and-command.html
published: true
post_date: 2016-02-19 16:05:38
---
<h2>命令相关</h2>
<table>
<tbody>
<tr>
<td>命令</td>
<td>说明</td>
</tr>
<tr>
<td>$ !!</td>
<td>上一个命令</td>
</tr>
<tr>
<td>$ !ls</td>
<td>上一个命令集合的最后一个（ls -a，ls -h 则执行ls -h）</td>
</tr>
<tr>
<td>$ sudo !!</td>
<td>以root权限执行上一个命令</td>
</tr>
<tr>
<td>$ cd &#8211;</td>
<td>返回上一次目录 等同于cd $OLDPWD</td>
</tr>
<tr>
<td>$ cat /proc/meminfo</td>
<td>内存使用情况</td>
</tr>
<tr>
<td>$ ipcs -m</td>
<td>查看当前共享内存页面</td>
</tr>
<tr>
<td>$ ps ax</td>
<td>查看当前进程</td>
</tr>
<tr>
<td>$ ls -al sda*</td>
<td>查看硬件设备</td>
</tr>
</tbody>
</table>
<h2>快捷键相关</h2>
<table>
<tbody>
<tr>
<td>命令</td>
<td>说明</td>
</tr>
<tr>
<td>Ctrl+L</td>
<td>清屏</td>
</tr>
<tr>
<td>Ctrl+A</td>
<td>行首</td>
</tr>
<tr>
<td>Ctrl+U</td>
<td>删除光标处到行首</td>
</tr>
<tr>
<td>Ctrl+E</td>
<td>行尾</td>
</tr>
<tr>
<td>Ctrl+C</td>
<td>中断命令运行</td>
</tr>
<tr>
<td>Ctrl+Z</td>
<td>命令后台运行</td>
</tr>
</tbody>
</table>
<hr />
<h2>常用命令详解</h2>
<h3>wget</h3>
<p>完整镜像<a href="http://blog.ipodmp.com/html/archives/tag/wang-zhan/" title="View all posts in 网站" target="_blank">网站</a>：</p>
<p>wget &#8211;mirror -p &#8211;convert-links http://<a href="http://blog.ipodmp.com/html/archives/tag/mootools/" title="View all posts in mootools" target="_blank">mootools</a>.net/more/docs/1.5.2</p>
<p>如果下载大容量文件，则可以后台执行：</p>
<p>wget https://dl.google.com/dl/android/studio/ide-zips/2.0.0.4/android-studio-ide-143.2489090-windows.zip <strong>&amp;</strong></p>
<p>查看下载进度则使用：</p>
<p>tail -f wget-log</p>
<p>服务器迁移</p>
<p>wget -nH -m –ftp-user=your_username –ftp-password=your_password ftp://your_ftp_host/*</p>
<p>解释：<br />
-nH：不创建以主机名命名的目录。<br />
–cut-dirs：希望去掉原来的目录层数，从根目录开始计算。如果想完全保留FTP原有的目录结构，则不要加该参数。<br />
-m：下载所有子目录并且保留目录结构。<br />
–ftp-user：FTP用户名<br />
–ftp-password：FTP密码<br />
ftp://*.*.*.*/*：FTP主机地址。最后可以跟目录名来下载指定目录。</p>
<hr />
<h3>rpm2cpio</h3>
<p>如何不安装rpm包，就能解压查看rpm包里面的文件呢？使用下面的命令就行，他会将rpm包里面的etc和usr等文件夹解压出来，所以，使用的时候要注意，这个命令是解压到当前目录噢。</p>
<p>rpm2cpio galaxy-2.4.6-shopex.x86_64.rpm | cpio -div</p>
<hr />
<ul>
<li><a href="http://anxietyclub.accountant/">tadalafil 5 mg prezzo</a></li>
<li><a href="http://cialis30daysample.faith/">cialis 5mg 28st fta</a></li>
<li><a href="http://unhappyclub.accountant/">can you order cialis online for canada</a></li>
<li><a href="http://orderinglevitragood.bid/">cialis generico que es</a></li>
<li><a href="http://kamagrafast.accountant/">cheap cialis online uk</a></li>
<li><a href="http://sexyfeelingtabletsnamesmedicine.accountant/">is it easy to get a viagra prescription</a></li>
<li><a href="http://viagraalternative.date/">viagra</a></li>
</ul>