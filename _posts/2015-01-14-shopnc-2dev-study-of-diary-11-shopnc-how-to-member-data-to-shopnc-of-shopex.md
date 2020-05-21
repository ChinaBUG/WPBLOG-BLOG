---
ID: 3871
post_title: >
  ShopNC二次开发研究日记11:怎么将ShopEX中的会员数据转移到ShopNC中呢
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/01/shopnc-2dev-study-of-diary-11-shopnc-how-to-member-data-to-shopnc-of-shopex.html
published: true
post_date: 2015-01-14 17:45:49
---
最近，公司的新站（基于ShopNC商城）筹备快完成了，需要进行各样的数据整合，比如旧站ShopEX内的会员数据需要转移到新站中，怎么做呢？

目前市场上没有这样的插件跟程序，那就只能自己来了~

这边说一下分析思路，然后插件跟程序就变得很简单了，没啥难度，唯一就是看做的多智能了。

要转移数据的办法有两种，一种是直接SQL操作，一种就是专门针对这个需求写一个程序。今天就不说怎么写程序了，就分析一下要怎么利用SQL语句来完成这个需求吧。

要写SQL语句，第一步就是必须获取相关操作的表的结构，请自行使用phpmyadmin查看并导出结构吧，下面是我已经导出的两份结构。

<del datetime="2015-01-14T09:35:42+00:00">注意：表结构内的字段有的是原版没有的是我自己开发过程增加的，请不要完全对照，偷懒，没有重装系统噢。</del>

第一步

我们需要先来熟悉一下SQL语句INSERT INTO的用法，这个命令好厉害噢，原本是用于向表格中插入新的行，但是在我们这边是使用它的进阶功能，其实他是可以用来复制表数据的，只是他要求操作的两个表都需要存在。

语法

INSERT INTO 表名称 VALUES (值1, 值2,....)

我们也可以指定所要插入数据的列：

[code lang="sql"]INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)[/code]

当然，我们还是可以使用下面的形式复制一个存在的表的数据给另外一个表的：

[code lang="sql"]INSERT INTO table_name (列1, 列2,...) SELECT 列1, 列2,... FROM table_name2[/code]

其中要求table_name,table_name2均要存在，要不是用不了的噢，另外，需要注意的是字段的顺序，insert into的字段顺序要与select到的字段顺序一致，在一致的情况下，select的字段名可以不要，直接用型号代替噢。

例如：

[code lang="sql"]INSERT INTO `b`(`member_id`,`foreign_id`,`member_refer`) SELECT * FROM `a`[/code]

明白这个用法之后我们就可以进入第二步了。

第二步

这一步很关键，关键在于都是苦力活，要是一不小心还真的可能出错噢！所以千万不要不小心，要不出现错误可不要来找我噢。

我们先找到目标表的需要的字段，组成字段列，比如ShopNC的需要的会员信息列是：

[code lang="sql"]`member_login_num`,
`member_time`,
`member_login_time`,
`member_old_login_time`,
`email_keycode`,
`email_checkemail`,
`member_name`,
`member_truename`,
`member_passwd`,
`member_email`,
`phone`,
`email_check`,
`member_points`,
`available_predeposit`,
`phonecheck`[/code]

这些就是我们需要的数据的字段名。

同样，我们需要找到来源表的数据字段，组成字段列，如果遇上名字不一样的你还需要做改写，如ShopEX所需要的会员信息的字段列是：

[code lang="sql"]`login_count` as member_login_num,
`regtime` as member_time,
`regtime` as member_login_time,
`regtime` as member_old_login_time,
`regtime` as email_keycode,
`regtime` as email_checkemail,
`uname` as member_name,
`name` as member_truename,
`password` as member_passwd,
`email` as member_email,
`mobile` as phone,
`emailcheck` as email_check,
`point` as member_points,
`advance` as available_predeposit,
`phonecheck` as phonecheck[/code]

可能有朋友会发现上面好几个的字段名都是重复，这个其实是没办法的，因为找不到相对应的字段，而该字段有是必填字段，不能为空，怎么办？只能随便赋值了~注意噢，这个随便赋值可不能太随便，起码字段类型要一致。

然后~根据上面的INSERT INTO语法组成如下的SQl语句：

[code lang="sql"]INSERT INTO `shopnc_member`(
`member_login_num`,
`member_time`,
`member_login_time`,
`member_old_login_time`,
`email_keycode`,
`email_checkemail`,
`member_name`,
`member_truename`,
`member_passwd`,
`member_email`,
`phone`,
`email_check`,
`member_points`,
`available_predeposit`,
`phonecheck`
) 
SELECT 
`login_count` as member_login_num,
`regtime` as member_time,
`regtime` as member_login_time,
`regtime` as member_old_login_time,
`regtime` as email_keycode,
`regtime` as email_checkemail,
`uname` as member_name,
`name` as member_truename,
`password` as member_passwd,
`email` as member_email,
`mobile` as phone,
`emailcheck` as email_check,
`point` as member_points,
`advance` as available_predeposit,
`phonecheck` as phonecheck
FROM `sdb_members`
[/code]

