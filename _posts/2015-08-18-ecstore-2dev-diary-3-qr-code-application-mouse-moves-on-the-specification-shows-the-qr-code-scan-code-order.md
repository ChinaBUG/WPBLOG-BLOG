---
ID: 4152
post_title: >
  ECStore二次开发日记之3.二维码应用：鼠标移到多规格上就显示二维码，扫码下单
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2015/08/ecstore-2dev-diary-3-qr-code-application-mouse-moves-on-the-specification-shows-the-qr-code-scan-code-order.html
published: true
post_date: 2015-08-18 15:12:24
---
序

本身ECStore系统是支持二维码购物的，但是，他是后台查看商品时扫描二维码之后自动添加到购物车内，而前台却是扫描后进入商品详情页。

文

运营的同仁希望鼠标移到商品的规格上，就要显示出当前规格的二维码，扫描之后自动添加到购物车中。

要实现这个需求，我们分如下几步：
<ol>
	<li>怎样可以扫码直接加入购物车</li>
	<li>怎么显示二维码</li>
	<li>怎么在商品规格上添加二维码的效果</li>
</ol>
根据查看WAP的购物车程序发现，如果一个商品链接后面带有qr参数的话，那么扫描之后打开会自动加入购物车。

具体代码位于app &gt; b2c &gt; controller &gt; wap &gt; product.php内的大约74行：
<pre>        if( $_GET['qr'] ){
            $url = $this-&gt;gen_url(array('app'=&gt;'b2c','ctl'=&gt;'wap_cart','act'=&gt;'qrCodeAddCart','arg0'=&gt;$productId));
            $this-&gt;redirect($url);
        }</pre>
判断有参数直接转向~

那么现在就剩下怎么将二维码显示出来了。

