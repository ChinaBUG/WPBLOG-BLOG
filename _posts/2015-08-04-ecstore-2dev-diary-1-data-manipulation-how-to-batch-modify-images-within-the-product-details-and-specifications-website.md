---
ID: 4114
post_title: >
  ECStore二次开发日记之1.数据操作-如何批量修改商品详情及规格内的图片网址
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-1-data-manipulation-how-to-batch-modify-images-within-the-product-details-and-specifications-website.html
published: true
post_date: 2015-08-04 17:52:40
---
因为公司的ECStore系统，以前用过一个域名，然后中途又用一个域名（IP），然后最后才是目前的域名，这个就出现了一个大的问题！

系统的商品详情，商品的多规格图片，品牌图片等内容均出现这些曾经的域名与IP怎么办？

也许，你觉得使用SQL直接修改来的比较快，是的，但是！请注意，直接替换会害死你的，因为多规格使用PHP的serialize序列化后的值怎么替换呢？

好吧，不管是使用SQL还是怎样，都不会影响我们日记的目标，怎么在做ECStore的二次开发的时候操作数据库。

~

我们在做ShopEX4.85系列二次开发的时候要操作数据库经常会用到如下代码：

$this-&gt;db = $this-&gt;system-&gt;database();

这个是调用数据库对象的方法，那么在ECStore中怎么办呢？直接这样子操作可以嘛？

可以的话就不说了噢，因为ECStore使用PHP5的一些开发技术，所以调用方式改变为如下形式了：

$this-&gt;db = kernel::database();

没错，就是这么简单，具体的代码在app|base|kernel.php中有定义，有需要的朋友可以自己去查看~

除了上面的调用方式之外，你还可以使用如下方式调用：

kernel::database()-&gt;select($sql);

那么其他一些常用的方法也改变了没？没有，具体的方法定义查看app|base|lib|db|connections.php文件，下面是方法及原型：
<table>
<tbody>
<tr>
<td>方法</td>
<td>用途说明</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;prefix</td>
<td> 返回系统定义的数据库表前缀</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;lastinsertid()</td>
<td> 返回最后插入的ID值</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;quote($string)</td>
<td> 返回加完引号的字符串</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;count($sql)</td>
<td> 返回查询的数据记录数</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;getRows($rs,$row=10)</td>
<td> 获取$RS对象的N条数据</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;selectlimit($sql,$limit=10,$offset=0)</td>
<td> 获取限定的数据</td>
</tr>
<tr>
<td> $this-&gt;db-&gt;selectrow($sql)</td>
<td> 获取唯一一条查询的数据</td>
</tr>
<tr>
<td>$this-&gt;db-&gt;select($sql, $skipModifiedMark=false)</td>
<td> 获取查询的数据</td>
</tr>
<tr>
<td>$this-&gt;db-&gt;exec($sql , $skipModifiedMark=false, $db_lnk=null)</td>
<td> 执行查询</td>
</tr>
</tbody>
</table>
有了上面的方法，现在做起ECStore的二次开发是不是格外的顺畅呢？！

下面就是利用这些语句来一个实例吧，顺便解决一下，域名错误的问题。

~

备份数据

我们做任何操作数据的时候最好能养成一个习惯，就是要备份，特别是操作全局的时候，现在第一步就是来备份数据库吧，内容比较多，直接复制吧，当然你要看完也是可以的。