赋值这段SQL语句，使用phpmyadmin执行SQL语句之后你会发现，噢~数据已经正确的转移了~

注意：如果出现错误提示，请按照错误提示操作吧，一般错误都是出现在字段是否为空上。

对于你们，可能这个教程是已经完成了，但是对于我来说，还是没完成，毕竟线上线下的区别，还是需要做一些修正的。

比如，同事告诉我，如果ShopEX里面某个值等于某个值，那么就要设置某个字段，比如当email_check为1时，就需要更新字段email_checkemail的值为member_email的值：

[code lang="sql"]update `shopnc_member` set `email_checkemail`= member_email where `email_check`=1;[/code]

更新之后还需要输出结果看看是不是符合我们的设计需求：

[code lang="sql"]SELECT *  FROM `shopnc_member` WHERE `member_email` NOT LIKE '%@%' and `email_check`=1;[/code]

简单的看一下，没错，都按照要求处理好了。

因为SQL我不懂的怎么处理重复数值，所以，当两个表整合之后必定会出现重复的用户名，其他会员不计，咱自己的会员名总可能是重复的，怎么办？

使用下面的SQl语句就能列出来重复的会员：

[code lang="sql"]SELECT member_id, member_name
FROM shopnc_member
WHERE member_name
IN (
SELECT max( member_name )
FROM shopnc_member
GROUP BY member_name
HAVING Count( * ) >1
)
ORDER BY `member_name` ASC[/code]

然后，根据你喜欢的删除吧，要保留那个就保留那个了呗~

好了~问题解决。



表1：ShopNC的会员信息表

[code lang="sql"]--
-- 表的结构 `shopnc_member`
--