我们查看一下代码发现在app &gt; b2c &gt; view &gt; site &gt; product &gt; index.html文件中的501行中：
<pre>var state;
container.addEvents({
......</pre>
这个代码中集中监控了多规格的点击事件，那么我们能不能增加上去呢？

答案是肯定的。

那么我们增加的问题解决就会遇上另外一个问题就是，二维码的来源，就是这个二维码在哪里呢？系统本身有吗？

好吧，经过分析还是自己生成吧，不管生成缓存还是生成图片文件，都是需要自己写代码实现的。

综上所述，我们目前要解决的问题是：
<ol>
	<li>怎么实现二维码的生成</li>
	<li>怎么实现调用二维码并展示</li>
</ol>
现在我们来解决第一个问题，如何生成我们需要的二维码。

ECStore系统中已经集成了PHP QR Code库了，它是一个生成二维码的PHP类库，具体用法直接百度狗狗之，我这边不详细介绍，就说一下如何在ECStore中调用吧。

我们在app &gt; b2c &gt; controller &gt; site &gt; product.php文件中增加一个方法：
<pre>    /*
    2015.07.22 ChinaBUG 二维码生成
    用于前台商品详情的商品页面的二维码显示。
    */
    function erweima( $pid = 29408,$gid = 0,$addToCart = 0,$debug = 0 )
    {
        header("Cache-Control:no-store, no-cache, must-revalidate"); // HTTP/1.1
        header("Expires: Mon, 26 Jul 1997 05:00:00 GMT");// 强制查询etag
        header('Progma: no-cache');
        //
        $url = array(
            'app'=&gt;'b2c',
            'ctl'=&gt;'wap_product',
            'full'=&gt;1,
            'arg0'=&gt;$pid,
        );        
        $url = app::get('wap')-&gt;router()-&gt;gen_url( $url );
        // 自动加入购物车
        // $addToCart = false;
        if( $addToCart ) $url .= '?qr=1';
        // 缓存KEY
        $cacheKey = 'qrcode_'.$pid.'_'.$addToCart;
        if( !cachemgr::get( $cacheKey,$content ) )
        {
            cachemgr::co_start();
            //二维码纠错级别
            $errorCorrectionLevel = 'L'; //L M Q H
            //二维码生成图片大小 1-10
            $matrixPointSize = 4;
            ob_start();
                weixin_qrcode_QRcode::png( $url, false, $errorCorrectionLevel, $matrixPointSize, 2);
                $content = ob_get_contents();
            //ob_clean();
            ob_end_clean();
            cachemgr::set( $cacheKey, $content, cachemgr::co_end() );
        }
        //
        if($debug == 0){
            header("Content-type: image/png");
            echo $content;
        }else{
            print_r(array(
                        'pid'=&gt;$pid,
                        'gid'=&gt;$gid,
                        'addToCart'=&gt;$addToCart,
                        'cacheKey'=&gt;$cacheKey,
                        'cachemgr::get(errorCorrectionLevel:L)'=&gt;$errorCorrectionLevel,
                        'content'=&gt;$content,
                        'preview'=&gt;'&lt;div style="height:132px;width:132px;background-image:url('data:image/png;base64,'.base64_encode($content).'')"&gt;&lt;/div&gt;'
                    ));
        }        
    }</pre>
这个方法用于前台的二维码调用，第一次调用会生成缓存。

第二个问题就是如何展示二维码，这个时候我们需要考虑的是，要直接用AJAX调用上面的方法，然后定位显示，还是直接输出结构，再来显示呢？

我选择的是输出结构，然后用JS来控制显示隐藏的办法，为什么要选择这个，跟我自己的考量有关系，起码觉得速度快一点。

好了，现在请打开app &gt; b2c &gt; view &gt; site &gt; product &gt; info &gt; spec.html文件，然后找到：
<pre>              &lt;span&gt;&lt;{$item.spec_value}&gt;&lt;/span&gt;
              &lt;{/if}&gt;
              &lt;i&gt;&lt;/i&gt;
            &lt;/a&gt;</pre>
改为如下：请注意闭合的标签噢。
<pre>              &lt;span&gt;&lt;{$item.spec_value}&gt;&lt;/span&gt;
              &lt;{/if}&gt;
              &lt;i&gt;&lt;/i&gt;
            &lt;/a&gt;
                &lt;div class="pre-qr" style="width:0px;height:0px;overflow: hidden;"&gt;
                    &lt;div style="float:left;width:132px;height:132px;"&gt;
                        &lt;img style="width:132px;height:132px;" src="&lt;{link app=b2c ctl=site_product act=erweima arg0=$item.product_id?$item.product_id:$page_product_basic.product_id arg1=$page_product_basic.goods_id arg2=1}&gt;"/&gt;
                    &lt;/div&gt;
                    &lt;div style="float:right;padding-top: 40px;"&gt;扫一扫&lt;br/&gt;直接通过二维码下单&lt;/div&gt;
                &lt;/div&gt;</pre>
&nbsp;

这段代码用途是：在每一个规格下面都输出一个二维码图片，然后用于我们脚本的调用，脚本控制他的显示跟隐藏哈。请注意重点是调用生成二维码的方法噢。

现在我们再来看看怎么调用这个图片结构来显示二维码。

请打开app &gt; b2c &gt; view &gt; site &gt; product &gt; index.html文件中，找到：
<pre>    'click:relay(.spec-attr)': function(e){
        if(this.hasClass('selected')) return;
        var a = this.getElement('a');
        var url = a.href;
        var id = a.rel;
        if(!id) return;
        if (window.history &amp;&amp; history.pushState) {
            e.stop();
            /*html5 history manage*/
            if(ajax){
                ajax.cancel();
            }
            else {
                state = {title: document.title, html: container.innerHTML, url: location.href, id: id};
            }
            ajax = Query.send(Router.basic(id), {
                method: 'post',
                onSuccess: function(rs) {
                    try{
                        rs = JSON.decode(rs);
                        if(rs.error) {
                            return Message.error(rs.error);
                        }
                    }catch(e) {
                        updateBasic(rs, id, url);
                    }
                }
            });
        }
    }
});</pre>
然后在尾部});这个前方回车加入一行，然后写上如下代码：
<pre>   'mouseenter:relay(.spec-attr a)': function(event){    
        var pro_qrcode = $('product-action-qrcode').getCoordinates();
        if( pro_qrcode.top == null ) return;
    
        console.log( '-----------------' );
        console.log( event.target.tagName );
        event.stopPropagation();
        var obj = $(event.target),
            marginWH = 0;
        Coord = obj.getCoordinates();
        console.log( '---'+Coord.top+'PX---'+Coord.left+'PX---'+Coord.width+'PX---'+Coord.height+'PX-----' );
        if( event.target.tagName == 'A' )
        {
            obj = obj.getNext();            
        }
        else if( event.target.tagName == 'IMG' || event.target.tagName == 'SPAN' )
        {
            obj = obj.getParent().getNext();
            if( event.target.tagName == 'SPAN' ) marginWH = 8;
        }
        else if( event.target.tagName == 'I' )
        {
            return;
        }
        //
        if( obj == null ) return;
        //
        obj.setStyles({
            top: Coord.top + 15 - marginWH,
            left:Coord.left - 15 - marginWH,
            position: 'absolute',
            'z-index': 999999,
            'background-color': '#fff'
        });
        //
        if( showqrState == false){
            showqrState = true;
            var myFx = new Fx.Elements( 
                obj,
                {
                    onComplete: function(){
                        showqrState = false;
                    }
                }
                ).start({
                0: {
                    top: [Coord.top + 15 - marginWH, pro_qrcode.top-41],
                    left:[Coord.left - 15 - marginWH,pro_qrcode.left-41],
                    width: [0,240],
                    height: [0,140],
                    opacity: [0,1]
                }
            });
        }
        console.log( '-----------------' );
    },
    'mouseleave:relay(.spec-attr)': function(event) {
        $$('.pre-qr').setStyles({
            width:0,
            height:0,
            overflow:'hidden'
        });
    }</pre>
一个是鼠标移上去的事件，一个是鼠标移开后的事件。

第一个事件的原理是，有一个设定的位置，那个位置的ID是product-action-qrcode，获取他的所在坐标，然后把规格的二维码按照条件给他显示在这个坐标上，覆盖他。要特别注意的是事件对象的获取，就是代码中的event.target.tagName的判断，本来想要取消事件冒泡，可结果在测试的使用出现特效冲突，最后就辛苦一下了，全部判断吧。具体为啥就不懂了。^_^

然后，保存，刷新，你就会发现鼠标移上去就会出现我们想要的二维码，然后扫描之后就直接手机打开，并自动添加到购物车中了。

&nbsp;