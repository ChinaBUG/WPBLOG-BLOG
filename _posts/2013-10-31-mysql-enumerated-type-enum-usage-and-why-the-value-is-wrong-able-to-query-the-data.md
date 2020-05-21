---
ID: 2691
post_title: >
  MySQL-枚举类型ENUM的用法及为什么值是错误的却能查询到数据
author: ChinaBUG
post_excerpt: |
  是数据类型的问题！
  然后查找了一下枚举类型ENUM的用法之后，没有对这个问题做直接解答，但是从字里行间可以感觉，真的是数据类型的问题啦。
  因为枚举类型ENUM是字符串类型的，所以访问什么的都是需要引号的噢。
  那么为什么会出现上面的“奇迹”呢？
  主要原因在于枚举类型如果值是在指定的范围内的，则自动获取填写，而且这个值是忽略大小写的！那么如果值不在指定的范围内怎么办呢？MySQL会自动默认选择第一项的值！
  我们回到之前SQL上来看，如果将phonecheck=1修改为phonecheck=2的话会怎样？
  结果是：我们获得了phonecheck的值为1的记录！那么phonecheck=0为什么没有数据呢？原因可能在于枚举类型的值的序号从1开始，所以默认找不到这个需要的值了。
  从这个案例我们得出如下结论：
  在写SQL语句时，请严格按照数据类型来写SQL语句，如一般的会员ID是数字型的，是不用加引号的，但是在MySQL里面这个可加可不加，获取的数据是一致的。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/mysql-enumerated-type-enum-usage-and-why-the-value-is-wrong-able-to-query-the-data.html
published: true
post_date: 2013-10-31 16:34:34
---
今天，也就刚才，小王进来找我，发现了一个好神奇的事情！

场景描述一下：

在ShopEX的数据库sdb_members之中增加phonecheck字段，结构如下：
<table id="tablestructure">
<tbody>
<tr>
<th nowrap="nowrap"><label for="checkbox_row_61">phonecheck</label></th>
<td><bdo dir="ltr">enum('0','1')</bdo></td>
<td><dfn title="Unicode (多语言), 不区分大小写">utf8_general_ci</dfn></td>
<td nowrap="nowrap"></td>
<td>否</td>
<td nowrap="nowrap">0</td>
</tr>
</tbody>
</table>
这个字段的作用就是当值是1时证明已经手机验证，0时证明还没手机验证。

在数据浏览的时候，我的记录上的phonecheck的值是0。然后，使用下面的语句，之后你就会见证一个奇迹！我的记录竟然出现了！！！

select member_id,phonechecknum,firstphonecheck from sdb_members where mobile ='15959******' and phonechecknum = '15959******' and phonecheck=1

根据正常情况分析，因为这SQL查询的是我的会员信息，而我的会员信息的phonecheck的值刚才浏览时已经确定了是0了，那么这句SQL查询之后肯定是没有记录的了。

可是，奇迹！居然有记录，而且还是我的会员信息这个记录！

然后，小王突发奇想，代码修改为

select member_id,phonechecknum,firstphonecheck from sdb_members where mobile ='15959******' and phonechecknum = '15959******' and phonecheck=true

然后你会发现还是可以查询出数据来！

反之一下试试看，尝试之后发现当phonecheck=0与phonecheck=false之后查询记录都为空！

好神奇对不？！

然后，我一想为什么不加上引号呢？

select member_id,phonechecknum,firstphonecheck from sdb_members where mobile ='15959******' and phonechecknum = '15959******' and phonecheck=‘0’

哈~~有记录了，再改为1，果然没有数据了~

是数据类型的问题！

然后查找了一下枚举类型ENUM的用法之后，没有对这个问题做直接解答，但是从字里行间可以感觉，真的是数据类型的问题啦。

因为枚举类型ENUM是字符串类型的，所以访问什么的都是需要引号的噢。

那么为什么会出现上面的“奇迹”呢？

主要原因在于枚举类型如果值是在指定的范围内的，则自动获取填写，而且这个值是忽略大小写的！那么如果值不在指定的范围内怎么办呢？MySQL会自动默认选择第一项的值！

我们回到之前SQL上来看，如果将phonecheck=1修改为phonecheck=2的话会怎样？

结果是：我们获得了phonecheck的值为1的记录！那么phonecheck=0为什么没有数据呢？原因可能在于枚举类型的值的序号从1开始，所以默认找不到这个需要的值了。

从这个案例我们得出如下结论：

在写SQL语句时，请严格按照数据类型来写SQL语句，如一般的会员ID是数字型的，是不用加引号的，但是在MySQL里面这个可加可不加，获取的数据是一致的。