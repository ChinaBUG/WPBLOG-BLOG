---
ID: 2548
post_title: >
  API-京东商城APIs接口开发经验分享(商家类型授权登陆接口调用转换时错误)
author: ChinaBUG
post_excerpt: |
  1、FBP,LBP,SOPL,SOP商家类型说明
  2、获取商家的物流供应商jingdong.logistics.carriers.list接口与360buy.delivery.logistics.get接口
  3、API接口调用时出现错误62，提示信息为json conversion error（json转换时错误）是什么问题呢？
  4、此接口无需输入应用级参数，但需要构建空参数360buy_param_json={}
  5、输入运单号，物流公司，然后通过API更新店铺的订单，而且希望订单能自动设置为发货状态。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/08/api-jingdong-mall-apis-interface-development-experience-sharing.html
published: true
post_date: 2013-08-28 10:58:57
---
在开发京东商城APIs应用时，感谢技术李华宁同学的耐心解答，非常感激。

<strong><span style="text-decoration: underline;">1、FBP,LBP,SOPL,SOP商家类型说明</span></strong>

FBP :京东给商家一个独立操作的后台， 商家五地入库（北京，上海，广州，成都，武汉），从仓储 到配送 到客服都是京东来操作， 京东本身自营的产品所有能享受的服务，商家都能享受（支持211限时达，自提，货到付款，POS刷卡），客户体验值最高。

LBP : 京东给商家一个独立操作的后台， 商家无须入库，要求订单产生后12小时内将产生的订单包装好发货，分别发货到京东的五地仓储，36小时内到京东商城仓库，由京东来开具给消费者的发票。

SOPL: 京东给商家一个独立操作的后台， 商家无须入库，要求订单产生后12小时内将产生的订单包装好发货，分别发货到京东的五地仓储，36小时内到京东商城仓库，由商家来开具给消费者的发票。

SOP: 京东给商家一个独立操作的后台， 跟淘宝商城模式比较类似， 要求订单产生后12小时内发货，由商家来承担所有的服务

<span style="text-decoration: underline;"><strong>2、获取商家的物流供应商jingdong.logistics.carriers.list接口与360buy.delivery.logistics.get接口</strong></span>

面向商家服务 -&gt; 物流服务中的接口为京东青龙物流系统仓储部分对应接口。

如果您是商家，需要获取物流公司id，请调取面向商家服务 -&gt; 配送服务中商家获取物流公司接口。

SO如果要获取商铺的配送方式及其ID的话，那么只能使用360buy.delivery.logistics.get接口而不是jingdong.logistics.carriers.list接口。

<span style="text-decoration: underline;"><strong>3、API接口调用时出现错误62，提示信息为json conversion error（json转换时错误）是什么问题呢？</strong></span>

我在调用360buy.delivery.logistics.get及jingdong.logistics.carriers.list时，会出现如下提示：

Array ( [error_response] =&gt; Array ( [code] =&gt; 62 [zh_desc] =&gt; json conversion error [en_desc] =&gt; json转换时错误 ) )

参考文档说明：

<em><span style="text-decoration: underline;">③ 业务参数错误</span></em>
<em><span style="text-decoration: underline;">这种错误一般是由于服务端（服务提供者）异常引起的。用户遇到这类错误需要隔一段时间再重试就可以解决。</span></em>

这是很奔溃的一个问题，只能自己找问题了。通过跟踪发现调用json的地方是在JdApi2.php文档中的

$apiParams["360buy_param_json"] = $this-&gt;getAppJsonParams($itemParam);

这个地方，在调用getAppJsonParams时会出现错误，你说我怎么知道的？还不是一步步的注释一步步的找下来，脑子不好，只能用这种土办法了~~

然后你会发现之所以出错是因为$itemParam变量为空造成的。为什么$itemParam变量会为空噢？其实就是不懂得这个接口人怎么弄得误会引起的哈，我们看看API文档对这两个API接口的说明中出现一个参数说明：

<strong>应用级输入参数</strong>
<table width="556" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="112">
<p align="center"><strong>名称</strong></p>
</td>
<td valign="top" width="74">
<p align="center"><strong>类型</strong></p>
</td>
<td valign="top" width="104">
<p align="center"><strong>是否必须</strong></p>
</td>
<td valign="top" width="107">
<p align="center"><strong>示例值</strong></p>
</td>
<td valign="top" width="158">
<p align="center"><strong>描述</strong></p>
</td>
</tr>
<tr>
<td valign="top"></td>
<td valign="top"></td>
<td valign="top"></td>
<td valign="top"></td>
<td valign="top">此接口无需输入应用级参数，但需要构建空参数360buy_param_json={}</td>
</tr>
</tbody>
</table>
需要构建空参数360buy_param_json={}！怎么构建？估计基础差的人都是不懂，我也是不懂，所以我是直接放空，所以就出现这个问题了，那就不放空呗，随便填写一个参数，然后你会发现问题解决了。如果这样子还是不行的话，那么你需要继续看下面的内容了，下面将告诉你怎么处理这个问题。

<span style="text-decoration: underline;"><strong>4、此接口无需输入应用级参数，但需要构建空参数360buy_param_json={}</strong></span>

怎么构建？像我等没有基础的人会有人不懂这句话是什么意思，哎，经过我的研究发现如下解决方案。

4.1直接在生成的地方增加这个字符串呗：

找到JdApi2.php文档的$requestUrl = $this-&gt;buildUrl($sysParams);处，直接字符串添加上360buy_param_json={}，就可以了，只要组合成类似如下的网址即可：

http://gw.api.360buy.com/routerjson?360buy_param_json={}&amp;access_token=[你自己的]&amp;app_key=[你自己的]&amp;method=360buy.delivery.logistics.get&amp;timestamp=2013-05-30 00:00:00&amp;v=2.0&amp;sign=E981702AF260F37FCCD7D60FD19AAEA7

4.2那就修改一下程序吧，省的每次自己添加多麻烦呀：

找到JdApi2.php文档的getAppJsonParams方法，将这个方法改为如下：
<pre>public function getAppJsonParams($itemParam){
    ksort($itemParam);
    if($itemParam){
        return json_encode($itemParam);
    }else return '{}';
}</pre>
然后执行就可以正常了。

5、输入运单号，物流公司，然后通过API更新店铺的订单，而且希望订单能自动设置为发货状态。

在API中找到两个360buy.order.sop.waybill.update 与360buy.order.sop.outstorage，从字面上理解前者是“SOP修改运单号”，后者是“订单SOP出库”，用过才知道，出库就是点击发货呀，点击完毕发货，才能使用修改运单号，汗~

另外，我使用360buy.order.sop.waybill.update 接口结果出现提示：

Array ( [error_response] =&gt; Array ( [code] =&gt; 10400014 [zh_desc] =&gt; 褰撳墠鐘舵€佺殑璁㈠崟涓嶈兘淇敼杩愬崟鍙� [en_desc] =&gt; order state is not suitable for modifying waybill number ) )

SO应该使用360buy.order.sop.outstorage接口。哎，看不懂京东的文档。

PS：附上官方技术的答复

请使用订单SOP出库接口（360buy.order.sop.outstorage）完成出库操作，订单状态也会随之变为“WAIT_GOODS_RECEIVE_CONFIRM 等待确认收货”。SOP修改运单号接口（360buy.order.sop.waybill.update）的功能是修改物流运单号，只有在完成出库操作后，方可调用该接口。