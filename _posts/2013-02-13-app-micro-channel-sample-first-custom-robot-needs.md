---
ID: 2320
post_title: >
  APP-微信自定义机器人的最初需求样本
author: ChinaBUG
post_excerpt: |
  虫曰：话说最近微信的应用比较火哈~所以偶也做了点研究，开发了一点，苦于不懂得怎么教文案写需求，正好看到白鸦的需求样本，写的不错哈，摘抄一下。
  本站目前开发的微信应用有（请使用微信添加应用名为好友来测试或者扫描下面的二维码添加关注）：
  百科全微	赶圩网xu.com	专业程序二次开发
  目前计划是用作日常的便民服务，提供快递到件查询，电话号码查询等
  这个是所在负责的商城官方的服务号，目前还在思考提供什么服务
  我自己的淘宝店铺的官方微信号，用作宝贝推荐。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/02/app-micro-channel-sample-first-custom-robot-needs.html
published: true
post_date: 2013-02-13 15:49:07
---
虫曰：话说最近微信的应用比较火哈~所以偶也做了点研究，开发了一点，苦于不懂得怎么教文案写需求，正好看到白鸦的需求样本，写的不错哈，摘抄一下。

本站目前开发的微信应用有（请使用微信添加应用名为好友来测试或者扫描下面的二维码添加关注）：
<table>
<tbody>
<tr>
<td style="text-align: center;">百科全微</td>
<td style="text-align: center;">赶圩网xu.com</td>
<td style="text-align: center;">专业程序二次开发</td>
</tr>
<tr>
<td style="text-align: center;">目前计划是用作日常的便民服务，提供快递到件查询，电话号码查询等</td>
<td style="text-align: center;">这个是所在负责的商城官方的服务号，目前还在思考提供什么服务</td>
<td style="text-align: center;">我自己的淘宝店铺的官方微信号，用作宝贝推荐。</td>
</tr>
<tr>
<td><img alt="百科全微" src="http://blog.ipodmp.com/wp-content/uploads/2013/02/ico_baikequanwei_430.jpg" width="150" /></td>
<td><img alt="赶圩网xu.com" src="http://blog.ipodmp.com/wp-content/uploads/2013/02/ico_ganxu78_430.jpg" width="150" /></td>
<td><img alt="专业程序二次开发" src="http://blog.ipodmp.com/wp-content/uploads/2013/02/ico_php2dev_430.jpg" width="150" /></td>
</tr>
</tbody>
</table>
前不久给微信提了几个未来可以通用的接口需求，我认为可以作为很多企业的智能客服去使用，也可以配套上他们的CRM。虽然到目前为止Guang.com的一个兼职工程师还没有完全实现这些设计，但依然还是挺好玩的～

最近有不少新的微信账户在使用这些接口（可以添加如下好友来测试：订酒店、下厨房、逛），也有人在讨论这些接口可以怎么用。看在没啥商业机密的份上，奉献出我最早的需求邮件（因为是写给具体开发和设计师的，因为不能当面沟通，所有相当啰嗦，希望你能有耐心看完）

——————————————————-
发件人: “白鸦”;
发送时间: 2012年8月31日(星期五) 凌晨2:08
收件人：xxxxxxxxxxxxxxxxxxx
主题: 关于Guang.com “消费助手” 的相关用例和产品需求

各位，实在没时间详细写需求，我就用邮件大概说一下吧。

整体思路上我打算把“消费助手”当作个性化推荐引擎来玩的。不过初期我打算从问答机器人开始，以后逐渐加入个性化的回答、餐馆推荐等，以及备忘秘书、降价通知、附近美食、菜品做法、天气查询等内容。下面是目前第一步打算实现的用例：

1、用户添加“逛”为微信好友。

2、添加成功后，Guang会自动发送一条欢迎和提示的信息给用户。
如，“感谢您的关注！如果您需要找什么好东西，可以问我，我会尽力给你推荐 :) ”

3、用户使用文字或语音提出自己的需求。
如，“有什么适合在办公室里喝水的马克杯或保温杯”，或一段这样的语音。

4、如果是语音，我们会调用Google的API接口，让Google返回理解过后的文字信息给我们；如果是文字我们就直接采用。然后，系统会将用户提出的需求进行分词(只拆出我们熟悉的词)，并分析出这些词是属于“子类目”还是“一般标签”。
如，按上例中的需求，我们可能会分出：“办公室”、“马克杯”、“保温杯”、“保温”、“办公”五个我们熟悉的词，其中我们可以知道“马克杯”和“保温杯”是“子类目”，其他的词会全部当作“一般标签”处理。

