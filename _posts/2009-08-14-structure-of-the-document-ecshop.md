---
ID: 153
post_title: ECShop的文件架构
author: ChinaBUG
post_excerpt: >
  Ecshop是国内用的比较多的几个网店之一，相对于ShopEx，比较简单些，要进行二次开发也比较容易些。在进行二次开发时，对其的文件结构就必须有很充分的认识，下面是我收集整理到的EcShop的文件结构，有了这张图，在开发时要方便不少，特此留下记录，以后查起来也方便些。
layout: post
permalink: >
  http://blog.ipodmp.com/2009/08/structure-of-the-document-ecshop.html
published: true
post_date: 2009-08-14 15:07:40
---
 　　Ecshop是国内用的比较多的几个网店之一，相对于ShopEx，比较简单些，要进行二次开发也比较容易些。在进行二次开发时，对其的文件结构就必须有很充分的认识，下面是我收集整理到的EcShop的文件结构，有了这张图，在开发时要方便不少，特此留下记录，以后查起来也方便些。
 
　　模板使用简易说明

　　以下为引用的内容：

　　一、模板系统介绍

　　Ecshop模板系统使用我们自行研发的模板控制系统和著名的PHP开源模板程序Smarty组合而成。为了方便用户开发模板，还使用了Dreamweaver的模板和库的功能。

　　二、模板设计原则

　　二、模板系统

　　文件结构

Ecshop根目录/
        |
        |-&gt;其它目录
        |-&gt;themes
                |-&gt;例:default (模板项目目录)
                                |-&gt;images                      (模板图片目录)
                                |-&gt;library                      (模板库目录)
                                |-screenshot.png        (模板预览图)
                                |-style.css                      (模板所使用样式表)
                                |-article.dwt                 (阅读文章)
                                |-article_cat.dwt        (文章列表)
                                |-category.dwt            (分类列表)
                                |-compare.dwt            (商品比较)
                                |-gallery.dwt                (商品图片)
                                |-goods.dwt                  (商品)
                                |-index.dwt                   (商城首页)
                                |-respond.dwt              (支付)
                                |-secarch_result.dwt        (搜索结果)
                                |-shopping_flow.dwt        (购物流程)
                                |-snatch.dwt                        (夺宝奇兵)
                                |-user.dwt                                (用户中心)
 

 <strong>library 说明</strong>
以下为引用的内容：
articles.lbi - 文章列表
article_info.lbi - 文章内容
article_list.lbi - 文章列表
best_goods.lbi - 精品推荐
bought_goods.lbi - 购买过此商品的人购买过哪些商品
brand_goods.lbi - 品牌的商品
cart.lbi - 购物车
cart_view.lbi - 查看购物车
category_tree.lbi - 商品分类树
cat_goods.lbi - 分类下的商品
comments.lbi - 用户评论
comment_form.lbi - 发表评论的表单
consignee.lbi - 收货人信息
fittings.lbi - 相关配件
footer.lbi - 页脚
gallery.lbi - 商品相册
goods_detail.lbi - 商品详情
goods_info.lbi - 商品基本信息
goods_list.lbi - 商品列表
help.lbi - 帮助内容
history.lbi - 历史记录
hot_goods.lbi - 热卖商品
invoice_query.lbi - 发货单查询
member.lbi - 会员登录区
member_info.lbi - 会员信息
nav_main.lbi - 主导航
new_goods.lbi - 新品上架
order_confirm.lbi - 订单确认
order_detail.lbi - 订单详情
order_view.lbi - 订单信息
package_card.lbi - 包装和贺卡
pages.lbi - 列表分页
page_top.lbi - 页面顶部
payment.lbi - 支付方式
promotion.lbi - 促销商品
properties.lbi - 商品属性
register_login.lbi - 购物流程登录和注册
related_goods.lbi - 相关商品
search_advanced.lbi - 高级搜索表单
search_form.lbi - 搜索表单
search_result.lbi - 搜索结果
shipping.lbi - 配送方式
signin.lbi - 会员登录表单
snatch_bid.lbi - 夺宝奇兵出价表单
snatch_goods.lbi - 夺宝奇兵活动的商品
snatch_list.lbi - 夺宝奇兵活动列表
snatch_price.lbi - 夺宝奇兵价格列表
snatch_result.lbi - 夺宝奇兵活动结果
top10.lbi - 销售排行
ur_here.lbi - 当前位置
user_address.lbi - 会员中心收货人列表
user_address_add.lbi - 会员中心添加收货人
user_booking.lbi - 会员中心用户缺货登记
user_booking_add.lbi - 会员中心用户添加缺货登记
user_collect.lbi - 会员中心用户收藏夹
user_forgetpassword.lbi - 会员中心找回密码

