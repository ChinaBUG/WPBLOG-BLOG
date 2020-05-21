---
ID: 3409
post_title: >
  OA-怎么使用Notepad++正则替换数字
author: ChinaBUG
post_excerpt: |
  今天，在开发PHP程序时需要一些数据库数据，导出原先的数据发现自动输出了一个ID而我是不希望有这个ID字段的噢，导出的数据部分如下：
  提示：其实使用phpmyadmin是可以控制导出的字段的，只是默认的话都是直接查询全部，所以就全部导出了~
  [sql]
  ......
  (130, 'PTSSC', '2X_1_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
  (131, 'PTSSC', '2X_2_2xzhxhz', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
  (132, 'PTSSC', '2X_2_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
  (133, 'PTSSC', '2X_1_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
  (134, 'PTSSC', '2X_1_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
  (135, 'PTSSC', '2X_1_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
  (136, 'PTSSC', '2X_2_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
  (137, 'PTSSC', '2X_2_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
  (138, 'PTSSC', '2X_2_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL);
  ......
  [/sql]
  就是130,131......这些数字我要删除掉噢，一个个删除是可以的，怎么批量删除？
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/oa-how-to-use-notepad-to-replace-numbers.html
published: true
post_date: 2014-04-11 16:08:37
---
今天，在开发PHP程序时需要一些数据库数据，导出原先的数据发现自动输出了一个ID而我是不希望有这个ID字段的噢，导出的数据部分如下：
<blockquote><em>提示：其实使用phpmyadmin是可以控制导出的字段的，只是默认的话都是直接查询全部，所以就全部导出了~</em></blockquote>
[sql]
......
(130, 'PTSSC', '2X_1_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
(131, 'PTSSC', '2X_2_2xzhxhz', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
(132, 'PTSSC', '2X_2_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
(133, 'PTSSC', '2X_1_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
(134, 'PTSSC', '2X_1_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
(135, 'PTSSC', '2X_1_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
(136, 'PTSSC', '2X_2_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
(137, 'PTSSC', '2X_2_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
(138, 'PTSSC', '2X_2_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL);
......
[/sql]

就是130,131......这些数字我要删除掉噢，一个个删除是可以的，怎么批量删除？

一直用Notepad++来编辑东西就是还没用过里面的替换，今天就拿来开刀哈~~系统自带的记事本比较简陋是没有这个功能的噢。

搜索、替换或者Ctrl+H打开替换窗口，作如下设置：

查找目标：\(\d*,
替换为：\(
查找模式：正则表达式

然后点击全部替换，然后你就得到了干净的数据了~~

[sql]
......
('PTSSC', '2X_1_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
('PTSSC', '2X_2_2xzhxhz', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
('PTSSC', '2X_2_2xzhxkd', NULL, '500', '500', '500', NULL, '170', '180', '190', NULL),
('PTSSC', '2X_1_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
('PTSSC', '2X_1_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
('PTSSC', '2X_1_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
('PTSSC', '2X_2_2xzxfs', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
('PTSSC', '2X_2_2xzxhz', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL),
('PTSSC', '2X_2_2xzxbd', NULL, '500', '500', '500', NULL, '85', '90', '95', NULL);
......
[/sql]

厉害吧~~记得要使用这个功能最好就是多学一下正则表达式，不懂得话可以看看本博客内的相关文章噢

<a title="正则表达式语法" href="http://blog.ipodmp.com/ms-regexp-grammar/">正则表达式语法</a>
<a title="常用正则表达式" href="http://blog.ipodmp.com/archives/common-regular-expressions/">常用正则表达式</a>
<a title="JavaScript-JS的replace方法与正则表达式结合应用讲解" href="http://blog.ipodmp.com/archives/javascript-js-replace-regular-expression-application-to-explain/">JavaScript-JS的replace方法与正则表达式结合应用讲解</a>

<em>多学一点，方便一点</em>