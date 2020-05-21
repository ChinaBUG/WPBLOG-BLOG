---
ID: 2280
post_title: >
  Discuz-Discuz-X模板教程$_G变量注解
author: ChinaBUG
post_excerpt: |
  G变量的使用方法：
  直接复制下面的变量放到模板需要的位置即可！
  例如：$_G['style'][boardlogo] 刷新后就会 显示一张logo
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/discuz-x-template-tutorial-_g-variable-comment.html
published: true
post_date: 2013-01-06 16:15:09
---
G变量的使用方法：
直接复制下面的变量放到模板需要的位置即可！
例如：$_G['style'][boardlogo] 刷新后就会 显示一张logo

$_G['uid'] =&gt; 当前登录UID

$_G['username'] =&gt; 当前登录用户名

$_G['adminid'] =&gt; 当前登录ID管理组ID

$_G['groupid'] =&gt; 当前登录ID用户组ID

$_G['cookie'] =&gt; 客户端cookie

$_G['formhash'] =&gt; 当前登录ID的【FORMHASH】 主要用于表单提交

$_G['timestamp'] =&gt; 当前活动时间

$_G['starttime'] =&gt; 1317042440.3242

$_G['clientip'] =&gt; 当前访问者IP地址

$_G['referer'] =&gt; 当前请求的地址，主要用户表单提交

$_G['charset'] =&gt; 程序编码

$_G['PHP_SELF'] =&gt; 当前访问页面的相对地址

$_G['siteurl'] =&gt; 程序访问地址

$_G['siteroot'] =&gt; 程序所在域名的相对目录

$_G['fid'] =&gt; 当前版块id【主题列表页、帖子页】出现

$_G['tid'] =&gt; 当前帖子ID【帖子页】出现

$_G['basescript'] =&gt; 当前页面所在频道

$_G['basefilename'] =&gt; 当前页面php文件名

$_G['staticurl'] =&gt; 程序附件目录

$_G['mod'] =&gt; 当前页面的MOD值【例如：forum.php?mod=xxx】

$_G['inajax'] =&gt; 当前ajax请求的值【无-0 有-1】

$_G['page'] =&gt; 当前分页ID

$_G['tpp'] =&gt; 当前分页每页显示数量

$_G['seokeywords'] =&gt; 当前页面seo关键词

$_G['seodescription'] =&gt; 当前页面seo介绍

$_G['timenow'] =&gt; Array
(
[time] =&gt; 2011-9-26 21:07 当前服务器时间
[offset] =&gt; +8 当前服务器时区
)

$_G['config'] =&gt; Array
(
$_G['config'][db] =&gt; Array
(
$_G['config'][db][1] =&gt; Array
(
$_G['config'][db][1][dbhost] =&gt; localhost 数据库连接地址
$_G['config'][db][1][dbuser] =&gt; root 数据库用户名
$_G['config'][db][1][dbpw] =&gt; 123456 数据库密码
$_G['config'][db][1][dbcharset] =&gt; utf8 数据库编码
$_G['config'][db][1][pconnect] =&gt; 0
$_G['config'][db][1][dbname] =&gt; dxutf 数据库名
$_G['config'][db][1][tablepre] =&gt; pre_ 数据表前缀
)
)
)

原文摘自：<a id="thread_subject" href="http://www.discuz.net/thread-2395689-1-1.html">Discuz-X模板教程 G变量注解之 $_G['xxx'] 全局基本变量 By：cr180</a>

$_G['setting'][sitename] =&gt; 全局-站点信息-网站名称

$_G['setting'][siteurl] =&gt; 全局-站点信息-网站URL

$_G['setting'][regname] =&gt; 全局-注册访问-注册-注册地址

$_G['setting'][reglinkname] =&gt; 全局-注册访问-注册-注册链接文字

$_G['setting'][regverify] =&gt; 全局-注册访问-注册-新用户注册验证

$_G['setting'][icp] =&gt; 全局-站点信息-网站备案信息代码

$_G['setting'][imagelib] =&gt; 全局-上传设置-基本设置-图片处理库类型

$_G['setting'][extcredits] =&gt; 积分情况 自行打印

$_G['setting'][creditsformula] =&gt; 全局-积分设置-基本设置-总积分计算公式

$_G['setting'][cacheindexlife] =&gt; 全局-性能优化-论坛页面缓存设置-缓存论坛首页有效期

$_G['setting'][cachethreaddir] =&gt; 全局-性能优化-论坛页面缓存设置-缓存目录

