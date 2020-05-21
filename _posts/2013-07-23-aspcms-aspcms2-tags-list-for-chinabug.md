---
ID: 2514
post_title: ASP-AspCms2标签大全ChinaBUG整理
author: ChinaBUG
post_excerpt: |
  一、网站通用标签
  二、常用内容调用签
  三、导航菜单
  四、多图片的处理
  五、IF标签使用
layout: post
permalink: >
  http://blog.ipodmp.com/2013/07/aspcms-aspcms2-tags-list-for-chinabug.html
published: true
post_date: 2013-07-23 23:28:55
---
序

这一年多来，都在使用CMS系统建站，也少许的做了一点研究，这边是常用的一些内容及笔记，有些备注是自己在项目需要时做的研究及修改，当然，更多的功能增强是没办法一一写出来的，比如：网上死活没办法注册并查看的图片批量上传的功能也在客户的威逼之下自己处理了，还有分类封面功能等~算算还算修改的挺多内容的哈。

已完成的客户网站，有空的话可以去点点，互相交流一下，有什么建议请直接联系我，谢谢。

一、网站通用标签

{aspcms:sitepath}        网站终极目录(可放在二级目录,其它语言则在三级目录)
{aspcms:languagepath}        语言目录(后台多语言设置中设置)
{aspcms:siteurl}        网站地址
{aspcms:sitelogo}        LOGO地址
{aspcms:sitetitle}        网站标题
{aspcms:additiontitle}        网站附加标题
{aspcms:sitekeywords}        网站关键词
{aspcms:sitedesc}        网站描述
{aspcms:defaulttemplate}    默认模板
{aspcms:copyright}        网站版权

{aspcms:companyname}        公司名称
{aspcms:companyaddress}        公司地址
{aspcms:companypostcode}    邮政编码
{aspcms:companycontact}        联系人
{aspcms:companyphone}        电话号码
{aspcms:companymobile}        手机号码
{aspcms:companyfax}        公司传真
{aspcms:companyemail}        电子邮箱
{aspcms:companyicp}        备案号

{label:****}            自定义标签
{aspcms:userright}        用户对权限的读取，0为超级管理员，1为注册用户，2为游民

{aspcms:statisticalcode}    统计代码
{aspcms:onlineservice}        在线客服
{aspcms:kf}            其它客服系统
{aspcms:floatad}        漂浮广告
{aspcms:coupletad}        对联广告
{aspcms:windowad}        弹出广告
{aspcms:slide},{aspcms:slidea}    图片幻灯{aspcms:slideb},{aspcms:slidec},{aspcms:slided}

-源码预览-
MainClass.asp line 359

二、常用内容调用签
=======所有页面都可调用=======
首页内容列表调用标签(无分页)
{aspcms:content sort=2 num=4 order=order star=1}
[content:id]编号
[content:title]标题
[content:content]内容
[content:downurl]下载地址
[content:link]链接
[content:i]计数
[content:titlecolor]标题颜色
[content:sortname]分类名称
[content:sortlink]分类链接
[content:date]日期
[content:visits]浏览次数
[content:author]作者
[content:source]来源
[content:tag]tag标签
[content:istop]置顶
[content:isrecommend]推荐
[content:isimage]图片新闻
[content:isfeatured]特别推荐
[content:isheadline]头条
[content:desc]描述
[content:pic]图片
[content:indexvideo]首页视频
[content:videourl] 内页视频
{/aspcms:content}

sort 分类ID
num 显示数量(默认10条)
order 排序规则
star 星级(1,2,3,4,5)

