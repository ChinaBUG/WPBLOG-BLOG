---
ID: 2785
post_title: >
  ShopEX二次开发DIY日记12之如何去除ShopEX版权
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  总是感觉为什么这个需求好多人在问？到哪里都有人在发帖问呢？
  好吧，有人问最好，正是我需要的，那就说一下我的解决办法呗~别的地方有没有用我是不知道了，不过我这边的一定是可以用的。
  这边需要申明的是：ShopEX的版权其实不影响什么，如果不是很特别需要的还请保留，你都不付费帮人做个广告总可以了吧？如果连这个都不愿意的话，你懂的。
  本文不支持鼓励大家去除版权，仅作技术探讨之用，有利用本文用于非法用途，与本站无关噢。
  第一种
  思路分析：我们知道版权出现的位置是最下面部分，还是最后一行对吧，所以我们就会想，我不让你显示那就隐藏呗。
  第二种
  思路分析：从第一种上我们知道了，不就是设置一下DIV的属性了，那么我直接给<{footer}>增加一个DIV标签，给包含住，然后再设置隐藏不就简单了？
  第三种
  思路分析：你也许会觉得上面两种很麻烦，还要做加法增加内容，难道就不能做一下减法，删除<{footer}>标签不就可以了？
  第四种
  思路分析：既然做减法，那就干脆直接找到<{footer}>控制代码所在，直接把版权部分取消，而其他的则不变，这不是最轻松的嘛，啥都不用修改？
layout: post
permalink: >
  http://blog.ipodmp.com/2013/11/shopex-2dev-diy-journal-12-how-to-remove-shopex-copyright.html
published: true
post_date: 2013-11-12 09:25:46
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

总是感觉为什么这个需求好多人在问？到哪里都有人在发帖问呢？

好吧，有人问最好，正是我需要的，那就说一下我的解决办法呗~别的地方有没有用我是不知道了，不过我这边的一定是可以用的。

这边需要申明的是：ShopEX的版权其实不影响什么，如果不是很特别需要的还请保留，你都不付费帮人做个广告总可以了吧？如果连这个都不愿意的话，你懂的。

本文不支持鼓励大家去除版权，仅作技术探讨之用，有利用本文用于非法用途，与本站无关噢。

第一种

思路分析：我们知道版权出现的位置是最下面部分，还是最后一行对吧，所以我们就会想，我不让你显示那就隐藏呗。

有的朋友隐藏就直接整个底部都隐藏掉，那个。。。有点傻是不，底部还是有好多东西我们需要使用的哈~

我们知道DIV标签有一个属性是溢出（），这个属性的意思就是限定一个空间，超出的部分可以选择对溢出部分怎么处理，具体请参考<a href="http://www.w3school.com.cn/css/pr_pos_overflow.asp">CSS overflow 属性</a>，下面是部分内容：
<table>
<tbody>
<tr>
<th>值</th>
<th>描述</th>
</tr>
<tr>
<td>visible</td>
<td>默认值。内容不会被修剪，会呈现在元素框之外。</td>
</tr>
<tr>
<td>hidden</td>
<td>内容会被修剪，并且其余内容是不可见的。</td>
</tr>
<tr>
<td>scroll</td>
<td>内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。</td>
</tr>
<tr>
<td>auto</td>
<td>如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。</td>
</tr>
<tr>
<td>inherit</td>
<td>规定应该从父元素继承 overflow 属性的值。</td>
</tr>
</tbody>
</table>
上表就是overflow属性的值列表，我们在这个需求中需要用到hidden值。

我们只要在修改的时候让&lt;{footer}&gt;跟我们需要的内容包含在同一个的DIV标签之中，然后设定一下溢出，指定一下这个DIV标签的高度，然后就正好的隐藏掉版权了噢。

第二种

思路分析：从第一种上我们知道了，不就是设置一下DIV的属性了，那么我直接给&lt;{footer}&gt;增加一个DIV标签，给包含住，然后再设置隐藏不就简单了？

