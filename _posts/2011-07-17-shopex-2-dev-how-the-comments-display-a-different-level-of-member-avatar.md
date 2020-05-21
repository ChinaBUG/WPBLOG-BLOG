---
ID: 1663
post_title: >
  ShopEX-二次开发-如何在评论时根据会员的等级显示不同的头像
author: ChinaBUG
post_excerpt: |
  如何给不同的会员等级设置不同的头像，这个需求的难点是怎么获得会员的级别，索性ShopEX已经提供一个变量给我们，让我们知道登陆的会员是什么级别的。
  保存会员登录信息的变量为$this->member，如果输出的话，他的结构是如下：
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-2-dev-how-the-comments-display-a-different-level-of-member-avatar.html
published: true
post_date: 2011-07-17 12:05:47
---
如何给不同的会员等级设置不同的头像，这个需求的难点是怎么获得会员的级别，索性ShopEX已经提供一个变量给我们，让我们知道登陆的会员是什么级别的。

保存会员登录信息的变量为$this-&gt;member，如果输出的话，他的结构是如下：

Array(
[member_id] =&gt; 402
[member_lv_id] =&gt; 1
[email] =&gt; seaone@qq.com
[uname] =&gt; chinabug
[b_year] =&gt; 1981
[b_month] =&gt; 7
[b_day] =&gt; 10
[unreadmsg] =&gt; 0
[cur] =&gt; CNY
[lang] =&gt;
[point] =&gt; 1394
[experience] =&gt; 55
)

其中member_lv_id这个就是会员的级别啦，这个值是后台我们设置会员级别的时候那个级别多代表的ID噢。

PS：怎么获取这个会员级别的ID？进入后台，进入会员等级，在会员级别列表里面找到你希望了解的会员级别，点击在新窗口里查看，然后会弹出新的窗口，你看到新窗口的地址里面会有一串的字符：.../index.php?ctl=member/level&amp;act=detail&amp;p[0]=1，最后面的1就是这个级别的ID啦，这个就是显示在上面的member_lv_id里面。

假设我们的会员级别有四个，普通会员、分销会员、高级会员、VIP会员，代表的ID分别为1、2、3、4吧。

现在我们设定普通会员的头像是一只元宝，分销会员是两只元宝，以此类推，VIP会员就是四个元宝，请把图片准备好，我们放到根目录的statics文件夹里面，新建一个文件夹名字就叫memberlv吧，那么四个级别的图片分别为：statics/memberlv/1.png、statics/memberlv/2.png、statics/memberlv/3.png、statics/memberlv/4.png。

只要我们获取级别，然后输出对应的图片即可。

部分代码：

$memberlv = array(
1=&gt;'statics/memberlv/1.png',
2=&gt;'statics/memberlv/2.png',
3=&gt;'statics/memberlv/3.png',
4=&gt;'statics/memberlv/4.png',
);

$this-&gt;pagedata['memberlv'] = $memberlv[$this-&gt;member['member_lv_id']];

在视图文件上要显示头像的地方写进&lt;img src="&lt;{$memberlv}&gt;"/&gt;然后浏览一下就显示出来了。

&nbsp;

&nbsp;