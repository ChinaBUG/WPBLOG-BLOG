---
ID: 4267
post_title: UBuntu-我的Linux入门日志01
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/10/ubuntu-getting-started-with-my-linux-log-01.html
published: true
post_date: 2015-10-25 15:34:25
---
多年前，尝试了红帽的Linux系统，但是因为工作的性质没有用心学习下去（那个时候使用的是ASP脚本开发WEB网站），自从使用PHP开发应用到现在，越来越多被提及需要在Linux环境下工作的经验，虽然我有，但是问及熟悉，操作，配置等方面的经验，哑口无言呀，因为确实没有真正的去熟悉，熟练使用过。

所以，十月国庆后，我给自己布置的任务是要开始从零学习Linux了。

今天来公司加班一下，安装一下我的UBuntu吧。

安装其实没啥说的，创建虚拟机，然后一步步的安装下去，如果有问题的话可以网上查找安装教程，需要提醒的是，需要创建一个替代root账户的用户名，并设置账户，我在这个地方重新安装了两次才知道这边设置的是登录账户，密码不是root的密码，为了识别简单，我取名root1跟密码是root1，偷懒一下~

然后，重启之后就会进入登陆界面。

这个时候就要使用上面设置的账户跟密码，我这边是root1跟root1。

另外，区别于windows的是输入密码是隐藏的，要输入正确噢。

登陆之后，我们就可以使用命令了。

然后，是不是觉得很奇怪root账号密码是多少呢？

问一下度娘，发现《<a href="http://jingyan.baidu.com/article/5225f26b0ac250e6fb09084e.html">UBUNTU的默认root密码是多少，修改root密码</a>》

好吧，直接在命令行下面输入命令吧：

$<strong> sudo</strong> <strong>passwd</strong> root

根据提示操作呗，设置完毕。这个时候可以切换到root账号下操作：

$<strong> su</strong> root

根据提示输入密码就可以了。

当然，我们也可以切换到root1账号下，这个时候是不需要输入密码的。

早上，在51高招的微信公众账号里面看到几个Linux的命令，其中一个是sl命令，显示一个奔跑的火车哈，来安装一下呗。

# <strong>apt-get</strong> install sl

或者

$ <strong>sudo</strong> <strong>apt-get</strong> install sl

然后出现了下面的错误

ubuntu unable to locate package

问一下度娘哈~《<a href="http://blog.csdn.net/woaixiaozhe/article/details/7925409">Unable to locate package错误解决办法</a>》

不错，试一下：

# <strong>apt-get</strong> update

或者

$ <strong>sudo apt-get</strong> update

然后在执行就好了，感谢博主哈~

接着我们能干吗呢？

不懂命令怎么办呢？

试试help吧

$ <strong>help</strong>

或者

# <strong>help</strong>

注：<strong>$</strong>一般用户；<strong>#</strong>超级用户

然后会看到好多命令噢，随便找个呗，我比较熟悉里面的exit命令哈，

$ <strong>exit</strong>

退出登录状态哈。

我们查看一下文件列表吧：

$<strong> ls</strong>

然后，额没有内容？

好吧，用下面的命令吧

$ <strong>ls</strong> /

注：/ 根目录；./当前目录

然后会列出我们可以用的目录文件资源了。
<blockquote><strong><em>格式：ls [参数] [文件/目录]</em></strong>
<em>参数说明:</em>
<em>-a 表示列出所有的文件，包括以"."开头的隐藏文件</em>
<em>-d 如果其后接的是一个目录，则此只输出目录的名称</em>
<em>-l 表示以清单的形式列出文件的条目，包括文件的名称、权限、拥有者、大小、最后修改时间等</em>
<em>-t 表示列出的条目按最后修改的时间进行排序，默认是使用文件夹的名称来排序</em>
<em>-C 以文件的名称按列纵向排序</em>
<em>-F 在文件名后加一个符号来表示文件类型</em></blockquote>
为什么上面ls后没有内容呢？

其实，你只需要将命令改为

$ <strong>ls</strong> -a

增加a参数，显示隐藏文件，然后你会看到好多文件，文件前面都有一个点噢。

为什么呢？我们仔细看一下$命令行的后面，你会发现默认的是~$有一个波浪线符号对吧，

我们来试一下

$<strong> cd</strong> /

你会发现变成/$了，对吧。
<blockquote>《<a href="http://www.crifan.com/linux_change_directory_command_cd/">【整理】详解Linux中的切换路径命令：cd</a>》</blockquote>
~这个符号表示的是我们的用户根目录。

就是代表/home/root1这个目录了，我们可以输入

:~# cd /home/root1

:/home/root1# ls -a

看到的内容就是上面我们查看到的内容一样噢。

具体的说明我找了一份资料，很不错《<a href="http://www.crifan.com/linux_use_tilde_represent_user_home_directory_and_why/">【整理】Linux系统中用波浪号~表示用户的根目录即$HOME，以及为何用波浪号表示用户根目录</a>》，阅读下呗。

然后今天就这样子吧，下次再来分享。