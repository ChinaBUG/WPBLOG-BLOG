---
ID: 3019
post_title: >
  ShopNC-二次开发研究日记之数据库操作
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopnc-2dev-database-operation.html
published: true
post_date: 2013-12-10 15:27:03
---
怎样在ShopNC里对数据库进行操作呢？其实很简单，具体如下噢：

第一步：新建模型

模型又分两种，一种是直接调用系统本身的相关方法，另一种就是定义了自己的模型的话，则加载，具体语法如下：

[php]
//framework\core\model.php
$model = Model();
//model\member.model.php
$model = Model('member');
[/php]

第二步：调用相关的方法

相关的方法包含怎么定义筛选条件，排序，分组等等，一般用的不多噢，常用的记住即可。

2.1 table方法

这个方法唯一的作用就是指定要操作的数据表，参数值为表的名字，不加前缀的表名！如<code>“</code><code>shopnc_member”这个是会员信息表，其中我设置的前缀是“<code>shopnc_”那么只需要输入：</code></code>

[php]$model-&gt;table('member')...[/php]

即可。另外，要说明的是，如果使用自定义模型的话，这个方法可以不用指定。
<blockquote><em>小提示：</em>
<em>怎么查看自己的商城的数据库的信息还有数据库前缀呢？请打开根目录下的config.ini.php文档即可查看到。</em></blockquote>
2.2 field方法

这个方法的作用是指定要获取的字段列表，如果不指定返回全部的字段，一般情况下，建议用到那些字段读入那些字段比较好。格式语法如下：

[php]$model-&gt;table('member')-&gt;field('member_id,member_name,member_truename,store_id,member_avatar')...[/php]

2.3 getFields方法

这个方法就是获取全部的表的关键keyID的值，调用之后将返回全部的表的关键key的名字。格式语法如下：

[php]$model-&gt;table('member')-&gt;getFields()[/php]

这个方法调用的时候要注意，那个table一定要指定，当然你随便写啦，然后才能调用，要不返回空白的内容。

2.4 where方法

这个方法就是指定条件，筛选数据了，格式语法如下：

[php]$model-&gt;table('member')-&gt;where($where)...[/php]

$where这个变量的取值有点讲究：
<ol>
	<li>字符串：直接加入where条件内，即使用的时候
$where=‘<code>member_id=6</code>’;然后就可以查找到member_id等于6的数据。</li>
	<li>数组：如果是数组的话比较复杂，他需要判断操作符什么的
$where['_op'] = “AND|OR”;//默认为AND
$where = array('member_id'=&gt;1);或者$where['member_id'] = 1;
其他形式，较为复杂，请看附录的部分，对这类的设置做一个较为全面的介绍</li>
</ol>
2.5 order方法

这个方法就是指定排序的方法，只要参数填写一下就会排序了噢，一般都是直接写的：

[php]$model-&gt;table('member')-&gt;order('member_id asc')...[/php]

2.6 limit方法

这个方法是指定获取数据多少个噢，直接填写记录数量：

[php]$model-&gt;table('member')-&gt;limit(10)...[/php]

2.7 page方法

数据分页用的，分几页，规则也是一样的：

[php]$model-&gt;table('member')-&gt;page(10)...[/php]

第三步：执行操作数据库

3.1获取数据是我们开发的主要任务，那么设置了第二部中的参数之后，我们就要获取符合条件的数据，获取的方式有两种：

3.1.1 find方法：获取一条记录，系统自动增加limit参数。

3.1.2 select方法：获取符合的全部记录

3.1.3 getby_*方法：根据相应的字段名获取数据，*可以是任意符合要求的字段名，如getby_id(1)则返回ID为1的数据，如getby_store_id($_SESSION['store_id'])则获取store_id为$_SESSION['store_id']的记录，简单说就是获取当前店铺信息。

3.1.4 getfby_*方法：功能同上，唯一区别是该方法可以指定返回的字段，而上面的只能返回全部字段。如getfby_store_id($_SESSION['store_id'],'express')则是返回express字段。

3.2插入数据

[php]$model-&gt;table('member')-&gt;insert( $data, $options = array( ), $replace = FALSE )
$model-&gt;table('member')-&gt;insertAll( $datas, $options = array( ), $replace = FALSE )[/php]

3.3更新数据

[php]$model-&gt;update( $data, $options )[/php]

$options['where'] 指定更新的条件。

3.4删除数据

[php]$model-&gt;delete( $options = array( ) )[/php]

$options['where'] 指定删除的条件。

3.5 清除表的全部数据

[php]$model-&gt;table('member')-&gt;clear( array('table'=&gt;'web'));[/php]

注意：table方法的表可以随便设置，删除的是clear方法的参数table指定的表。上面例子实际上清除了web表的数据。

~..~ ~..~ ~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~~..~ ~..~

<strong>附录：</strong>

<em>A：WHERE定义设置</em>

1、$where['member_id|store_id'] = array(6,2);

结果：( ((member_id = '6') AND (member_id = '2') ) OR ((store_id = '6') AND (store_id = '2') ) )

2、$where['member_id|store_id'] = array('_multi'=&gt;true,6,2);

结果：( (member_id = '6') OR (store_id = '2') )

3、$where['member_id&amp;store_id'] = array(6,2);

结果：( ((member_id = '6') AND (member_id = '2') ) AND ((store_id = '6') AND (store_id = '2') ) )

4、$where['member_id&amp;store_id'] = array('_multi'=&gt;true,6,2);

结果：( (member_id = '6') AND (store_id = '2') )

5、$where['member_id'] = array('GT',6);

结果：( member_id &gt; '6' )

注意：

操作符范围是EQ|NEQ|GT|EGT|LT|ELT|NOTLIKE|LIKE，exp，IN，BETWEEN。

其中BETWEEN的话，则值需要是字符串，里面包含两个日期用逗号隔开，如strtotime('20130101').','.strtotime('20130202')这样子。

而exp的格式是值必须包含操作符等信息，如在更新时指定'member_login_num'=&gt;array('exp','member_login_num+1')。