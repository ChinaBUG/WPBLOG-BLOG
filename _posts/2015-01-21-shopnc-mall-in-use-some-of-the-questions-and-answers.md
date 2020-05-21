---
ID: 3904
post_title: >
  ShopNC-商城使用过程中出现的一些问题及解答
author: ChinaBUG
post_excerpt: |
  1、某个分类下面全部商品都被下架，查看错误提示是“违规下架”
  2、商品分类设置了SEO参数，为什么前台还是显示默认的信息description和keywords，看不到自己设置的内容呢？
layout: post
permalink: >
  http://blog.ipodmp.com/2015/01/shopnc-mall-in-use-some-of-the-questions-and-answers.html
published: true
post_date: 2015-01-21 13:30:18
---
一些专门性的Db Error错误造成的原因及解决方案，请参考本站文章《<a title="ShopNC-错误信息列表及解决方案" href="http://blog.ipodmp.com/archives/shopnc-list-of-error-messages-and-solutions/">ShopNC-错误信息列表及解决方案</a>》

本帖专门收集虫神在使用ShopNC过程中，遇见的错误及处理方案，如果你有什么错误的话，可以留言，我会尝试重现并解决。

1、某个分类下面全部商品都被下架，查看错误提示是“违规下架”

答：

这个情况，应该是修改了分类造成的，如果你没有后台修改商品分类的话，应该就不是这个问题了。

后台的分类管理，在编辑界面上有一个提示：

在编辑"类型"和勾选"关联到子分类"时，涉及分类下的商品将会被进行"违规下架"处理，商品在重新编辑后才能正常使用，请慎重操作。

这个说明，如果你编辑的时候，没有把“<input id="t_associated" name="t_associated" type="checkbox" value="1" /><label for="t_associated">关联到子分类</label>”前面的可选框取消的话，那么编辑之后该分类下的全部商品都会被下架。

2、商品分类设置了SEO参数，为什么前台还是显示默认的信息description和keywords，看不到自己设置的内容呢？

答：

请认真检查设置的分类的keywords选项是否有设置值，如果该分类只是设置了title或者description就是没有设置keywords的话，那么前台肯定不会显示出来的！

查看源码会发现，生成缓存的记录的条件是keywords不为空。

3、后台订单管理按时间导出发生错误：Db Error: Unknown column 'addtime' in 'where clause'

答：

具体原因在《<a title="ShopNC-错误信息列表及解决方案" href="http://blog.ipodmp.com/archives/shopnc-list-of-error-messages-and-solutions/">ShopNC-错误信息列表及解决方案</a>》一文中已经解释，这边直接修改。

找到\admin\control\trade.php查找addtime然后会找到下面代码处：

[php]
if ($time1 &amp;&amp; $time2){
$condition['addtime'] = array('between',array($time1,$time2));
}elseif($time1){
$condition['addtime'] = array('egt',$time1);
}elseif($time2){
$condition['addtime'] = array('elt',$time2);
}[/php]

改为下面

[php]        //2015.02.12 ChinaBUG 导出订单信息,提示     Db Error: Unknown column 'addtime' in 'where clause'
        if ($time1 &amp;&amp; $time2){
            $condition['add_time'] = array('between',array($time1,$time2));
        }elseif($time1){
            $condition['add_time'] = array('egt',$time1);
        }elseif($time2){
            $condition['add_time'] = array('elt',$time2);
        }[/php]

4、......</pre>