<strong>PHP处理页的说明</strong>
以下为引用的内容：
\affiche.php:  广告处理文件
\ajax.php: 
\article.php:  文章内容
\article_cat.php:  文章分类
\category.php:  商品分类
\compare.php:  商品比较程序
\feed.php:  RSS Feed 生成程序
\flow.php:  购物流程
\gallery.php:  商品相册
\goods.php:  商品详情
\index.php:  首页文件
\receive.php:  处理收回确认的页面
\respond.php:  支付响应页面
\search.php:  搜索程序
\snatch.php: 
\user.php:  会员中心
\admin\admin_logs.php:  记录管理日志文件
\admin\ads.php:  广告管理程序
\admin\ad_position.php:  广告位置管理程序
\admin\area_manage.php:  地区列表管理文件
\admin\article.php: 
\admin\articlecat.php: 
\admin\attribute.php:  属性规格管理
\admin\bonus.php:  红包的处理文件
\admin\bonus_type.php:  红包类型的处理
\admin\brand.php:  品牌管理
\admin\card.php:  贺卡管理程序
\admin\category.php:  商品分类管理程序
\admin\comment_manage.php:  用户评论管理文件
\admin\convert.php:  转换程序
\admin\database.php: 
\admin\flow_stats.php:  流量统计
\admin\friend_link.php:  友情链接管理
\admin\get_password.php:  管理员新密码
\admin\gift.php:  管理中心赠品管理
\admin\goods.php:  商品管理程序
\admin\goods_booking.php:  缺货处理管理程序
\admin\goods_type.php:  商品类型管理程序
\admin\guest_stats.php:  客户统计
\admin\help.php:  管理中心帮助信息
\admin\index.php:  控制台首页
\admin\integrate.php:  第三方程序会员数据整合插件管理程序
\admin\mail_template.php:  管理中心模版管理程序
\admin\message.php: 
\admin\order.php:  订单管理
\admin\order_stats.php:  订单统计
\admin\pack.php:  包装管理程序
\admin\payment.php:  支付方式管理程序
\admin\picture_batch.php:  图片批量处理程序
\admin\privilege.php:  管理员信息以及权限管理
\admin\repay.php: 
\admin\sale_general.php:  销售概况
\admin\sale_list.php:  销售明细列表文件
\admin\sale_order.php:  商品销售排行
\admin\shipping.php:  配送方式管理程序
\admin\shipping_area.php:  配送区域管理程序
\admin\shophelp.php: 
\admin\shopinfo.php: 
\admin\shop_config.php:  管理中心商店设置
\admin\sitemap.php:  站点地图生成程序
\admin\snatch.php: 
\admin\sql.php:  会员管理程序
\admin\template.php:  管理中心模版管理程序
\admin\users.php:  会员管理程序
\admin\users_order.php:  会员排行统计文件
\admin\user_msg.php:  客户留言
\admin\user_rank.php:  会员等级管理程序
\admin\visit_sold.php:  访问购买比例
\admin\vote.php:   调查管理程序
\admin\includes\cls_exchange.php: 
\admin\includes\cls_google_sitemap.php:  Google sitemap 类
\admin\includes\cls_phpzip.php:  ZIP 处理类
\admin\includes\init.php:  管理中心公用文件
\admin\includes\lib_ajax.php:  管理中心用于Ajax的类库
\admin\includes\lib_image.php:  管理中心图片处理函数库
\admin\includes\lib_main.php:  管理中心公用函数库
\admin\includes\lib_report.php:  报表统计函数文件
\admin\includes\lib_template.php:  管理中心模版相关公用函数库
\admin\js\editzone.js(2):  编辑区脚本类
\admin\js\listzone.js(2):  列表脚本类
\admin\js\region.js(2):  公用脚本函数库
\admin\js\selectzone.js(2):  select脚本类
\admin\js\utils.js(2):  公用脚本函数库
\admin\js\validator.js(2):  表单验证类
\includes\cls_captcha.php:  验证码图片类
\includes\cls_ecshop.php:  基础类
\includes\cls_ecshop.php(56):      密码编译方法;
\includes\cls_rss.php:  RSS 类
\includes\cls_smtp.php:  SMTP 邮件类
\includes\inc_constant.php:  常量
\includes\init.php:  前台公用文件
\includes\lib_common.php:  公用函数库
\includes\lib_goodscat.php:  前台公用函数库
\includes\lib_insert.php:  动态内容函数库
\includes\lib_main.php:  前台公用函数库
\includes\lib_payment.php:  支付接口函数库
\includes\iconv\cls_iconv.php:  字符集转换类
\includes\ip\cls_ip.php:  IP 归属地查询类
\includes\modules\integrates\discuz.php:  会员数据处理类
\includes\modules\integrates\ecshop.php:  会员数据处理类
\includes\modules\integrates\molyx.php:  会员数据处理类(MolyX)
\includes\modules\integrates\phpwind.php:  会员数据处理类
\includes\modules\integrates\vbb.php:  会员数据处理类(VBB)
\includes\modules\payment\alipay.php:  支付宝插件
\includes\modules\payment\bank.php:  银行汇款（转帐）插件
\includes\modules\payment\chinabank.php:  快钱插件
\includes\modules\payment\cod.php:  货到付款插件
\includes\modules\payment\kuaiqian.php:  快钱插件
\includes\modules\payment\paypalcn.php:  贝宝插件
\includes\modules\payment\post.php:  邮局汇款插件
\includes\modules\shipping\cac.php:  上门取货插件
\includes\modules\shipping\ems.php:  EMS插件
\includes\modules\shipping\express.php:  城际快递插件
\includes\modules\shipping\flat.php:  邮政包裹插件
\includes\modules\shipping\post_express.php:  邮政包裹插件
\includes\modules\shipping\post_mail.php:  邮局平邮插件
\includes\modules\shipping\sf_express.php:  顺丰速运 配送方式插件
\includes\modules\shipping\sto_express.php:  申通快递 配送方式插件