-源码预览-
MainClass.asp line 847
lnum = labelArr("num")
ltype = labelArr("type")
lsort = labelArr("sort")
lorder = labelArr("order")
case "id" : orderStr =" order by ContentID desc"
case "visits" : orderStr =" order by Visits desc"
case "time" : orderStr =" order by a.AddTime desc"
case "order" : orderStr =" order by IsTop desc,isrecommend desc,ContentOrder,a.AddTime desc"
case "istop" : orderStr =" and IsTop order by ContentOrder,a.AddTime desc"
case "isrecommend" : orderStr =" and isrecommend order by ContentOrder,a.AddTime desc"
case "isimagenews" : orderStr =" and IsImageNews order by ContentOrder,a.AddTime desc"
case "isheadline" : orderStr =" and IsHeadline order by ContentOrder,a.AddTime desc"
case "isfeatured" : orderStr =" and IsFeatured order by ContentOrder,a.AddTime desc"
ltime = labelArr("time")
case "day" : whereTime=" and  DateDiff('d',a.AddTime,'"&amp;now()&amp;"')=0"
case "week" : whereTime=" and  DateDiff('w',a.AddTime,'"&amp;now()&amp;"')=0"
case "month" : whereTime=" and  DateDiff('m',a.AddTime,'"&amp;now()&amp;"')=0"
aboutkey = labelArr("tag")
lstar=labelArr("star")

