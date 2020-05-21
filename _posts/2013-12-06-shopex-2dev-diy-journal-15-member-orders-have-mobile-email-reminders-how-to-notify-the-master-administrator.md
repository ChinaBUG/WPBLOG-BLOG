---
ID: 2974
post_title: >
  ShopEX二次开发DIY日记15之会员下订单会有手机邮箱提醒，怎么改为同时通知站长管理员
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  今天，逛一下群发现有人想要下面的修改：shopex会员下订单会邮件提醒会员，现在我想全部改成邮件通知管理员，在哪里把变量改成管理员邮箱。这个想法很好，因为之前也有客户有类似的需求，不过他的需求是系统有新订单时可以自动发邮件通知他。
  小故事：
  记得当时我问他为什么不直接短信通知他呢？他回答我说，网站发短信需要钱，发邮件不用，而且发送到的邮箱是139邮箱，而移动的邮箱收到邮件通常都会发短信通知你收到邮件了。
  超赞的省钱攻略~~
  那么今天DIY日记的目的就是打造这么一个系统：会员下订单会有手机邮箱提醒，怎么改为同时通知站长管理员。
  我们先来说说这个DIY的前提吧：
  ShopEX相应版本的源码
  懂一点点的PHP基础
  懂得PHP的调试技能
  其他编程相关经验
  上面的要求随便说说啦，不具备的，那就请不要往下看了~~不会要求你一定要懂，但是可以要求你不要看我的文章。
  研究过程啐啐念
  打开\core\model\trading\mdl.order.php或者\core_v5\model\trading\mdl.order.php然后找到
  $this->fireEvent('create',$data,$data['member_id']); //订单生成成功事件
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/shopex-2dev-diy-journal-15-member-orders-have-mobile-email-reminders-how-to-notify-the-master-administrator.html
published: true
post_date: 2013-12-06 09:53:37
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

今天，逛一下群发现有人想要下面的修改：<span style="text-decoration: underline;">shopex会员下订单会邮件提醒会员，现在我想全部改成邮件通知管理员，在哪里把变量改成管理员邮箱</span>。这个想法很好，因为之前也有客户有类似的需求，不过他的需求是系统有新订单时可以自动发邮件通知他。
<blockquote><em>小故事：
记得当时我问他为什么不直接短信通知他呢？他回答我说，网站发短信需要钱，发邮件不用，而且发送到的邮箱是139邮箱，而移动的邮箱收到邮件通常都会发短信通知你收到邮件了。
超赞的省钱攻略~~</em></blockquote>
那么今天DIY日记的目的就是打造这么一个系统：<span style="text-decoration: underline;">会员下订单会有手机邮箱提醒，怎么改为同时通知站长管理员</span>。

我们先来说说这个DIY的前提吧：
<ol>
	<li><em>ShopEX相应版本的源码</em></li>
	<li><em>懂一点点的PHP基础</em></li>
	<li><em>懂得PHP的调试技能</em></li>
	<li><em>其他编程相关经验</em></li>
</ol>
上面的要求随便说说啦，不具备的，那就请不要往下看了~~不会要求你一定要懂，但是可以要求你不要看我的文章。

<strong><span style="text-decoration: underline;">研究过程啐啐念</span></strong>

打开\core\model\trading\mdl.order.php或者\core_v5\model\trading\mdl.order.php然后找到

[php]$this-&gt;fireEvent('create',$data,$data['member_id']); //订单生成成功事件[/php]

然后就是跟踪了解这个的方法的调用情况。
<blockquote><em>为什么？
很多人会问直接这边修改，为什么还要研究？额~我们这个是DIY日记，是教你怎么分析问题，怎么解决我们的需求，而不是教程！不是教你做！</em></blockquote>
直觉上我觉得这个是关键，因为很明显代码里面只有这个跟发送有关的，为什么我这么确认？汗~~因为我有经验呗，那你没经验的话请找到相关的所有地方都去查看，分析代码的业务逻辑，然后自然也就有经验了。

既然这个地方是关键，但是怎么用？我们还是避免不了需要去研究这个方法的调用方式，怎么让这个方法有效的工作呢？

我们查找mdl.order.php文件当前的代码发现没有关于fireEvent方法的定义，那么很自然的我们就只能到他的父类shopObject类去找fireEvent方法，在这个父类里面我们找到fireEvent的定义：

[php]/**
* fireEvent 触发事件
* @param mixed $event
* @access public
* @return void
*/
function fireEvent($action , &amp;$object, $member_id=0){
    $trigger = &amp;$this-&gt;system-&gt;loadModel('system/trigger');
    return $trigger-&gt;object_fire_event($action,$object, $member_id,$this);
}[/php]

很简单，简洁的代码，他又调用另外一个的模块的方法，继续跟踪下去吧，我们找到core\model_v5\system\mdl.trigger.php下面的object_fire_event方法。发现代码好复杂：

