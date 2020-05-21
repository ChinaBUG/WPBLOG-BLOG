---
ID: 2274
post_title: Discuz-系统预定义CSS样式
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/discuz-system-predefined-css-style.html
published: true
post_date: 2013-01-06 13:32:50
---
在弄模板主题的时候DIY模板时需要用到一些样式，怎么知道系统有不需要自己重新定义呢？翻翻template/default/common/common.css文件就会发现好多的定义噢，难道我都要摘抄？肯定不是啦，这边是我整理的，在DIY主题时可能用到的一些样式定义，用做笔记哈~

/*
Name: mod_reset
Level: Global
Explain: 重定义浏览器默认样式
Last Modify: Pony
*/
body, ul, ol, li, dl, dd, p, h1, h2, h3, h4, h5, h6, form, fieldset, .pr, .pc { margin: 0; padding: 0; }
ul li, .xl li { list-style: none; }
label { cursor: pointer; }
/*
Name: mod_float
Level: Global
Sample: class="z/y"
Explain: .z/.y 浮动 left/right
Last Modify: lushnis
*/
.z { float: left; }
.y { float: right; }

/*
Name: mod_clearfix
Level: Global
Sample: class="cl"
Explain: Clearfix,避免因子元素浮动而导致的父元素高度缺失能问题
Last Modify: lushnis
*/
.cl:after { content: "."; display: block; height: 0; clear: both; visibility: hidden; }
.cl { zoom: 1; }

/*
元素获取焦点时隐藏外边框
*/
.hidefocus { outline: none; }

/*
Name: mod_hr
Level: Global
Sample:
&lt;hr /&gt;
Explain: 重定义
&lt;hr /&gt;
元素的样式，去除默认边距
Last Modify: lushnis
*/
.mn hr, .sd hr { margin: 0 10px; }
/*
Name: mod_hr_solid
Level: Global
Dependent: mod_hr
Sample:
&lt;hr class="l" /&gt;
Explain: 定义 1px 高度实线样式的
&lt;hr /&gt;
元素，具有两个个扩展样式，.l2 和 .l3，分别实现 2px 和 3px 的实线分割线
Last Modify: lushnis
*/
hr.l { height: 1px; border: none; background: {COMMONBORDER}; color: {COMMONBORDER}; }
hr.l2 { height: 2px; }
hr.l3 { height: 3px; }
/*
Name: mod_hr_dashed
Level: Global
Dependent: mod_hr
Sample:
&lt;hr class="da" /&gt;
Explain: 定义 1px 高度虚线样式的
&lt;hr /&gt;
元素
Last Modify: lushnis
*/
hr.da { height: 0; border: none; border-top: 1px dashed {COMMONBORDER}; background: transparent; color: transparent; }