思路是没错的。只需要设置display:none;即可。试试看，不过，如果你没啥经验的话，会发觉用第一种办法来说比较简单，虽然代码多写了一点。

第三种

思路分析：你也许会觉得上面两种很麻烦，还要做加法增加内容，难道就不能做一下减法，删除&lt;{footer}&gt;标签不就可以了？

思路没错，然后去掉之后你会发现网站的部分JS脚本不能使用了。因为ShopEX的一些支持的文件资源都在&lt;{footer}&gt;里面一起输出，取消了，则这些支撑的东西没有输出来，自然程序就不工作了噢。

聪明的你肯定会想到，那就只输出这些需要的东西呗。

先正常执行一次，然后将&lt;{footer}&gt;输出的内容复制，去除掉版权的部分，剩余的直接写进模板尾部，保存之后，版权也没有了噢。

不过这种方式并不建议噢。慎用。不懂代码的孩子容易缺胳膊少腿的，还把本来挺智能化的东西变成死班的应用。

第四种

思路分析：既然做减法，那就干脆直接找到&lt;{footer}&gt;控制代码所在，直接把版权部分取消，而其他的则不变，这不是最轻松的嘛，啥都不用修改？

对，这个思路最好一了百了噢。

那么你知道这个控制代码在哪里嘛？不知道。。。

那你知道这个代码有没有加密嘛？也不知道。。。

嗯，写本文就是要告诉你上面的答案：

控制代码位于：core\include\smartyplugins\function.footer.php或者core\include_v5\smartyplugins\function.footer.php里面，而且这两个文件还是加密的噢。