$_G['setting'][cachethreadlife] =&gt; 全局-性能优化-论坛页面缓存设置-缓存帖子有效期

$_G['setting'][bbrulestxt] =&gt; 全局-注册访问-注册-网站服务条款

$_G['setting'][bbname] =&gt; 全局-站点信息-站点名称

$_G['setting'][attachurl] =&gt; 全局-上传设置-基本设置-本地附件URL地址

$_G['setting'][attachdir] =&gt; 全局-上传设置-基本设置-本地附件保存位置

$_G['setting'][anonymoustext] =&gt; 界面-界面设置-全局-匿名用户的昵称

$_G['setting'][threadsticky] =&gt; 界面-界面设置-主题列表-置顶主题的标识

$_G['setting'][defaultindex] =&gt; 默认首页文件名forum.php

$_G['setting'][verify] =&gt; 用户-认证设置

$_G['setting'][rewriterule] =&gt; 后台伪静态规则情况

$_G['setting'][ucenterurl] =&gt; UCenter地址

$_G['setting'][plugins] =&gt; 后台插件设置与启用情况

$_G['setting'][navlogos] =&gt; 后台界面设置-导航设置-内置导航的logo组

$_G['setting'][navmn] =&gt; 后台设置的导航情况，主要用于导航判断

$_G['setting'][navs] =&gt; 页头导航数组，可参考此数组进行页头导航重写

$_G['setting'][footernavs] =&gt; 页尾导航

$_G['setting'][spacenavs] =&gt; 家园模块左侧导航

$_G['setting'][mynavs] =&gt; 页头导航右边快捷导航按钮内容

$_G['setting'][topnavs] =&gt; 页头顶部导航内容

$_G['setting'][forumpicstyle] =&gt; Array 版块主题封面
(
$_G['setting'][forumpicstyle][thumbwidth] =&gt; 主题封面宽度

$_G['setting'][forumpicstyle][thumbheight] =&gt; 主题封面高度
)

$_G['setting'][activityfield] =&gt; 全局-站点功能-活动主题-发起者必填信息

$_G['setting'][activityextnum] =&gt; 全局-站点功能-活动主题-扩展资料项数量

$_G['setting'][activitypp] =&gt; 全局-站点功能-活动主题-用户列表每页显示参与活动的人数

$_G['setting'][activitycredit] =&gt; 全局-站点功能-活动主题-使用积分

$_G['setting'][activitytype] =&gt; 全局-站点功能-活动主题-内置类型

$_G['setting'][adminemail] =&gt; 全局-站点信息-管理员邮箱

原文摘自：<a href="http://www.discuz.net/thread-2395695-1-1.html">Discuz-X模板教程 G变量注解之$_G['setting'] 全局后台各项设置 By：cr180</a>

$_G['member'] =&gt; Array 当前登录用户个人信息
(
$_G['member'][uid] =&gt; UID
$_G['member'][email] =&gt; 邮箱地址
$_G['member'][username] =&gt; 用户名
$_G['member'][password] =&gt; 经过MD5后的密码（别乱输出！！！切记）
$_G['member'][status] =&gt; 用户是否已经删除
$_G['member'][emailstatus] =&gt; 邮箱验证状态 0未验证 1验证通过
$_G['member'][avatarstatus] =&gt; 头像上传状态 0未上传 1已上传
$_G['member'][videophotostatus] =&gt; 视频认证 0未认证 1已认证
$_G['member'][adminid] =&gt; 所在管理组ID
$_G['member'][groupid] =&gt; 所在用户组ID
$_G['member'][groupexpiry] =&gt; 所在用户组有效期
$_G['member'][extgroupids] =&gt; 扩展用户组
$_G['member'][regdate] =&gt; 注册时间
$_G['member'][credits] =&gt; 214 现有总积分
$_G['member'][notifysound] =&gt; 短消息声音
$_G['member'][timeoffset] =&gt; 所在时区
$_G['member'][newpm] =&gt; 新短消息数量
$_G['member'][newprompt] =&gt; 新提醒数量
$_G['member'][accessmasks] =&gt; 这个貌似访问权限，不确定
$_G['member'][allowadmincp] =&gt; 是否拥有管理面板权限 0否 1是
$_G['member'][onlyacceptfriendpm] =&gt; 是否只接受好友短消息 0否 1是
$_G['member'][conisbind] =&gt; 是否绑定QQ 0否 1是
$_G['member'][lastvisit] =&gt; 上次访问时间
)

