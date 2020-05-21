---
ID: 3118
post_title: >
  mootools-DIY系列2:模仿淘宝设计一个商品飞入购物车的特效果的插件
author: ChinaBUG
post_excerpt: |
  mootools-DIY系列1:模仿淘宝设计一个商品飞入购物车的特效果看完了，是不是觉得比较简单噢，那么本系列2我们继续来个高深的，怎么封装这个代码变成插件，可以让人下载回去直接使用的呢？
  怎么来写一个mootools的插件呢？！
  推荐阅读老外的专著：How to write a Mootools Class 非常棒的一个教程，从零教你怎么制作一个插件类噢
  下面来写一个mootools.fly2cart.js插件吧~~
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/mootools-diy-series-2-taobao-imitate-the-design-of-a-product-into-the-shopping-cart-to-fly-special-effects-plug-ins.html
published: true
post_date: 2013-12-25 11:13:24
---
<a title="mootools-DIY系列1:模仿淘宝设计一个商品飞入购物车的特效果" href="http://blog.ipodmp.com/archives/mootools-diy-series-1-taobao-imitate-the-design-of-a-product-into-the-shopping-cart-flying-effects/">mootools-DIY系列1:模仿淘宝设计一个商品飞入购物车的特效果</a>看完了，是不是觉得比较简单噢，那么本系列2我们继续来个高深的，怎么封装这个代码变成插件，可以让人下载回去直接使用的呢？

怎么来写一个mootools的插件呢？！

推荐阅读老外的专著：<a href="http://mootorial.com/wiki/mootorial/09-howtowriteamootoolsclass">How to write a Mootools Class</a> 非常棒的一个教程，从零教你怎么制作一个插件类噢

下面来写一个mootools.fly2cart.js插件吧~~

第一步：
<pre>var fly2cart= new Class(
    {
    Implements: [Options, Events],
    options: {},
    initialize: function(options){
        this.setOptions(options);
    }
});</pre>
第二步：
<pre>var fly2cart= new Class(
    {
    Implements: [Options, Events],
    options: {
        cartID:'shopping-cart',
        items:'.items',
        item:'.item',
        addtocart:'.add-to-cart',
        imgstag:'img',
        fxduration:1000,
        opacity:.5
    },
    initialize: function(options){
        this.setOptions(options);
        this.runfly2cart();
    },
    runfly2cart: function(){
        var cart_pos = $( this.options.cartID ).getCoordinates();
        $$( this.options.items + ' ' + this.options.addtocart ).addEvent('click', function (e) {
            var obj = e.target;
            var obj_good_img = obj.getParent( this.options.item ).getFirst( this.options.imgstag );
            var obj_good_img_clone = obj_good_img.clone();
            obj_good_img_clone.setStyles(obj_good_img.getCoordinates()).setStyles({position: 'absolute'}).inject(document.body);
            var fx = new Fx.Morph(obj_good_img_clone, {duration: 1000,link: 'chain'});
            fx.start({
                top: (cart_pos.top + (cart_pos.height/2)),
                left: (cart_pos.left + (cart_pos.width/2)),
                opacity: .5,
                width: 0,
                height: 0});
        }.bind(this));
    }
});</pre>
第三步：
<pre>window.addEvent('domready', function(){
    new fly2cart();
});
或者
window.addEvent('domready', function(){
    new fly2cart({
        cartID:'shopping-cart',
        items:'.items',
        item:'.item',
        addtocart:'.add-to-cart',
        imgstag:'img',
        fxduration:1000,
        opacity:.5
    });
});</pre>
然后你就可以看到效果了噢~~帅吧~ 完整的代码与效果请看： <iframe src="http://jsfiddle.net/ChinaBUG/nAB5b/embedded/" height="300" width="100%" allowfullscreen="allowfullscreen" frameborder="0"></iframe> 当然，这边我去掉了一些容错等信息的代码，对容错，版权等比较重视的人可以自己加噢。