/* [!]使用注意 */
hr.bk { margin-bottom: 10px !important; *margin-bottom: 2px !important; height: 0; border: none; border-top: 1px solid {WRAPBG}; background: transparent; color: transparent; }
.n .sd hr.bk { border-top-color: #F9F9F9; }
/* 清除Margin */
hr.m0 { margin-left: 0; margin-right: 0; }

/*
Name: mod_page_header
Level: Global
Sample:
&lt;h1 class="ph"&gt;Text&lt;/h1&gt;
Explain: 页面中标题级别的文字 [!]此处须整合为一个单独 class
Last Modify: lushnis
*/
/* .wx --&gt; weight text 粗体字，通常用于大标题 */
.wx, .ph { font-family: 'Microsoft YaHei', 'Hiragino Sans GB', 'STHeiti', Tahoma, 'SimHei', sans-serif; font-weight: 100; }
/* Page header */ .ph { font-size: 20px; }
/* Main title */ .mt { padding: 10px 0; font-size: 16px; }

/* 行内分割竖线 */ .pipe { margin: 0 5px; color: #CCC; }

/* 文本属性：字号、颜色、粗细 */
/*
Name: mod_text_size
Level: Global
Sample: class="xs*"
Explain: 文字字号，分为四个级别
Last Modify: lushnis
*/
.xs0 { font-family: {SMFONT}; font-size: {SMFONTSIZE}; -webkit-text-size-adjust: none; }
.xs1 { font-size: 12px !important; }
.xs2 { font-size: 14px !important; }
.xs3 { font-size: 16px !important; }
/*
Name: mod_text_gray_level
Level: Global
Dependent: -
Sample: class="xs[*]"
Explain: 文字字号，分为四个级别
Last Modify: lushnis
*/
.xg1, .xg1 a { color: {LIGHTTEXT} !important; }
.xg1 .xi2 { color: {HIGHLIGHTLINK} !important; }
.xg2 { color: {MIDTEXT}; }
/*
Name: mod_text_importance_level
Level: Global
Sample: class="xs[*]"
Explain: 文字提亮级别，分为两级，默认模板中，1为橙色，2为蓝色
Last Modify: lushnis
*/
.xi1, .onerror { color: {NOTICETEXT}; }
.xi2, .xi2 a, .xi3 a { color: {HIGHLIGHTLINK} ; }
/*
Name: mod_text_weight_level
Level: Global
Sample: class="xs[*]"
Explain: 文字字号，分为四个级别
Last Modify: lushnis
*/
.xw0 { font-weight: 400; }
.xw1 { font-weight: 700; }
/*
Name: mod_border
Level: Global
Dependent: -
Sample: class="bbda/bbs"
Explain: 边框样式，该模块仅作用于元素的下边框，分为虚线和实线两种，宽度均为 1px
Last Modify: lushnis
*/
.bbda { border-bottom: 1px dashed {COMMONBORDER}; }
.bbs { border-bottom: 1px solid {COMMONBORDER} !important; }
/*
Name: mod_border_reset
Level: Global
Sample: class="bw0/bw0_all"
Explain: 去除边框
Last Modify: lushnis
*/
.bw0 { border: none !important; }
.bw0_all, .bw0_all th, .bw0_all td { border: none !important; }
/*
Name: mod_background_reset
Level: Global
Sample: class="bg0_c/bg0_i/bg0_all"
Explain: 去除背景，bg0_c、bg0_i 和 bg0_all 分别为去除背景颜色、去除背景图片和去除所有背景元素
Last Modify: Pony
*/
.bg0_c { background-color: transparent !important; }
.bg0_i { background-image: none !important; }
.bg0_all { background: none !important; }

/*
Name: mod_notice_line
Level: Global
Sample:
<div class="ntc_l">Explain: 黄色背景的提示条，一般用在单行醒目提示，不可用于多行块级区域
Last Modify: lushnis
*/
.ntc_l { padding: 5px 10px; background: #FEFEE9; }
.ntc_l .d { width: 20px; height: 20px; background: url({IMGDIR}/op.png) no-repeat 0 0; line-height: 9999px; overflow: hidden; }
.ntc_l .d:hover { background-position: 0 -20px; }
/*
Name: mod_margin
Level: Global
Sample: class="mtn/mtm/mtw/..."
Explain: 外边距样式，作用于元素的上下外边距，上下各具有 n, m, w 三个级别
Last Modify: lushnis
*/
.mtn { margin-top: 5px !important; }
.mbn { margin-bottom: 5px !important; }
.mtm { margin-top: 10px !important; }
.mbm { margin-bottom: 10px !important; }
.mtw { margin-top: 20px !important; }
.mbw { margin-bottom: 20px !important; }
/*
Name: mod_padding
Level: Global
Sample: class="ptn/ptm/ptw/..."
Explain: 内边距样式，作用于元素的上下内边距，上下各具有 n, m, w 三个级别
Last Modify: lushnis
*/
.ptn { padding-top: 5px !important; }
.pbn { padding-bottom: 5px !important; }
.ptm { padding-top: 10px !important; }
.pbm { padding-bottom: 10px !important; }
.ptw { padding-top: 20px !important; }
.pbw { padding-bottom: 20px !important; }</div>
/* 弹出层 以下 class 都可以分开写，单独定义，以便个性化 */
/* 四条边、四个角的公用样式 */
.t_l, .t_c, .t_r, .m_l, .m_r, .b_l, .b_c, .b_r { overflow: hidden; {FLOATMASKBGCODE}; opacity: 0.2; filter: alpha(opacity=20); }
/* 四个角 */
.t_l, .t_r, .b_l, .b_r { width: 8px; height: 8px; }
/* 上下两条边 */
.t_c, .b_c { height: 8px; }
/* 左右两条边 */
.m_l, .m_r { width: 8px; }

.t_l { -moz-border-radius: 8px 0 0 0; -webkit-border-radius: 8px 0 0 0; border-radius: 8px 0 0 0; }
.t_r { -moz-border-radius: 0 8px 0 0; -webkit-border-radius: 0 8px 0 0; border-radius: 0 8px 0 0; }
.b_l { -moz-border-radius: 0 0 0 8px; -webkit-border-radius: 0 0 0 8px; border-radius: 0 0 0 8px; }
.b_r { -moz-border-radius: 0 0 8px 0; -webkit-border-radius: 0 0 8px 0; border-radius: 0 0 8px 0; }
.m_c { {FLOATBGCODE}; }

/* 弹出层内容区 by Pony */
.m_c .tb { margin: 0 0 10px; padding: 0 10px; }
.m_c .c { padding: 0 10px 10px; }
.m_c .o { padding: 8px 10px; height: 26px; text-align: right; border-top: 1px solid #CCC; background: {COMMONBG}; }
/* 分享时会用到 */
.m_c .el { width: 420px; }
.m_c .el li { padding: 0; border: none; }

/* .flb 弹出层header */
.flb { padding: 10px 10px 8px; height: 20px; line-height: 20px; }
.flb em { float: left; font-size: 14px; font-weight: 700; color: {HIGHLIGHTLINK}; }
.flb em a { text-decoration: none; }
.flb .needverify { float: left; margin-left: 8px; padding-left: 13px; width: 45px; height: 21px; line-height: 21px; background: url({IMGDIR}/re_unsolved.gif) no-repeat 0 0; font-size: 12px; color: {LIGHTTEXT}; font-weight: 400; }
.flb .onerror, .flb .onright { padding-left: 20px; height: auto; line-height: 140%; white-space: nowrap; font-size: 12px; font-weight: 400; }
.flb .onerror { background: url({IMGDIR}/check_error.gif) no-repeat 0 50%; }
.flb .onright { background: url({IMGDIR}/check_right.gif) no-repeat 0 50%; color: {MIDTEXT}; }

.flb span { float: right; color: {LIGHTTEXT}; }
.flb span a, .flb strong { float: left; text-decoration: none; margin-left: 8px; font-weight: 400; color: {LINK}; }
.flb span a:hover { color: {LIGHTTEXT}; }
.flbc { float: left; width: 20px; height: 20px; overflow: hidden; text-indent: -9999px; background: url({IMGDIR}/cls.gif) no-repeat 0 0; cursor: pointer; }
.flbc:hover { background-position: 0 -20px; }

.floatwrap { overflow: auto; overflow-x: hidden; margin-bottom: 10px; height: 280px; }

.f_c { }
.f_c li { list-style: none; }
.f_c hr.l { margin: 0; }
.f_c a { color: {HIGHLIGHTLINK}; }
.f_c .list { margin: 0 auto 10px; width: 570px; border-top: 3px solid {COMMONBORDER}; }
.f_c .list th, .f_c .list td { padding: 5px 2px; height: auto; border-bottom: 1px dashed {COMMONBORDER}; }
.f_c .list .btns th, .f_c .list .btns td { border-bottom: none; }
.f_c .th th, .f_c .th td { padding: 10px 0; }
.f_c .list th { background: none; }

/* 弹窗未开启时 nofloat */
.nfl { height: auto !important; height: 320px; min-height: 320px; }
.nfl .f_c { margin: 60px auto; padding: 20px; width: 580px; border: 3px solid {COMMONBG}; background: {WRAPBG}; }
.nfl .loginform { height: auto; }
.nfl .clause { width: auto; height: auto; }

/* 打招呼 by Pony */
.poke { margin-bottom: 10px; }
.poke li { float: left; margin: 0 1% 5px 0; width: 32%; height: 22px; }
.poke img { vertical-align: middle; }

/* 普通数据列表 datatable by michael */
.dt { border-top: 1px solid {COMMONBORDER}; width: 100%; }
.dt th { background: {COMMONBG}; }
.dt td, .dt th { padding: 7px 4px; border-bottom: 1px solid {COMMONBORDER}; }
.dt .c { width: 50px; }

/* 用来展示数据的表格 */
.tdat { width: 100%; border: 1px solid {COMMONBORDER}; }
.tdat th, .tdat td { padding: 4px 5px; border: 1px solid {COMMONBORDER}; }

/* pgs --&gt; pages &amp; postbutton 分页、发帖按钮, pgb --&gt;返回首页, nxt --&gt;下一页 */
.pgs {}
.pgs #newspecial, .pgs #newspecialtmp, .pgs #post_reply, .pgs #post_replytmp { float: left; margin-right: 5px; }
.pg { float: right; }
.pg, .pgb { line-height: 26px; }
.pg a, .pg strong, .pgb a, .pg label { float: left; display: inline; margin-left: 4px; padding: 0 8px; height: 26px; border: 1px solid; border-color: {SPECIALBORDER}; background-color: {WRAPBG}; background-repeat: no-repeat; color: {LINK}; overflow: hidden; text-decoration: none; }
.pg a.nxt, .pgb a { padding: 0 10px; }
.pg a:hover, .pgb a:hover { border-color: {HIGHLIGHTLINK}; color: {HIGHLIGHTLINK}; }
.pg a.nxt { padding-right: 25px; background-image: url({IMGDIR}/arw_r.gif); background-position: 90% 50%; }
.pg a.prev { background-image: url({IMGDIR}/arw_l.gif); background-position: 50% 50%; }
.pg strong { background-color: {SPECIALBG}; }
.pgb a { padding-left: 25px; background-image: url({IMGDIR}/arw_l.gif); background-position: 10px 50%; }
.pg label { cursor: text; }
.ie6 .pg label { padding-top: 3px; height: 23px; }
.pg label .px { padding: 0; width: 25px; height: 16px; line-height: 16px; }
#pgt .pg, #pgt .pgb { margin-top: 5px; }
/* 用于行动的按钮 button action */
.bac {margin: 0; padding: 0; width: 70px; height: 30px;line-height: 30px; color: {LINK}; overflow: hidden; text-decoration: none; background: url({IMGDIR}/pg_arw.png) no-repeat 0 0; text-align: center; text-indent: -7px; display: block;}

#psd .bn .mbn input, #postbox input { margin-right: 4px; }
#postbox .mbn, #psd .mbn { height: 1.6em; line-height: 1.6em; }

/* 用于积分奖励提示等弹出层提示 */
.popupcredit {}
.pc_l, .pc_c, .pc_inner, .pc_r { width: 29px; height: 56px; line-height: 56px; background: url({IMGDIR}/popupcredit_bg.gif) no-repeat 0 0; }
.pc_c { width: 200px; background-position: 0 -56px; background-repeat: repeat-x; }
.pc_inner { white-space: nowrap; text-align: center; width: auto; background-position: 50% -112px; }
.pc_inner i { margin-right: 10px; font-size: 12px; font-style: normal; color: {LIGHTLINK}; font-weight: 400; }
.pc_inner span { margin-right: 15px; color: #FFEA97; font-size: 14px; font-weight: 700; }
* html .pc_inner span { display: inline-block; }
.pc_inner span a { color: #FFEA97; text-decoration: underline; }
.pc_inner span em { color: {LIGHTLINK}; font-size: 18px; font-weight: 400; }
.pc_inner span u { font-size: 10px; text-decoration: none; }
.pc_inner span em.desc { color: #930; }
.pc_btn img { opacity: 0.5; }
.pc_btn:hover img { opacity: 1; }
.pc_r { background-position: -30px 0; }

/*
Name: mod_tip
Level: Global
Explain: 弹出的气泡信息，1、2、3、4 分别指气泡尖角从左上到左小顺时针方向的位置
Last Modify: lushnis
*/
.tip { position: absolute; padding: 10px; width: 260px; border: 1px solid #B1B1B1; background: #FEFEE9; }
.tip_1, .tip_2 { margin-top: 8px; }
.tip_3, .tip_4 { margin-top: -8px; }
.tip_horn { position: absolute; width: 11px; height: 6px; overflow: hidden; }
.tip_1 .tip_horn { left: 5px; top: -6px; background: url({IMGDIR}/tip_top.png); }
.tip_2 .tip_horn { right: 5px; top: -6px; background: url({IMGDIR}/tip_top.png); }
.tip_3 .tip_horn { right: 5px; bottom: -6px; background: url({IMGDIR}/tip_bottom.png); }
.tip_4 .tip_horn { left: 5px; bottom: -6px; background: url({IMGDIR}/tip_bottom.png); }
.tip_js .tip_horn { right: 61px; bottom: -6px; background: url({IMGDIR}/tip_bottom.png); }
.aimg_tip { margin-top: 0; }