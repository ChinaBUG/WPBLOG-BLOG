---
ID: 2901
post_title: >
  ShopNC二次开发研究日记2:怎么修改特别推荐的个数(解除特别推荐6个的限制)
author: ChinaBUG
post_excerpt: |
  虫曰：
  《ShopNC二次开发研究日记》系列由ChinaBUG企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。
  -
  主页的“特别推荐”区域只有4个商品，不管后台怎么增加还是4个，想要增加为8个怎么办？只能自己修改呗~
  根据控制器的流程我们找到了control\index.php文件，然后发现调用这个地方的是_product()方法，然后查阅代码之后会发现，他只允许输出4个商品哈~
  $recommend_limit    = 4;//显示个数
  哈~写的很清楚嘛~改了
  然后，你会发现，怎么没反应？
  好吧，其实是因为它使用了缓存，所以我们需要删除缓存文件，缓存文件位于
  cache\index\product.php
  将这个文件删除，然后你刷新就会发现----还是没反应！
  为什么呢？
  因为我们后台测试的数据本身就只有4个商品呀，笨死了对吧~哈哈，增加一下吧
  提示：怎么添加推荐商品
  如果你进入“网站》推荐类型》特别推荐”你会发现编辑里面是没有增加商品的入口的，那么怎么增加商品噢？其实，增加的入口在“商品》商品管理”里面选择需要推荐的商品，然后点击最下方的“推荐”，然后选择推荐类型即可。
  添加完毕你会发现，果然出现8个商品了噢，不过样式需要调整一下，要不难看。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopnc-2dev-study-of-diary-2-featured-how-to-modify-the-number-of-especially-recommended-six-to-lift-restrictions.html
published: true
post_date: 2013-11-26 17:05:14
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=ShopNC二次开发研究日记">ShopNC二次开发研究日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据研究ShopNC的二次开发过程而写，其中的案例大多来源于QQ群或者爱好者的提问。

-

主页的“特别推荐”区域只有4个商品，不管后台怎么增加还是4个，想要增加为8个怎么办？只能自己修改呗~

根据控制器的流程我们找到了control\index.php文件，然后发现调用这个地方的是_product()方法，然后查阅代码之后会发现，他只允许输出4个商品哈~

$recommend_limit    = 4;//显示个数

哈~写的很清楚嘛~改了

然后，你会发现，怎么没反应？

好吧，其实是因为它使用了缓存，所以我们需要删除缓存文件，缓存文件位于

cache\index\product.php

将这个文件删除，然后你刷新就会发现----还是没反应！

为什么呢？

因为我们后台测试的数据本身就只有4个商品呀，笨死了对吧~哈哈，增加一下吧
<blockquote><em>提示：怎么添加推荐商品</em>
<em>如果你进入“网站》推荐类型》特别推荐”你会发现编辑里面是没有增加商品的入口的，那么怎么增加商品噢？其实，增加的入口在“商品》商品管理”里面选择需要推荐的商品，然后点击最下方的“推荐”，然后选择推荐类型即可。</em></blockquote>
添加完毕你会发现，果然出现8个商品了噢，不过样式需要调整一下，要不难看。

<em><strong><span style="text-decoration: underline;">思路扩展</span></strong></em>

那么只能有一个特别推荐吗？

当然不是，我们在推荐类型里面可以看到好多噢，怎么调用？

系统提供了JS的调用方式，如果你不希望使用怎么办呢？

建议你还是使用JS的方式，要不开发的话需要修改，然后不利于升级呀~

下面谈一下思路。
<pre>    private function _product(){
        if (!$list = F('product','','cache/index')){
            //推荐商品
            $recommend_limit    = 8;//显示个数
            $model_recommend    = Model('recommend');
            $condition    = array(
                'goods_show'=&gt;'1',
                <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">'recommend_id'=&gt;'1',</span></span>
                'limit'=&gt;$recommend_limit,
                'field'    =&gt; 'recommend_goods.recommend_id,goods.goods_id,goods.store_id,goods.goods_name,goods.goods_image,goods.goods_store_price'
            );
            $list    = $model_recommend-&gt;getGoodsList($condition);
        }
        F('product',$list,'cache/index');
        return $list;
    }</pre>
从这个方法中我们可以明白，其实也就是获取数据然后缓存，然后返回给模板输出，那么我只要再写一个方法不就可以了吗？或者更甚我写一个方便的方法呗对吧。批量调用哈~

那么上面代码那个比较关键呢？请看红色部分吧！

然后提供一个综合的方法给大家把，其他的就自己研究，个人觉得应该是用不到的啦。
<pre>    private function _product($id=1,$num=4){
        if (!$list = F('product'.$id,'','cache/index')){
            //推荐商品
            $recommend_limit    = $num;//显示个数
            $model_recommend    = Model('recommend');
            $condition    = array(
                'goods_show'=&gt;'1',
                <span style="text-decoration: underline;"><span style="color: #ff0000; text-decoration: underline;">'recommend_id'=&gt;$id,</span></span>
                'limit'=&gt;$recommend_limit,
                'field'    =&gt; 'recommend_goods.recommend_id,goods.goods_id,goods.store_id,goods.goods_name,goods.goods_image,goods.goods_store_price'
            );
            $list    = $model_recommend-&gt;getGoodsList($condition);
        }
        F('product'.$id,$list,'cache/index');
        return $list;
    }</pre>
自己去测试一下吧~~