备份的数据表名是在原先数据表名后加上执行当时的时间。
<pre>$curTime = date('YmdHis');
$sql_bak = " CREATE  TABLE  `sdb_b2c_goods_{$curTime}` (
 `goods_id` bigint( 20  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `bn` varchar( 200  )  DEFAULT NULL ,
 `name` varchar( 200  )  NOT  NULL DEFAULT  '',
 `price` decimal( 20, 3  )  NOT  NULL DEFAULT  '0.000',
 `type_id` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `cat_id` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '0',
 `brand_id` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `marketable` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'true',
 `store` mediumint( 8  ) unsigned DEFAULT  '0',
 `notify_num` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '0',
 `uptime` int( 10  ) unsigned  DEFAULT NULL ,
 `downtime` int( 10  ) unsigned  DEFAULT NULL ,
 `last_modify` int( 10  ) unsigned  DEFAULT NULL ,
 `p_order` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '30',
 `d_order` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '30',
 `score` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `cost` decimal( 20, 3  )  NOT  NULL DEFAULT  '0.000',
 `mktprice` decimal( 20, 3  )  DEFAULT NULL ,
 `weight` decimal( 20, 3  )  DEFAULT NULL ,
 `unit` varchar( 20  )  DEFAULT NULL ,
 `brief` varchar( 255  )  DEFAULT NULL ,
 `goods_type` enum(  'normal',  'bind',  'gift'  )  NOT  NULL DEFAULT  'normal',
 `image_default_id` varchar( 32  )  DEFAULT NULL ,
 `udfimg` enum(  'true',  'false'  ) DEFAULT  'false',
 `thumbnail_pic` varchar( 32  )  DEFAULT NULL ,
 `small_pic` varchar( 255  )  DEFAULT NULL ,
 `big_pic` varchar( 255  )  DEFAULT NULL ,
 `intro` longtext,
 `store_place` varchar( 255  )  DEFAULT NULL ,
 `min_buy` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `package_scale` decimal( 20, 2  )  DEFAULT NULL ,
 `package_unit` varchar( 20  )  DEFAULT NULL ,
 `package_use` enum(  '0',  '1'  )  DEFAULT NULL ,
 `score_setting` enum(  'percent',  'number'  ) DEFAULT  'number',
 `store_prompt` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `nostore_sell` enum(  '0',  '1'  ) DEFAULT  '0',
 `goods_setting` longtext,
 `spec_desc` longtext,
 `params` longtext,
 `disabled` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'false',
 `rank_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `comments_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `view_w_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `view_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `count_stat` longtext,
 `buy_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `buy_w_count` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 `p_1` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_2` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_3` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_4` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_5` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_6` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_7` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_8` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_9` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_10` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_11` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_12` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_13` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_14` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_15` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_16` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_17` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_18` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_19` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_20` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `p_21` varchar( 255  )  DEFAULT NULL ,
 `p_22` varchar( 255  )  DEFAULT NULL ,
 `p_23` varchar( 255  )  DEFAULT NULL ,
 `p_24` varchar( 255  )  DEFAULT NULL ,
 `p_25` varchar( 255  )  DEFAULT NULL ,
 `p_26` varchar( 255  )  DEFAULT NULL ,
 `p_27` varchar( 255  )  DEFAULT NULL ,
 `p_28` varchar( 255  )  DEFAULT NULL ,
 `p_29` varchar( 255  )  DEFAULT NULL ,
 `p_30` varchar( 255  )  DEFAULT NULL ,
 `p_31` varchar( 255  )  DEFAULT NULL ,
 `p_32` varchar( 255  )  DEFAULT NULL ,
 `p_33` varchar( 255  )  DEFAULT NULL ,
 `p_34` varchar( 255  )  DEFAULT NULL ,
 `p_35` varchar( 255  )  DEFAULT NULL ,
 `p_36` varchar( 255  )  DEFAULT NULL ,
 `p_37` varchar( 255  )  DEFAULT NULL ,
 `p_38` varchar( 255  )  DEFAULT NULL ,
 `p_39` varchar( 255  )  DEFAULT NULL ,
 `p_40` varchar( 255  )  DEFAULT NULL ,
 `p_41` varchar( 255  )  DEFAULT NULL ,
 `p_42` varchar( 255  )  DEFAULT NULL ,
 `p_43` varchar( 255  )  DEFAULT NULL ,
 `p_44` varchar( 255  )  DEFAULT NULL ,
 `p_45` varchar( 255  )  DEFAULT NULL ,
 `p_46` varchar( 255  )  DEFAULT NULL ,
 `p_47` varchar( 255  )  DEFAULT NULL ,
 `p_48` varchar( 255  )  DEFAULT NULL ,
 `p_49` varchar( 255  )  DEFAULT NULL ,
 `p_50` varchar( 255  )  DEFAULT NULL ,
 PRIMARY  KEY (  `goods_id`  ) ,
 UNIQUE  KEY  `uni_bn` (  `bn`  ) ,
 KEY  `ind_p_1` (  `p_1`  ) ,
 KEY  `ind_p_2` (  `p_2`  ) ,
 KEY  `ind_p_3` (  `p_3`  ) ,
 KEY  `ind_p_4` (  `p_4`  ) ,
 KEY  `ind_p_5` (  `p_5`  ) ,
 KEY  `ind_p_6` (  `p_6`  ) ,
 KEY  `ind_p_7` (  `p_7`  ) ,
 KEY  `ind_p_8` (  `p_8`  ) ,
 KEY  `ind_p_9` (  `p_9`  ) ,
 KEY  `ind_p_10` (  `p_10`  ) ,
 KEY  `ind_p_11` (  `p_11`  ) ,
 KEY  `ind_p_12` (  `p_12`  ) ,
 KEY  `ind_p_13` (  `p_13`  ) ,
 KEY  `ind_p_14` (  `p_14`  ) ,
 KEY  `ind_p_15` (  `p_15`  ) ,
 KEY  `ind_p_16` (  `p_16`  ) ,
 KEY  `ind_p_17` (  `p_17`  ) ,
 KEY  `ind_p_18` (  `p_18`  ) ,
 KEY  `ind_p_19` (  `p_19`  ) ,
 KEY  `ind_p_20` (  `p_20`  ) ,
 KEY  `ind_p_23` (  `p_23`  ) ,
 KEY  `ind_p_22` (  `p_22`  ) ,
 KEY  `ind_p_21` (  `p_21`  ) ,
 KEY  `ind_p_24` (  `p_24`  ) ,
 KEY  `ind_p_25` (  `p_25`  ) ,
 KEY  `ind_p_26` (  `p_26`  ) ,
 KEY  `ind_p_27` (  `p_27`  ) ,
 KEY  `ind_p_28` (  `p_28`  ) ,
 KEY  `ind_p_29` (  `p_29`  ) ,
 KEY  `ind_p_30` (  `p_30`  ) ,
 KEY  `ind_p_31` (  `p_31`  ) ,
 KEY  `ind_p_32` (  `p_32`  ) ,
 KEY  `ind_p_33` (  `p_33`  ) ,
 KEY  `ind_p_34` (  `p_34`  ) ,
 KEY  `ind_p_35` (  `p_35`  ) ,
 KEY  `ind_p_36` (  `p_36`  ) ,
 KEY  `ind_p_37` (  `p_37`  ) ,
 KEY  `ind_p_38` (  `p_38`  ) ,
 KEY  `ind_p_39` (  `p_39`  ) ,
 KEY  `ind_p_40` (  `p_40`  ) ,
 KEY  `ind_p_41` (  `p_41`  ) ,
 KEY  `ind_p_42` (  `p_42`  ) ,
 KEY  `ind_p_43` (  `p_43`  ) ,
 KEY  `ind_p_44` (  `p_44`  ) ,
 KEY  `ind_p_45` (  `p_45`  ) ,
 KEY  `ind_p_46` (  `p_46`  ) ,
 KEY  `ind_p_47` (  `p_47`  ) ,
 KEY  `ind_p_48` (  `p_48`  ) ,
 KEY  `ind_p_49` (  `p_49`  ) ,
 KEY  `ind_p_50` (  `p_50`  ) ,
 KEY  `ind_frontend` (  `disabled` ,  `goods_type` ,  `marketable`  ) ,
 KEY  `idx_goods_type` (  `goods_type`  ) ,
 KEY  `idx_d_order` (  `d_order`  ) ,
 KEY  `idx_goods_type_d_order` (  `goods_type` ,  `d_order`  ) ,
 KEY  `idx_marketable` (  `marketable`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_b2c_goods_{$curTime}` SELECT * FROM `sdb_b2c_goods`;

CREATE  TABLE  `sdb_dbeav_meta_value_text_{$curTime}` (
 `mr_id` mediumint( 8  )  unsigned NOT  NULL ,
 `pk` mediumint( 8  )  unsigned NOT  NULL ,
 `value` text NOT  NULL ,
 PRIMARY  KEY (  `mr_id` ,  `pk`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_dbeav_meta_value_text_{$curTime}` SELECT * FROM `sdb_dbeav_meta_value_text`;

CREATE  TABLE  `sdb_b2c_brand_{$curTime}` (
 `brand_id` mediumint( 8  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `brand_name` varchar( 50  )  NOT  NULL ,
 `brand_url` varchar( 255  )  DEFAULT NULL ,
 `brand_desc` longtext,
 `brand_logo` varchar( 255  )  DEFAULT NULL ,
 `brand_keywords` longtext,
 `brand_setting` longtext,
 `disabled` enum(  'true',  'false'  ) DEFAULT  'false',
 `ordernum` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `last_modify` int( 10  ) unsigned  DEFAULT NULL ,
 PRIMARY  KEY (  `brand_id`  ) ,
 KEY  `ind_disabled` (  `disabled`  ) ,
 KEY  `ind_ordernum` (  `ordernum`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_b2c_brand_{$curTime}` SELECT * FROM `sdb_b2c_brand`;

CREATE  TABLE  `sdb_b2c_member_systmpl_{$curTime}` (
 `tmpl_name` varchar( 100  )  NOT  NULL ,
 `content` longtext,
 `edittime` int( 10  )  NOT  NULL ,
 `active` enum(  'true',  'false'  ) DEFAULT  'true',
 PRIMARY  KEY (  `tmpl_name`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_b2c_member_systmpl_{$curTime}` SELECT * FROM `sdb_b2c_member_systmpl`;

CREATE  TABLE  `sdb_content_article_bodys_{$curTime}` (
 `id` mediumint( 8  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `article_id` mediumint( 8  )  unsigned NOT  NULL ,
 `tmpl_path` varchar( 50  )  DEFAULT NULL ,
 `content` longtext,
 `seo_title` varchar( 100  )  DEFAULT NULL ,
 `seo_description` mediumtext,
 `seo_keywords` varchar( 200  )  DEFAULT NULL ,
 `goods_info` longtext,
 `hot_link` longtext,
 `length` int( 10  ) unsigned  DEFAULT NULL ,
 `image_id` varchar( 32  )  DEFAULT NULL ,
 PRIMARY  KEY (  `id`  ) ,
 UNIQUE  KEY  `ind_article_id` (  `article_id`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_content_article_bodys_{$curTime}` SELECT * FROM `sdb_content_article_bodys`;

CREATE  TABLE  `sdb_content_article_nodes_{$curTime}` (
 `node_id` mediumint( 8  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `parent_id` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '0',
 `node_depth` tinyint( 1  )  NOT  NULL DEFAULT  '0',
 `node_name` varchar( 50  )  NOT  NULL DEFAULT  '',
 `node_pagename` varchar( 50  )  DEFAULT NULL ,
 `node_path` varchar( 200  )  DEFAULT NULL ,
 `seo_title` varchar( 100  )  DEFAULT NULL ,
 `seo_description` mediumtext,
 `seo_keywords` varchar( 200  )  DEFAULT NULL ,
 `has_children` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'false',
 `ifpub` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'false',
 `hasimage` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'false',
 `ordernum` mediumint( 8  )  unsigned NOT  NULL DEFAULT  '0',
 `homepage` enum(  'true',  'false'  ) DEFAULT  'false',
 `uptime` int( 10  ) unsigned  DEFAULT NULL ,
 `tmpl_path` varchar( 50  )  DEFAULT NULL ,
 `list_tmpl_path` varchar( 50  )  DEFAULT NULL ,
 `content` longtext,
 `disabled` enum(  'true',  'false'  )  NOT  NULL DEFAULT  'false',
 PRIMARY  KEY (  `node_id`  ) ,
 KEY  `ind_disabled` (  `disabled`  ) ,
 KEY  `ind_ordernum` (  `ordernum`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_content_article_nodes_{$curTime}` SELECT * FROM `sdb_content_article_nodes`;

CREATE  TABLE  `sdb_desktop_recycle_{$curTime}` (
 `item_id` mediumint( 8  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `item_title` varchar( 200  )  DEFAULT NULL ,
 `item_type` varchar( 80  )  NOT  NULL ,
 `app_key` varchar( 80  )  NOT  NULL ,
 `drop_time` int( 10  )  unsigned NOT  NULL ,
 `item_sdf` longtext NOT  NULL ,
 `permission` varchar( 80  )  DEFAULT NULL ,
 PRIMARY  KEY (  `item_id`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_desktop_recycle_{$curTime}` SELECT * FROM `sdb_desktop_recycle`;

CREATE  TABLE  `sdb_ectools_payments_{$curTime}` (
 `payment_id` varchar( 20  )  NOT  NULL DEFAULT  '',
 `money` decimal( 20, 3  )  NOT  NULL DEFAULT  '0.000',
 `cur_money` decimal( 20, 3  )  NOT  NULL DEFAULT  '0.000',
 `member_id` varchar( 100  )  DEFAULT NULL ,
 `status` enum(  'succ',  'failed',  'cancel',  'error',  'invalid',  'progress',  'timeout',  'ready'  )  NOT  NULL DEFAULT  'ready',
 `pay_name` varchar( 100  )  DEFAULT NULL ,
 `pay_type` enum(  'online',  'offline',  'deposit'  )  NOT  NULL DEFAULT  'online',
 `t_payed` int( 10  ) unsigned  DEFAULT NULL ,
 `op_id` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `payment_bn` varchar( 32  ) DEFAULT  '',
 `account` varchar( 50  )  DEFAULT NULL ,
 `bank` varchar( 50  )  DEFAULT NULL ,
 `pay_account` varchar( 50  )  DEFAULT NULL ,
 `currency` varchar( 10  )  DEFAULT NULL ,
 `paycost` decimal( 20, 3  )  DEFAULT NULL ,
 `pay_app_id` varchar( 100  )  NOT  NULL DEFAULT  '0',
 `pay_ver` varchar( 50  )  DEFAULT NULL ,
 `ip` varchar( 20  )  DEFAULT NULL ,
 `t_begin` int( 10  ) unsigned  DEFAULT NULL ,
 `t_confirm` int( 10  ) unsigned  DEFAULT NULL ,
 `memo` longtext,
 `return_url` varchar( 100  )  DEFAULT NULL ,
 `disabled` enum(  'true',  'false'  ) DEFAULT  'false',
 `trade_no` varchar( 30  )  DEFAULT NULL ,
 `thirdparty_account` varchar( 50  ) DEFAULT  '',
 PRIMARY  KEY (  `payment_id`  ) ,
 KEY  `ind_disabled` (  `disabled`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_ectools_payments_{$curTime}` SELECT * FROM `sdb_ectools_payments`;

CREATE  TABLE  `sdb_image_image_{$curTime}` (
 `image_id` char( 32  )  NOT  NULL ,
 `storage` varchar( 50  )  NOT  NULL DEFAULT  'filesystem',
 `image_name` varchar( 50  )  DEFAULT NULL ,
 `ident` varchar( 200  )  NOT  NULL ,
 `url` varchar( 200  )  NOT  NULL ,
 `l_ident` varchar( 200  )  DEFAULT NULL ,
 `l_url` varchar( 200  )  DEFAULT NULL ,
 `m_ident` varchar( 200  )  DEFAULT NULL ,
 `m_url` varchar( 200  )  DEFAULT NULL ,
 `s_ident` varchar( 200  )  DEFAULT NULL ,
 `s_url` varchar( 200  )  DEFAULT NULL ,
 `width` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `height` mediumint( 8  ) unsigned  DEFAULT NULL ,
 `watermark` enum(  'true',  'false'  ) DEFAULT  'false',
 `last_modified` int( 10  )  unsigned NOT  NULL DEFAULT  '0',
 PRIMARY  KEY (  `image_id`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_image_image_{$curTime}` SELECT * FROM `sdb_image_image`;

CREATE  TABLE  `sdb_pam_log_desktop_{$curTime}` (
 `event_id` mediumint( 8  )  unsigned NOT  NULL  AUTO_INCREMENT ,
 `event_time` varchar( 50  )  DEFAULT NULL ,
 `event_data` varchar( 500  )  DEFAULT NULL ,
 `event_type` text,
 PRIMARY  KEY (  `event_id`  )  ) ENGINE  = InnoDB  DEFAULT CHARSET  = utf8;
SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO';
INSERT INTO `sdb_pam_log_desktop_{$curTime}` SELECT * FROM `sdb_pam_log_desktop`;";

$sql_bak_array = explode(';',$sql_bak);

echo '&gt;&gt;&gt;&gt; 总的SQl条数：'.count( $sql_bak_array )."&lt;br/&gt;n";

foreach( $sql_bak_array as $sqlR ){
    if( $sqlR )
    {
        $rs = $this-&gt;db-&gt;exec( $sqlR );
        echo '更新语句为：'.$sqlR."&lt;br/&gt;n";
    }
}
</pre>
注：或者有同学想问，这么多代码，难道是我一个个的敲出来的？明显是不可能的，请善用工具好嘛，我是使用phpmyadmin来生成这个备份的脚本，修改一下增加一个参数，其他的都是直接盗用了。请注意$sql_bak变量的最后，分号一定要紧跟！！！

备份完毕。

~

修改数据库

现在按照我们的需要一个个的查询过去，有数据则使用修改语句直接修改，如果是序列化的数据那么就直接按照结构组装一下更新就可以了。具体请看下面：

注：因为内容较多，就展示部分的修改内容吧，其他的就自己看看吧。
<pre>        //1.PC详情
        echo "&lt;hr/&gt;n";
        echo '1.PC详情 '."&lt;br/&gt;n";
        $sql = 'SELECT count(`goods_id`) as _count FROM `sdb_b2c_goods` WHERE `intro` LIKE '%http://218.244.***.150%'';
        $rs = $mysql-&gt;selectrow( $sql );
        echo '查询语句为：'.$sql."&lt;br/&gt;n";
        echo '查询到的数据一共有 '.$rs['_count']."&lt;br/&gt;n";
        if( $rs['_count']&gt;0 )
        {
            $sql = 'UPDATE `sdb_b2c_goods` SET `intro` = replace( `intro`,"http://218.244.***.150", "http://www.shan***.cn" )';
            $rs = $mysql-&gt;exec( $sql );
            echo '更新语句为：'.$sql."&lt;br/&gt;n";
            echo '更新的数据一共有 '.$rs."&lt;br/&gt;n";
        }
        echo "&lt;hr/&gt;n";
        
        //2.WAP商品详情
        echo "&lt;hr/&gt;n";
        echo '2.WAP商品详情 '."&lt;br/&gt;n";
        $sql = 'SELECT count(`mr_id`) as _count FROM `sdb_dbeav_meta_value_text` WHERE `value` LIKE '%http://218.244.***.150%'';
        $rs = $mysql-&gt;selectrow( $sql );
        echo '查询语句为：'.$sql."&lt;br/&gt;n";
        echo '查询到的数据一共有 '.$rs['_count']."&lt;br/&gt;n";
        if( $rs['_count']&gt;0 )
        {
            $sql = 'UPDATE `sdb_dbeav_meta_value_text` SET `value` = replace( value,"http://218.244.***.150", "http://www.shan***.cn" )';
            $rs = $mysql-&gt;exec( $sql );
            echo '更新语句为：'.$sql."&lt;br/&gt;n";
            echo '更新的数据一共有 '.$rs."&lt;br/&gt;n";
        }
        echo "&lt;hr/&gt;n";        
        
        //3.商品详情的多规格图片
        echo "&lt;hr/&gt;n";
        echo '3.商品详情的多规格图片 '."&lt;br/&gt;n";
        $sql = 'SELECT count(`goods_id`) as _count FROM `sdb_b2c_goods` WHERE `spec_desc` LIKE '%http://218.244.***.150%'';
        $rs = $mysql-&gt;selectrow( $sql );
        echo '查询语句为：'.$sql."&lt;br/&gt;n";
        echo '查询到的数据一共有 '.$rs['_count']."&lt;br/&gt;n";
        if( $rs['_count']&gt;0 )
        {
        //
        $sql = "SELECT * FROM `sdb_b2c_goods` WHERE `spec_desc` LIKE '%http://218.244.***.150/%'";
        $rslist = $mysql-&gt;select($sql);
        if( $rslist )
        {
            $Str_find = array('http://218.244.***.150/');
            $Str_repl = array('http://www.shan***.cn/');
            foreach( $rslist as $key_rs1 =&gt; $val_rsl )
            {
                $val_rsl['spec_desc'] = ( unserialize( $val_rsl['spec_desc'] )===false ? $this-&gt;mb_unserialize( $val_rsl['spec_desc'] ): unserialize( $val_rsl['spec_desc'] ) );
                foreach( $val_rsl['spec_desc'] as $key_desc1 =&gt; $val_desc1){
                    foreach($val_desc1 as $key_desc2 =&gt; $val_desc2){
                        $val_rsl['spec_desc'][ $key_desc1 ][ $key_desc2 ]['spec_image_url'] = str_ireplace($Str_find,$Str_repl,$val_desc2['spec_image_url']);
                        foreach($val_desc2['spec_goods_images'] as $key_spec1 =&gt; $val_spec1){
                            $val_desc2['spec_goods_images'][ $key_spec1 ]['image_url'] = str_ireplace($Str_find,$Str_repl,$val_spec1['image_url']);
                        }
                        $val_rsl['spec_desc'][ $key_desc1 ][ $key_desc2 ]['spec_goods_images'] = $val_desc2['spec_goods_images'];
                    }
                }
                $sql_tmp = 'update `sdb_b2c_goods` set `spec_desc` = ''.serialize($val_rsl['spec_desc']).'' WHERE goods_id='.$val_rsl['goods_id'];
                echo '更新语句为：'.$sql_tmp."&lt;br/&gt;n";
                
                $rs = $mysql-&gt;exec($sql_tmp);
                
                // print_r( $rs );
                //echo ($key_rs1+1) . ' - GID : ' . $val_rsl['goods_id'] . ' - ' . $rs['rs'] . '&lt;br/&gt;';
            }
        }
        //    
            //echo '更新语句为：'.$sql."&lt;br/&gt;n";
            echo '更新的数据一共有 '.count($rslist)."&lt;br/&gt;n";
        }
        echo "&lt;hr/&gt;n";
</pre>
执行之后你就会发现，噢，天空干净了~~

今天二次开发日记你学会操作数据库了没？！赶紧去实践吧^_^