=======列表页可调用=======
列表页调用标签
{aspcms:sorttitle}标题                 在添加分类时设置
{aspcms:sortkeyword}关键词             在添加分类时设置
{aspcms:sortdesc}描述                     在添加分类时设置
//
{aspcms:sortname}
{aspcms:parentsortid
{aspcms:sortid}
{aspcms:topsortid}
{aspcms:sortkeyword}
{aspcms:sortdesc}
{aspcms:sorttitle}
{aspcms:SortContent}
{aspcms:indeximage}
//
{aspcms:list size=10 order=id}
[list:id] 编号
[list:link]链接
[list:i]计数
[list:content]内容
[list:downurl]下载地址
[list:titlecolor]标题颜色
[list:sortname]分类名称
[list:sortlink]分类链接
[list:date]日期
[list:visits]浏览次数
[list:author]作者
[list:source]来源
[list:tag]tag标签
[list:istop]置顶
[list:isrecommend]推荐
[list:isimage]图片新闻
[list:isfeatured]特别推荐
[list:isheadline]头条
[list:pic]图片
[list:自定义字段名]
{/aspcms:list}

[list:pagenumber len=5]               分页条调用标签

当前位置
首页{aspcms:position} &gt;&gt;[position:link]{/aspcms:position}
首页 &gt;&gt;新闻发布 &gt;&gt;公司新闻

=======单页可调用=======
默认模板about.html
单页调用标签
[about:title]标题                在添加分类时设置
[about:keyword]关键词            在添加分类时设置
[about:desc]描述                 在添加分类时设置
[about:info]详细内容             在添加分类时设置
[about:pic]图片             在添加分类时设置

{aspcms:sortname}分类名称
{aspcms:parentsortid}上级分类ID
{aspcms:sortid}分类ID
{aspcms:topsortid}顶级分类ID

当前位置
首页{aspcms:position} &gt;&gt;[position:link]{/aspcms:position}
首页 &gt;&gt;新闻发布 &gt;&gt;公司新闻

内容分页  在编辑内容时插入到内容中即可
{aspcms:page}

=======详细可调用=========
由于详细页content标签和{aspcms:content}标签有冲突，所以原[content:]标签改为[news:]

详细页调用标签
[news:title]标题
[news:keyword]关键词             在添加内容时可以设置
[news:desc]描述                  在添加内容时可以设置
[news:link]链接
[news:info]内容
[news:i]计数
[news:titlecolor]标题颜色
[news:sortname]分类名称
[news:sortlink]分类链接
[news:date]日期
[news:visits]浏览次数
[news:author]作者
[news:source]来源
[news:downurl]下载地址
[news:istop]置顶
[news:isrecommend]推荐
[news:isimage]图片新闻
[news:isfeatured]特别推荐
[news:isheadline]头条
[news:pic]图片
[news:videourl]视频
[news:pics]多图
[news:tag]tag标签
[news:linktag]带链接的tag标签
{aspcms:comment}              评论标签

全站评论标签
{aspcms:comment}
[comment:i]评论计数
[comment:name]评论人
[comment:link]评论文章地址
[comment:ip]评论人IP
[comment:info len=5]评论内容
[comment:date style=yy-m-d]评论时间
{/aspcms:comment}

内容分页  在编辑内容时插入到内容中即可
{aspcms:page}

{aspcms:prev}上一篇
{aspcms:next}下一篇
{aspcms:nexttitle} 下一篇的题目
{aspcms:prevtitle} 上一篇的题目
{aspcms:nextlink}下一篇的链接
{aspcms:prevlink}上一篇的链接

当前位置
首页{aspcms:position} &gt;&gt;[position:link]{/aspcms:position}
首页 &gt;&gt;新闻发布 &gt;&gt;公司新闻

评论功能调用  {aspcms:comment}

三、导航菜单

===无限级菜单===
{aspcms:navlist}
&lt;li&gt;&lt;a href="[navlist:link]"&gt;[navlist:name]&lt;/a&gt;&lt;/li&gt;
{aspcms:1navlist type=[navlist:sortid]}
&lt;a href="[1navlist:link]"&gt;[1navlist:name]&lt;/a&gt;
{aspcms:2navlist type=[1navlist:sortid]}
&lt;a href="[2navlist:link]"&gt;[2navlist:name]&lt;/a&gt;
{aspcms:3navlist type=[2navlist:sortid]}
&lt;a href="[3navlist:link]"&gt;[3navlist:name]&lt;/a&gt;
{/aspcms:3navlist}
{/aspcms:2navlist}
{/aspcms:1navlist}
{/aspcms:navlist}
--两级菜单--
{aspcms:navlist}
&lt;li id="menu-item-[navlist:sortid]"&gt;
&lt;a href="[navlist:link]"&gt;[navlist:name]&lt;/a&gt;
{if:[navlist:subcount]&gt;0}
&lt;ul&gt;
{aspcms:1navlist type=[navlist:sortid]}
&lt;li id="menu-item-[1navlist:sortid]"&gt;&lt;a href="[1navlist:link]"&gt;[1navlist:name]&lt;/a&gt;&lt;/li&gt;
{/aspcms:1navlist}
&lt;/ul&gt;
{end if}
&lt;/li&gt;
{if:[navlist:sortid]&lt;&gt;107}
&lt;li&gt;&lt;/li&gt;
{end if}
{/aspcms:navlist}

[navlist:pic] 调用栏目图片
-源码预览-
MainClass.asp line 475
name,link,sortid,subcount,desc,pic,i,cursortid,pic

四、多图片的处理

{aspcms:cimages count=5 contentid=1}
[cimages:src]
[cimages:i]
{/aspcms:cimages}
count 输出数量，小于1或者不为数字的作为0处理，不输出
contentid 输出的内容id，一般在内容页调用，只需填写 contentid=[content:id]即可,错误的输入被赋值为-1
[cimages:src] 将被替换为/upLoad/product/month_1107/201107011448421775.png等数据循环输出

{aspcms:cimages contentid=[news:id]}
&lt;li&gt;&lt;a href="[cimages:src]"&gt;&lt;img src="[cimages:src]" width="50" height="50" desc=""&gt;&lt;/a&gt;&lt;/li&gt;
{/aspcms:cimages}

五、IF标签使用
1、满足条件则显示
{if:条件语句}
显示内容
{end if}

2、满足条件则显示内容1,否则显示内容2
{if:条件语句}
显示内容1
{else}
显示内容2
{end if}

实例:
1,给满足条件的添加不同的样式
{aspcms:content}
&lt;a {if:[content:i]=1} style="..."{end if} link="[content:link]"&gt;[content:title]&lt;/a&gt;
{/aspcms:content}
实例：
取余
{if:[list:i] MOD 4 = 0}...{end if}

其他内容请查阅官方文档。

PS：

早期也想自己写一个自己的cms系统这样也省的自己每次要重复的写代码，忙的团团转，结果，学艺不精哈~一直不能成行，耽误多年，老了，才发现有人已经写出来了，虽然aspcms不算很好，不过试用了这么多个的asp cms系统后还是觉得aspcms比较方便试用，满足一般企业网站的使用，虽然性能有点差。

另外，对于官方，注册也要付费来说，觉得有点那个了，在交流方面不能太固步自封。你可以收费但是这种费用收了也没什么意思，不是嘛~！