5、然后系统会在“子类目”里检索出包含(绝对匹配)了其中某个“一般标签”的商品，随后再按照匹配度、推荐权重将他们进行排序。
（匹配度排序：包含的越多排序越靠前；推荐权重：点击数、购买数、喜欢数、鉴定数的综合评分）
如，按上例，我们会在“马克杯”和“保温杯”这两个子类目下，找出商品名、小编推荐、商品标签包含有“办公室”、“保温”、“办公”这三个标签里多个或一个标签的商品。随后再按照匹配度、推荐权重将他们进行排序。

6、最后，Guang 会发送两条微信给用户。
第一条图文信息：多条图文信息列表，大图是匹配度排名第一的商品，后面2个小图是荐权重排名前4位的商品，随机2条（随机会让事情更有乐趣）。
如果这三个商品经过去重后，只有一个，则用一条图文信息的方式发送。
第二条文本信息（为每个用户每天最多只发一条这样的信息）：还想要更多可以发送“再来一次”试试。如果没有你喜欢的可以告诉我们“不喜欢”。

如，按上例，用户收到的信息会是：
第1条图文信息：（请参照微信公众平台发布出来的多条图文信息和单挑图文信息）
第2条文本信息：还想要更多可以发送“再来一次”试试。如果没有你喜欢的可以告诉我们“不喜欢”。

6.1、如果我们一个商品也没有匹配上，会发送一条“抱歉，我们暂时还没有发现适合你这个需求的好东西。稍后我们会继续为您搜寻，如果找到我们会在24小时内推荐给您”。同时，这个需求会被打上星标，转为人工处理。

6.2、再来一次就是把匹配度第二个，推荐权重的另外两个给他。如此类推。

7、如果用户给我们的信息中带有“不喜欢”三个字，系统会为该信息自动打上星标。转为人工处理。

ps1：万一我们一开始还不能支持语音，当用户发送一个语音过来的时候，我们会返回“麻烦您打字吧，我们暂时还听不懂您说的话”
ps2：当用户发送一个语图过来的时候，我们会返回“麻烦您打字吧，我们暂时还看不懂你的图”
ps3：如果他发了表情，我们会利用现有的微信公众平台的自动回复功能调戏他们或者不理他

——————–

以上用例简单来说就是：
a将用户发来的信息和用户的基本信息给我 》
b按照我的指令给用户发信息并将他存到对话记录里 》
c部分特殊信息麻烦自动帮我在后台打个星标以便我人工处理 》
d我发信息的时候你就不要发了。

根据以上用例，我设想：

首先，需要继续使用目前公共平台的后台，包括公众平台里的“自定义回复”。（用这些是为了省事儿，我们不用再走任何后台，只要一个工程师和随便抽空加上一个文案的人帮点忙，就能搞定这个机器人了）

然后，具体分别需要：
1、2、3点：目前公众平台已经实现；
3、4点，在3用户发送信息给我们后，需要微信将相关文字、图片或语音信息通过接口传送给我们，并告知这条信息的标识号。同时，将用户的微信ID、微信名、地区、性别、也一并发送给我（我需要分析不同属性用户的偏好，便于下一步针对不同的人返回不同的结果）。
5点，纯粹是Guang的内部机制
6点，麻烦提供微信现有的“文本信息”、“图片信息”、“语音信息”、“多条图文信息”、“单条图文信息”的格式要求，我将按照这个要求提供相应的信息，以及信息对应要发送给的人。然后通过相关接口将这些传给微信，微信帮我发出去，并将发送的记录存进当前公众平台单个用户的对话记录里。
6.1点，如果我返回给用户的信息是“无结果”的时，我会同时将用户的原始标识号发送给微信，麻烦微信帮我将这个原始信息在现有公众平台里加注“星标”。
7点，用户说“不喜欢”的时候，我会同时将用户的原始标识号发送给微信，麻烦微信帮我将这个原始信息在现有公众平台里加注“星标”。
6点、ps1、ps2、ps3，用户发了一条信息过来，当遇到这些我们系统给微信发了信息的时候，微信现在后台的“自动回复”应该失效。（避免同时发送用用户晕头）

我不懂技术，肯定有不少具体的实现细节我想的并非合理方案，欢迎大家在不改变我需求出发点的基础上，提出更好的方案或建议来。发现有任何我没表达清楚的地方可以直接电话我.

—
白鸦 ，Guang.com

.
以上内容目前可以在微信里添加如下好友来测试：订酒店、下厨房、逛。 （转载此文章者请带上这条广告）