原文摘自：<a id="thread_subject" href="http://www.discuz.net/thread-2395688-1-1.html">Discuz-X模板教程 G变量注解之 $_G['member'] 全局当前登录者信息 By：cr180</a>

$_G['style'] =&gt; Array
(<span style="color: white;">官方模板区 cr180整理</span>
$_G['style'][styleid] =&gt; 当前风格ID
$_G['style'][name] =&gt; 当前风格名
$_G['style'][templateid] =&gt; 当前模板体系
$_G['style'][tpldir] =&gt; 当前模板目录
$_G['style'][menuhoverbgcolor] =&gt; 导航菜单高亮背景颜色
$_G['style'][lightlink] =&gt; 浅色链接颜色
$_G['style'][floatbgcolor] =&gt; 弹出窗口背景属性
$_G['style'][dropmenubgcolor] =&gt; 下拉菜单背景属性
$_G['style'][floatmaskbgcolor] =&gt; 弹出窗口边框颜色属性
$_G['style'][dropmenuborder] =&gt; 下拉菜单边框色
$_G['style'][specialbg] =&gt; 彩色区域背景色(帖子用户信息栏、需强调的表头等)
$_G['style'][specialborder] =&gt; 彩色区域边框
$_G['style'][commonbg] =&gt; 通用显示区域背景颜色
$_G['style'][commonborder] =&gt; 通用边框颜色
$_G['style'][inputbg] =&gt; 输入框背景色
$_G['style'][inputborderdarkcolor] =&gt; 输入框边框深色
$_G['style'][headerbgcolor] =&gt; 页头背景
$_G['style'][headerborder] =&gt; 页头分割线高度
$_G['style'][sidebgcolor] =&gt; 家园侧边背景
$_G['style'][msgfontsize] =&gt; 帖子内容字号
$_G['style'][bgcolor] =&gt; 页面背景
$_G['style'][noticetext] =&gt; 提示信息颜色
$_G['style'][highlightlink] =&gt; 高亮链接颜色
$_G['style'][link] =&gt; 链接文字颜色
$_G['style'][lighttext] =&gt; 浅色文字
$_G['style'][midtext] =&gt; 中等文本颜色
$_G['style'][tabletext] =&gt; 普通文本颜色
$_G['style'][smfontsize] =&gt; 小号字体大小
$_G['style'][threadtitlefont] =&gt; 主题列表字体
$_G['style'][threadtitlefontsize] =&gt; 主题列表字体大小
$_G['style'][smfont] =&gt; 小号字体
$_G['style'][titlebgcolor] =&gt; 版块列表标题字体颜色
$_G['style'][fontsize] =&gt; 正常字体大小
$_G['style'][font] =&gt; 正常字体
$_G['style'][styleimgdir] =&gt; 扩展图片目录
$_G['style'][imgdir] =&gt; 界面基础图片目录
$_G['style'][boardimg] =&gt; logo所在路径
$_G['style'][headertext] =&gt; 页头文字颜色
$_G['style'][footertext] =&gt; 页脚文字颜色
$_G['style'][menubgcolor] =&gt; 导航菜单背景颜色
$_G['style'][menutext] =&gt; 导航菜单文字颜色
$_G['style'][menuhovertext] =&gt; 导航菜单高亮文字颜色
$_G['style'][wrapbg] =&gt; 主体表格背景色
$_G['style'][wrapbordercolor] =&gt; 主体表格边框色
$_G['style'][contentwidth] =&gt; 阅读区域宽度
$_G['style'][contentseparate] =&gt; 帖子间隔颜色
$_G['style'][inputborder] =&gt; 输入框边框浅色
$_G['style'][menuhoverbgcode] =&gt; 导航菜单高亮背景
$_G['style'][floatbgcode] =&gt; 弹出窗口背景色
$_G['style'][dropmenubgcode] =&gt; 下拉菜单背景色
$_G['style'][floatmaskbgcode] =&gt; 弹出窗口边框颜色
$_G['style'][headerbgcode] =&gt; 页头背景
$_G['style'][sidebgcode] =&gt; 家园侧边栏背景属性
$_G['style'][bgcode] =&gt; 全局背景属性属性
$_G['style'][titlebgcode] =&gt; 版块列表标题背景
$_G['style'][menubgcode] =&gt; 导航菜单背景属性
$_G['style'][boardlogo] =&gt; LOGO img代码
)

原文摘自：<a id="thread_subject" href="http://www.discuz.net/thread-2395680-1-1.html">Discuz-X模板教程 G变量注解之 $_G['style'] 全局可用的风格变量 By：cr180</a>