现在知道难度了吧，其实这个最简单，但是也是最有难度的，这个证明了一件事情，一个挺简单的需求可能需要花费比想象中更多的精力来解决。
<pre>&lt;?php
function tpl_function_footer($params, &amp;$smarty)
{
    $system = &amp;$GLOBALS['system'];
    $output = &amp;$system-&gt;loadModel('system/frontend');
    $tmpdata = $system-&gt;getConf("im.setting");
    $data = unserialize($tmpdata);
    $appmgr = $system-&gt;loadModel('system/appmgr');
    if($appmgr-&gt;openid_loglist()){
        $open_id_login = true; 
    }else{
        $open_id_login = false; 
    }
    $theme_dir = $system-&gt;base_url().'themes/'.$output-&gt;theme;
    echo $smarty-&gt;_fetch_compile_include('shop:common/footer.html',array('theme_dir'=&gt;$theme_dir,'certtext'=&gt;'&lt;a href="http://www.miibeian.gov.cn/ " target="blank"&gt;'.$system-&gt;getConf('site.certtext').'&lt;/a&gt;',
            'mini_cart'=&gt;($system-&gt;getConf('site.buy.target')==3),
            'passport_login'=&gt;$system-&gt;getConf('plugin.passport.config.current_use'),
            'preview_theme'=&gt;$system-&gt;in_preview_theme,
            'im_setting'=&gt;$data,
            'system_url'=&gt;$system-&gt;base_url(),
            'stateString'=&gt;'cron='.urlencode($system-&gt;request['action']['controller'].':'.
                $system-&gt;request['action']['method']).'&amp;p='.urlencode($system-&gt;request['action']['args'][0]),'certi_id'=&gt;$system-&gt;getConf('certificate.id'),'openid_lg_url'=&gt;urlencode($system-&gt;base_url()),'openid_open'=&gt;$open_id_login)
        );
    if(constant('SHOP_DEVELOPER')){
        $html .= $system-&gt;_debugger['log'];
    }
    if($system-&gt;getConf('shopex.wss.show')) {
        $wssjs=$system-&gt;getConf('shopex.wss.js');
    }
    if($system-&gt;getConf('certificate.channel.status') &amp;&amp;                    $system-&gt;getConf('certificate.channel.status')===true){
            $channel = $system-&gt;getConf('certificate.channel.service').'&lt;a href="'.$system-&gt;getConf('certificate.channel.url').'" target="_blank"&gt;'.$system-&gt;getConf('certificate.channel.name');
            $channel =$channel.'&lt;/a&gt;';
    }
    if($system-&gt;getConf('site.shopex_certify')==0){
      //站点底部
        $ref = $_SERVER['HTTP_HOST'];
        $check = md5($ref.'ShopEx@Store');
        $str = urlencode($system-&gt;getConf('certificate.str'));
        if(!$str){
            $str = urlencode(__('无'));
        }
        if(constant('SAAS_MODE')){
            $versionStr='';
        }else{
            $versionStr='v'.$system-&gt;_app_version;
        }

        if($system-&gt;use_gzip) {
            $gzip = 'enabled';
        } else {
            $gzip = 'disabled';
        }
        $themeFoot='&lt;div&gt;'.$system-&gt;getConf('system.foot_edit').'&lt;/div&gt;';
        $PoweredStr='&lt;div style="font-family:Verdana;line-height:20px!important;height:auto!important;font-size:11px!important;text-align:center;overflow:none!important;text-indent:0!important;"&gt;';
        if ($system-&gt;getConf('certificate.auth_type')=="commercial"){
            $greencard = $system-&gt;getConf('store.greencard');
            if (!isset($greencard)||$greencard)
            $PoweredStr.="&lt;a href='http://service.shopex.cn/show/certinfo.php?certi_id=".$system-&gt;getConf('certificate.id')."&amp;url=".rawurlencode($system-&gt;base_url())."' target='_blank'&gt;&lt;img src='statics/bottom-authorize.gif'&gt;&lt;/a&gt;&lt;br&gt;";
        }
        $PoweredStr.='&lt;a href="http://store.shopex.cn/rating/store_detail.php?ref='.$ref.'&amp;check='.$check.'&amp;str='.$str.'" target="_blank" style="color:#666;text-decoration:none;cursor:pointer"&gt;';
        $PoweredStr.='Powered&amp;nbsp;by&amp;nbsp;&lt;b style="color:#5c719e"&gt;Shop&lt;/b&gt;&lt;b style="color:#f39000"&gt;Ex&lt;/b&gt;';
        $PoweredStr.='&lt;/a&gt;';
        $PoweredStr.='&lt;span style="font-size:9px;"&gt;&amp;nbsp;'.$versionStr.'&lt;/span&gt;';
        $PoweredStr.='&lt;span style="color:#999;display:none"&gt;&amp;nbsp|Gzip '.$gzip.'&lt;/span&gt;&amp;nbsp;';
        if($channel){
        $PoweredStr.='&lt;br/&gt;&lt;span&gt;'.$channel.'&lt;/span&gt;&amp;nbsp;';
        }
        if($system-&gt;getConf('site.certtext')){
        $PoweredStr.='&lt;a href="http://www.miibeian.gov.cn/" target="blank" style="color:#666;text-decoration:none;cursor:pointer;display:block;"&gt;'.$system-&gt;getConf('site.certtext').'&lt;/a&gt;';
        }
        if($wssjs){
        $PoweredStr.='&lt;span style="display:none"&gt;'.$wssjs.'&lt;/span&gt;';
        }
        $PoweredStr.='&lt;/div&gt;';
    }
    return $html.$themeFoot.$PoweredStr;
}
?&gt;</pre>
看到代码，相信你可以修改了吧，请将代码里面的$PoweredStr相关的代码都给修改了，或者清空也可以，保存为core\include\smartyplugins\function.footer.php和core\include_v5\smartyplugins\function.footer.php两文件，然后清空缓存，打开网站就会发现，哇咧版权没有了。

嗯，这四种方式是我经常用的，当然，最后一种经常性就是收费解决的，现在免费公开了噢。

大家去试试吧。