[php]function object_fire_event($action , &amp;$object, $member_id,&amp;$target){
    //ob_start();'system.event_listener'
    if(false===strpos($action,':')){
        $trigger_event = $target-&gt;modelName.':'.$action;
        $modelName = $target-&gt;modelName;
    }else{
        $trigger_event = $action;
        list($modelName,$action) = explode(':',$action);
    }
    &lt;span style=&quot;text-decoration: underline; color: #ff0000;&quot;&gt;$type = $target-&gt;typeName;&lt;/span&gt;
    &lt;span style=&quot;text-decoration: underline; color: #ff0000;&quot;&gt;$this-&gt;system-&gt;messenger = &amp;$this-&gt;system-&gt;loadModel('system/messenger');&lt;/span&gt;
    &lt;span style=&quot;text-decoration: underline; color: #ff0000;&quot;&gt;$this-&gt;system-&gt;_msgList = $this-&gt;system-&gt;messenger-&gt;actions();&lt;/span&gt;
    &lt;span style=&quot;text-decoration: underline; color: #ff0000;&quot;&gt;if($this-&gt;system-&gt;_msgList[$type.'-'.$action]){&lt;/span&gt;
    &lt;span style=&quot;text-decoration: underline; color: #ff0000;&quot;&gt;$this-&gt;system-&gt;messenger-&gt;actionSend($type.'-'.$action,$object,$member_id);&lt;/span&gt;
}
......[/php]
<blockquote><em>省略代码作用提示：</em>
<em>如果你对这个省略的代码有兴趣的话，这里给你一个提示噢，我们系统后台有一个“工具箱&gt;自动化处理&gt;网店机器人”的功能，知道怎么增加新的功能跟应用吗？这个就是主要控制程序，执行我们定义的功能，研究这个自然就知道怎么开发机器人功能了。</em></blockquote>
上面截取部分重要的代码，从上面截取的代码上我们可以看到，这个方法就是调用来源的类的方法然后组合成一个请求，然后在调用我们的信息发送类（system/messenger）来发送需要的信息，上面代码的红色划线部分。

他的作用就是构建合适的事件名称，生成合适的模板，然后提交给信息队列，让系统自动来完成信息的发送。

那么我们很明显需要继续跟踪actionSend方法的调用情况了，请打开core\model_v5\system\mdl.messenger.php文件找到actionSend的定义：

[php]function actionSend($type,$data,$member_id=null){
    $actions = $this-&gt;actions();
    $senders = $this-&gt;getSenders($type);
    $level = $actions[$type]['level'];
    $desc = $actions[$type]['label'];
    foreach($senders as $sender){
        $tmpl_name = 'messenger:'.$sender.'/'.$type;
        $contractInfo = $data;
        &lt;span style=&quot;text-decoration: underline;&quot;&gt;&lt;span style=&quot;color: #ff0000; text-decoration: underline;&quot;&gt;if($sender &amp;&amp; ($target = $this-&gt;_target($sender,$contractInfo,$member_id))){&lt;/span&gt;&lt;/span&gt;
        if($level &lt; 9){ //队列
            $this-&gt;addQueue($sender,$target,$desc,$data,$tmpl_name,$level,$type);
        }else{ //直接发送
            $this-&gt;_send($sender,$tmpl_name,$target,$data,$type);
        }
        }
    }
}[/php]

你会看到这个方法的主要部分是由上面划线红色部分在操作的。那么我们再来看_target方法的定义:

[php]function _target($sender,$contectInfo,$member_id){
    $obj = &amp;$this-&gt;_load($sender);
    if(($dataname = $obj-&gt;dataname) &amp;&amp; $contectInfo[$dataname]){
        return $contectInfo[$dataname];
    }else{
        $row = $this-&gt;db-&gt;selectrow('select email,member_id,uname,custom,mobile from sdb_members where member_id='.intval($member_id));
        if($dataname){
           return $row[$dataname];
        }elseif($custom = unserialize($row['custom'])){
            return $custom[$sender];
        }else{
            return false;
        }
    }
}[/php]

然后，我们会发现，分析了半天，这边仅仅是判断是否存在相关的字段，如果不存在则将根据传入的会员ID来获取正确的字段数值。

分析过程完毕，我们可以得到一个结论就是，<em><span style="text-decoration: underline;">其实最初的那个生成订单成功的事件，我们只要传入要发送给对方的信息，然后系统就会自动发送成功的信息给对方了，</span></em>这个信息途径可以是手机短信

站内信息、网站邮件三种，具体是那种主要是后台那边设置的。
<blockquote><em>怎么设置各种事件发送的内容？
进入“会员&gt;会员管理&gt;邮件短信设置”然后打钩需要的事件，然后编辑一下模板即可。</em></blockquote>
从结论中我们知道在

[php]$this-&gt;fireEvent('create',$data,$data['member_id']); //订单生成成功事件[/php]

这一行的代码后面需要怎么折腾跟定制我们的需求了，分析完毕。

<strong><span style="text-decoration: underline;">研究成果展示</span></strong>

从研究结论知道我们要对那个订单生成成功事件做修改，又从代码分析得出，其实只要我们在数据中指定相应的参数值，则系统将会自动为我们发送相关类型的信息。那么我们来修改一下哈~

将

[php]$this-&gt;fireEvent('create',$data,$data['member_id']); //订单生成成功事件[/php]

修改为下面的代码：

[php]$data['email'] = 'seaone@qq.com';//你需要接收邮件的邮箱，最好不要是后台定义的那个
 $this-&gt;fireEvent('create',$data,$data['member_id']);[/php]

然后保存运行，你会发现什么呢？

对了，原来正常发送给下订单账户的邮件，居然转发到你指定的邮箱地址上了！！

然后我们再来修改一下：

[php]unset($data['email']);
 unset($data['member_id']);
 unset($data['mobile']);
 $data['member_id'] = 8;//需要发送的邮箱的会员编号
 $this-&gt;fireEvent('create',$data,$data['member_id']);[/php]

保存运行之后，居然~什么结果你知道吗？！

自己试试吧，不想公布答案了。

然后你会发现你的需求已经解决了！

当然，其实这个是二次开发的形式，个人觉得比较复杂噢，其实我们还可以用APP的形式来开发这个需求，而且更加简单！

敬请期待下一步的介绍吧~