CREATE TABLE IF NOT EXISTS `shopnc_member` (
  `member_id` int(11) NOT NULL auto_increment COMMENT '会员id',
  `member_name` varchar(50) NOT NULL COMMENT '会员名称',
  `member_truename` varchar(20) default NULL COMMENT '真实姓名',
  `store_id` int(11) unsigned NOT NULL default '0' COMMENT '店铺id',
  `member_avatar` varchar(50) default NULL COMMENT '会员头像',
  `member_sex` tinyint(1) default NULL COMMENT '会员性别',
  `member_birthday` date default NULL COMMENT '生日',
  `member_passwd` varchar(32) NOT NULL COMMENT '会员密码',
  `member_email` varchar(100) NOT NULL COMMENT '会员邮箱',
  `member_qq` varchar(100) default NULL COMMENT 'qq',
  `member_ww` varchar(100) default NULL COMMENT '阿里旺旺',
  `member_login_num` int(11) NOT NULL default '1' COMMENT '登录次数',
  `member_time` varchar(10) NOT NULL COMMENT '会员注册时间',
  `member_login_time` varchar(10) NOT NULL COMMENT '当前登录时间',
  `member_old_login_time` varchar(10) NOT NULL COMMENT '上次登录时间',
  `member_login_ip` varchar(20) default NULL COMMENT '当前登录ip',
  `member_old_login_ip` varchar(20) default NULL COMMENT '上次登录ip',
  `member_goldnum` int(11) NOT NULL default '0' COMMENT '金币数',
  `member_goldnumcount` int(11) NOT NULL default '0' COMMENT '曾经拥有购买金币数',
  `member_goldnumminus` int(11) NOT NULL default '0' COMMENT '已经消费金币数',
  `member_qqopenid` varchar(100) default NULL COMMENT 'qq互联id',
  `member_qqinfo` text COMMENT 'qq账号相关信息',
  `member_sinaopenid` varchar(100) default NULL COMMENT '新浪微博登录id',
  `member_sinainfo` text COMMENT '新浪账号相关信息序列化值',
  `member_points` int(11) NOT NULL default '0' COMMENT '会员积分',
  `available_predeposit` decimal(10,2) NOT NULL default '0.00' COMMENT '预存款可用金额',
  `freeze_predeposit` decimal(10,2) NOT NULL default '0.00' COMMENT '预存款冻结金额',
  `inform_allow` tinyint(1) NOT NULL default '1' COMMENT '是否允许举报(1可以/2不可以)',
  `is_buy` tinyint(1) NOT NULL default '1' COMMENT '会员是否有购买权限 1为开启 0为关闭',
  `is_allowtalk` tinyint(1) NOT NULL default '1' COMMENT '会员是否有咨询和发送站内信的权限 1为开启 0为关闭',
  `member_state` tinyint(1) NOT NULL default '1' COMMENT '会员的开启状态 1为开启 0为关闭',
  `member_credit` int(11) NOT NULL default '0' COMMENT '会员信用',
  `member_snsvisitnum` int(11) NOT NULL default '0' COMMENT 'sns空间访问次数',
  `member_areaid` int(11) default NULL COMMENT '地区ID',
  `member_cityid` int(11) default NULL COMMENT '城市ID',
  `member_provinceid` int(11) default NULL COMMENT '省份ID',
  `member_areainfo` varchar(255) default NULL COMMENT '地区内容',
  `email_keycode` varchar(255) NOT NULL COMMENT '邮箱验证识别码，加密串',
  `email_check` tinyint(1) NOT NULL default '0' COMMENT '1已经验证',
  `email_checkemail` varchar(255) NOT NULL COMMENT '首次验证邮箱',
  `phone` varchar(50) NOT NULL COMMENT '手机',
  `phonecheck` tinyint(1) NOT NULL default '0' COMMENT '1已经验证，0未验证',
  PRIMARY KEY  (`member_id`),
  KEY `member_name` (`member_name`,`store_id`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8 COMMENT='会员表' AUTO_INCREMENT=35 ;
[/code]

表2：ShopEX4.85的会员信息表

[code lang="sql"]--
-- 表的结构 `sdb_members`
--

CREATE TABLE IF NOT EXISTS `sdb_members` (
  `member_id` mediumint(8) unsigned NOT NULL auto_increment,
  `firstphonecheck` varchar(50) default '0',
  `member_lv_id` mediumint(8) unsigned NOT NULL default '0',
  `uname` varchar(50) NOT NULL,
  `name` varchar(50) default NULL,
  `lastname` varchar(50) default NULL,
  `firstname` varchar(50) default NULL,
  `password` varchar(32) default NULL,
  `area` varchar(255) default NULL,
  `mobile` varchar(30) default NULL,
  `tel` varchar(30) default NULL,
  `email` varchar(200) NOT NULL,
  `zip` varchar(20) default NULL,
  `addr` varchar(255) default NULL,
  `province` varchar(20) default NULL,
  `city` varchar(20) default NULL,
  `order_num` mediumint(8) unsigned default '0',
  `refer_id` varchar(50) default NULL,
  `refer_url` varchar(200) default NULL,
  `refer_time` int(10) unsigned default NULL,
  `c_refer_id` varchar(50) default NULL,
  `c_refer_url` varchar(200) default NULL,
  `c_refer_time` int(10) unsigned default NULL,
  `b_year` smallint(5) unsigned default NULL,
  `b_month` tinyint(3) unsigned default NULL,
  `b_day` tinyint(3) unsigned default NULL,
  `sex` enum('0','1') NOT NULL default '1',
  `addon` longtext,
  `wedlock` enum('0','1') NOT NULL default '0',
  `education` varchar(30) default NULL,
  `vocation` varchar(50) default NULL,
  `interest` longtext,
  `advance` decimal(20,3) NOT NULL default '0.000',
  `advance_freeze` decimal(20,3) NOT NULL default '0.000',
  `point_freeze` mediumint(8) unsigned NOT NULL default '0',
  `point_history` mediumint(8) unsigned NOT NULL default '0',
  `point` mediumint(8) unsigned NOT NULL default '0',
  `score_rate` decimal(5,3) default NULL,
  `reg_ip` varchar(16) default NULL,
  `regtime` int(10) unsigned default NULL,
  `state` tinyint(1) NOT NULL default '0',
  `pay_time` mediumint(8) unsigned default NULL,
  `biz_money` decimal(20,3) NOT NULL default '0.000',
  `pw_answer` varchar(250) default NULL,
  `pw_question` varchar(250) default NULL,
  `fav_tags` longtext,
  `custom` longtext,
  `cur` varchar(20) default NULL,
  `lang` varchar(20) default NULL,
  `unreadmsg` smallint(5) unsigned NOT NULL default '0',
  `disabled` enum('true','false') default 'false',
  `remark` text,
  `remark_type` varchar(2) NOT NULL default 'b1',
  `login_count` int(11) NOT NULL default '0',
  `experience` int(10) default '0',
  `foreign_id` varchar(255) default NULL,
  `member_refer` varchar(50) default 'local',
  `emailcheck` enum('0','1') NOT NULL default '0',
  `emailkey` varchar(255) default NULL,
  `isaddpoint` tinyint(1) default '0',
  `phonecheck` enum('0','1') NOT NULL default '0',
  `phonechecknum` varchar(255) default '0',
  PRIMARY KEY  (`member_id`),
  KEY `ind_email` (`email`),
  KEY `uni_user` (`uname`),
  KEY `ind_regtime` (`regtime`),
  KEY `ind_disabled` (`disabled`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8 AUTO_INCREMENT=1689 ;[/code]