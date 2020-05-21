---
ID: 3697
post_title: Android-控件属性大全
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/11/android-guinness-control-properties.html
published: true
post_date: 2014-11-17 09:34:49
---
<strong>控件属性：</strong>

<strong>android属性</strong>

Android功能强大，界面华丽，但是众多的布局属性就害苦了开发者，下面这篇文章结合了网上不少资料，

<strong>第一类:属性值为true或false</strong>
android:layout_centerHrizontal 水平居中 （Hrizontal表示水平）
android:layout_centerVertical 垂直居中 （Vertiacl表示垂直）
android:layout_centerInparent 相对于父元素完全居中
android:layout_alignParentBottom 贴紧父元素的下边缘 （align 表示使什么成为一行）
android:layout_alignParentLeft 贴紧父元素的左边缘
android:layout_alignParentRight 贴紧父元素的右边缘
android:layout_alignParentTop 贴紧父元素的上边缘
android:layout_alignWithParentIfMissing 如果对应的兄弟元素找不到的话就以父元素做参照物
<strong>第二类：属性值必须为id的引用名"@id/id-name"</strong>
android:layout_below 在某元素的下方
android:layout_above 在某元素的的上方
android:layout_toLeftOf 在某元素的左边
android:layout_toRightOf 在某元素的右边
android:layout_alignTop 本元素的上边缘和某元素的的上边缘对齐
android:layout_alignLeft 本元素的左边缘和某元素的的左边缘对齐
android:layout_alignBottom 本元素的下边缘和某元素的的下边缘对齐
android:layout_alignRight 本元素的右边缘和某元素的的右边缘对齐

<strong>第三类：属性值为具体的像素值，如30dip，40px</strong>
android:layout_marginBottom 离某元素底边缘的距离 margin英文是边缘的意思
android:layout_marginLeft 离某元素左边缘的距离
android:layout_marginRight 离某元素右边缘的距离
android:layout_marginTop 离某元素上边缘的距离

<strong>EditText的属性</strong>

android:hint 设置EditText为空时输入框内的提示信息。
android:gravity属性是对该view 内容的限定．比如一个button 上面的text. 你可以设置该text 在view的靠左，靠右等位置．以button为例，android:gravity="right"则button上面的文字靠右
android:layout_gravity
android:layout_gravity是用来设置该view相对与起父view 的位置．比如一个button 在linearlayout里，你想把该button放在靠左、靠右等位置就可以通过该属性设置．以button为 例，android:layout_gravity="right"则button靠右
android:scaleType：
android:scaleType是控制图片如何resized/moved来匹对ImageView的size。

ImageView.ScaleType / android:scaleType值的意义区别：
CENTER /center 按图片的原来size居中显示，当图片长/宽超过View的长/宽，则截取图片的居中部分显示
CENTER_CROP / centerCrop 按比例扩大图片的size居中显示，使得图片长(宽)等于或大于View的长(宽)
CENTER_INSIDE / centerInside 将图片的内容完整居中显示，通过按比例缩小或原来的size使得图片长/宽等于或小于View的长/宽
FIT_CENTER / fitCenter 把图片按比例扩大/缩小到View的宽度，居中显示
FIT_END / fitEnd 把图片按比例扩大/缩小到View的宽度，显示在View的下部分位置
FIT_START / fitStart 把图片按比例扩大/缩小到View的宽度，显示在View的上部分位置
FIT_XY / fitXY 把图片不按比例扩大/缩小到View的大小显示
MATRIX / matrix 用矩阵来绘制，动态缩小放大图片来显示。
** 要注意一点，Drawable文件夹里面的图片命名是不能大写的。

<strong>android:id
为控件指定相应的ID</strong>
android:text
指定控件当中显示的文字，需要注意的是，这里尽量使用strings.xml文件当中的字符串
android:gravity
指定View组件的对齐方式，比如说居中，居右等位置 这里指的是控件中的文本位置并不是控件本身
android:layout_gravity
指定Container组件的对齐方式．比如一个button 在linearlayout里，你想把该button放在靠左、靠右等位置就可以通过该属性设置．以button为 例，android:layout_gravity="right"则button靠右
android:textSize
指定控件当中字体的大小
android:background
指定该控件所使用的背景色，RGB命名法
android:width
指定控件的宽度 控件与组件
android:height
指定控件的高度
android:layout_width
指定Container组件的宽度
android:layout_height
指定Container组件的高度
android:layout_weight
View中很重要的属性，按比例划分空间
android:padding*
指定控件的内边距，也就是说控件当中的内容
android:sigleLine
如果设置为真的话，则控件的内容在同一行中进行显示
android:scaleType
是控制图片如何resized/moved来匹对ImageView的siz
android:layout_centerHrizontal
水平居中
android:layout_centerVertical
垂直居中
android:layout_centerInparent
相对于父元素完全居中
android:layout_alignParentBottom
贴紧父元素的下边缘
android:layout_alignParentLeft
贴紧父元素的左边缘
android:layout_alignParentRight
贴紧父元素的右边缘
android:layout_alignParentTop
贴紧父元素的上边缘
android:layout_alignWithParentIfMissing
如果对应的兄弟元素找不到的话就以父元素做参照物
android:layout_below
在某元素的下方
android:layout_above
在某元素的的上方
android:layout_toLeftOf
在某元素的左边
android:layout_toRightOf
在某元素的右边
android:layout_alignTop
本元素的上边缘和某元素的的上边缘对齐
android:layout_alignLeft
本元素的左边缘和某元素的的左边缘对齐
android:layout_alignBottom
本元素的下边缘和某元素的的下边缘对齐
android:layout_alignRight
本元素的右边缘和某元素的的右边缘对齐
android:layout_marginBottom
离某元素底边缘的距离
android:layout_marginLeft
离某元素左边缘的距离
android:layout_marginRight
离某元素右边缘的距离
android:layout_marginTop
离某元素上边缘的距离
android:paddingLeft
本元素内容离本元素右边缘的距离
android:paddingRight
本元素内容离本元素上边缘的距离
android:hint
设置EditText为空时输入框内的提示信息
android:LinearLayout
它确定了LinearLayout的方向，其值可以为vertical， 表示垂直布局horizontal， 表示水平布局
<strong>android:interpolator</strong>
可能有很多人不理解它的用法，文档里说的也不太清楚，其实很简单，看下面：interpolator定义一个动画的变化率（the rate of change）。这使得基本的动画效果(alpha, scale, translate, rotate）得以加速，减速，重复等。用通俗的一点的话理解就是：动画的进度使用 Interpolator 控制。interpolator 定义了动画的变化速度，可以实现匀速、正加速、负加速、无规则变加速等。Interpolator 是基类，封装了所有 Interpolator 的共同方法，它只有一个方法，即 getInterpolation (float input)，该方法 maps a point on the timeline to a multiplier to be applied to the transformations of an animation。Android 提供了几个 Interpolator 子类，实现了不同的速度曲线，如下：
AccelerateDecelerateInterpolator 在动画开始与介绍的地方速率改变比较慢，在中间的时侯加速
AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
DecelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始减速
LinearInterpolator 在动画的以均匀的速率改变
对于 LinearInterpolator ，变化率是个常数，即 f (x) = x.
public float getInterpolation(float input) {
return input;
}
Interpolator其他的几个子类，也都是按照特定的算法，实现了对变化率。还可以定义自己的 Interpolator 子类，实现抛物线、自由落体等物理效果。

TextView属性汇总

android:autoLink设置是否当文本为URL链接/email/电话号码/map时，文本显示为可点击的链接。可选值(none/web/email/phone/map/all)

android:autoText如果设置，将自动执行输入值的拼写纠正。此处无效果，在显示输入法并输入的时候起作用。

android:bufferType指定getText()方式取得的文本类别。选项editable 类似于StringBuilder可追加字符，也就是说getText后可调用append方法设置文本内容。spannable 则可在给定的字符区域使用样式，参见这里1、这里2。

android:capitalize设置英文字母大写类型。此处无效果，需要弹出输入法才能看得到，参见EditView此属性说明。

android:cursorVisible设定光标为显示/隐藏，默认显示。

android:digits设置允许输入哪些字符。如"1234567890.+-*/% ()"

android:drawableBottom在text的下方输出一个drawable，如图片。如果指定一个颜色的话会把text的背景设为该颜色，并且同时和background使用时覆盖后者。

android:drawableLeft在text的左边输出一个drawable，如图片。

android:drawablePadding设置text与drawable(图片)的间隔，与drawableLeft、 drawableRight、drawableTop、drawableBottom一起使用，可设置为负数，单独使用没有效果。

android:drawableRight在text的右边输出一个drawable。

android:drawableTop在text的正上方输出一个drawable。

android:editable设置是否可编辑。

android:editorExtras设置文本的额外的输入数据。

android:ellipsize设置当文字过长时,该控件该如何显示。有如下值设置："start"—-省略号显示在开头;"end" ——省略号显示在结尾;"middle"—-省略号显示在中间;"marquee" ——以跑马灯的方式显示(动画横向移动)

android:freezesText设置保存文本的内容以及光标的位置。

android:gravity设置文本位置，如设置成"center"，文本将居中显示。

android:hintText为空时显示的文字提示信息，可通过textColorHint设置提示信息的颜色。此属性在 EditView中使用，但是这里也可以用。

android:imeOptions附加功能，设置右下角IME动作与编辑框相关的动作，如actionDone右下角将显示一个"完成"，而不设置默认是一个回车符号。这个在EditView中再详细说明，此处无用。

android:imeActionId设置IME动作ID。

android:imeActionLabel设置IME动作标签。

android:includeFontPadding设置文本是否包含顶部和底部额外空白，默认为true。

android:inputMethod为文本指定输入法，需要完全限定名(完整的包名)。例如：com.google.android.inputmethod.pinyin，但是这里报错找不到。

android:inputType设置文本的类型，用于帮助输入法显示合适的键盘类型。在EditView中再详细说明，这里无效果。

android:linksClickable设置链接是否点击连接，即使设置了autoLink。

android:marqueeRepeatLimit在ellipsize指定marquee的情况下，设置重复滚动的次数，当设置为 marquee_forever时表示无限次。

android:ems设置TextView的宽度为N个字符的宽度。这里测试为一个汉字字符宽度

android:maxEms设置TextView的宽度为最长为N个字符的宽度。与ems同时使用时覆盖ems选项。

android:minEms设置TextView的宽度为最短为N个字符的宽度。与ems同时使用时覆盖ems选项。

android:maxLength限制显示的文本长度，超出部分不显示。

android:lines设置文本的行数，设置两行就显示两行，即使第二行没有数据。

android:maxLines设置文本的最大显示行数，与width或者layout_width结合使用，超出部分自动换行，超出行数将不显示。

android:minLines设置文本的最小行数，与lines类似。

android:lineSpacingExtra设置行间距。

android:lineSpacingMultiplier设置行间距的倍数。如"1.2"

android:numeric如果被设置，该TextView有一个数字输入法。此处无用，设置后唯一效果是TextView有点击效果，此属性在EdtiView将详细说明。

android:password以小点"."显示文本

android:phoneNumber设置为电话号码的输入方式。

android:privateImeOptions设置输入法选项，此处无用，在EditText将进一步讨论。

android:scrollHorizontally设置文本超出TextView的宽度的情况下，是否出现横拉条。

android:selectAllOnFocus如果文本是可选择的，让他获取焦点而不是将光标移动为文本的开始位置或者末尾位置。 TextView中设置后无效果。

android:shadowColor指定文本阴影的颜色，需要与shadowRadius一起使用。

android:shadowDx设置阴影横向坐标开始位置。

android:shadowDy设置阴影纵向坐标开始位置。

android:shadowRadius设置阴影的半径。设置为0.1就变成字体的颜色了，一般设置为3.0的效果比较好。

android:singleLine设置单行显示。如果和layout_width一起使用，当文本不能全部显示时，后面用"…"来表示。如android:text="test_ singleLine "

android:singleLine="true" android:layout_width="20dp"将只显示"t…"。如果不设置singleLine或者设置为false，文本将自动换行

android:text设置显示文本.

android:textAppearance设置文字外观。如 "?android:attr/textAppearanceLargeInverse"这里引用的是系统自带的一个外观，?表示系统是否有这种外观，否 则使用默认的外观。可textAppearanceButton/textAppearanceInverse/textAppearanceLarge /textAppearanceLargeInverse/textAppearanceMedium/textAppearanceMediumInverse/textAppearanceSmall/textAppearanceSmallInverse

android:textColor设置文本颜色

android:textColorHighlight被选中文字的底色，默认为蓝色

android:textColorHint设置提示信息文字的颜色，默认为灰色。与hint一起使用。

android:textColorLink文字链接的颜色.

android:textScaleX设置文字之间间隔，默认为1.0f。

android:textSize设置文字大小，推荐度量单位"sp"，如"15sp"

android:textStyle设置字形[bold(粗体) 0, italic(斜体) 1, bolditalic(又粗又斜) 2] 可以设置一个或多个，用"|"隔开

android:typeface设置文本字体，必须是以下常量值之一：normal 0, sans 1, serif 2, monospace(等宽字体) 3]

android:height设置文本区域的高度，支持度量单位：px(像素)/dp/sp/in/mm(毫米)

android:maxHeight设置文本区域的最大高度

android:minHeight设置文本区域的最小高度

android:width设置文本区域的宽度，支持度量单位：px(像素)/dp/sp/in/mm(毫米)，与layout_width 的区别看这里。

android:maxWidth设置文本区域的最大宽度

android:minWidth设置文本区域的最小宽度

<strong>Android activity属性汇总</strong>

android:allowTaskReparenting

是否允许activity更换从属的任务，比如从短信息任务切换到浏览器任务。

android:alwaysRetainTaskState

是否保留状态不变， 比如切换回home, 再从新打开， activity处于最后的状态

android:clearTaskOnLanunch

比如 P 是 activity, Q 是被P 触发的 activity, 然后返回Home, 从新启动 P，是否显示 Q

android:configChanges

当配置list发生修改时，是否调用 onConfigurationChanged() 方法 比如 "locale|navigation|orientation".

android:enabled

activity 是否可以被实例化,

android:excludeFromRecents

是否可被显示在最近打开的activity列表里

android:exported

是否允许activity被其它程序调用

android:finishOnTaskLaunch

是否关闭已打开的activity当用户重新启动这个任务的时候

android.icon

android:label

android:launchMode

activity启动方式， "standard" "singleTop" "singleTask" "singleInstance"

其中前两个为一组， 后两个为一组

android:multiprocess

允许多进程

android:name

activity的类名， 必须指定

androidnHistory

是否需要移除这个activity当用户切换到其他屏幕时。这个属性是 API level 3 中引入的

android:permission

android:process

一 个activity运行时所在的进程名，所有程序组件运行在应用程序默认的进程中，这个进程名跟应用程序的包名一致。中的元素process属性能够为所 有组件设定一个新的默认值。但是任何组件都可以覆盖这个默认值，允许你将你的程序放在多进程中运行。 如果这个属性被分配的名字以:开头，当这个activity运行时, 一个新的专属于这个程序的进程将会被创建。如果这个进程名以小写字母开头，这个activity将会运行在全局的进程中，被它的许可所提供。

android:screenOrientation

activity显示的模式, "unspecified" 默认值 "landscape" 风景画模式，宽度比高度大一些 "portrait" 肖像模式, 高度比宽度大。 "user" 用户的设置 "behind" "sensor" "nosensor"

<strong>android:stateNotNeeded</strong>

是否 activity被销毁和成功重启并不保存状态

android:taskAffinity

activity的亲属关系， 默认情况同一个应用程序下的activity有相同的关系

android:theme

activity的样式主题, 如果没有设置，则activity的主题样式从属于应用程序，参见元素的theme属性

<strong>android:windowSoftInputMode</strong>

activity主窗口与软键盘的交互模式, 自从API level 3 被引入

活动的主窗口如何与包含屏幕上的软键盘窗口交互。这个属性的设置将会影响两件事情:

1&gt; 软键盘的状态——是否它是隐藏或显示——当活动(Activity)成为用户关注的焦点。

2&gt; 活动的主窗口调整——是否减少活动主窗口大小以便腾出空间放软键盘或是否当活动窗口的部分被软键盘覆盖时它的内容的当前焦点是可见的。

它的设置必须是下面列表中的一个值，或一个"state…"值加一个"adjust…"值的组合。在任一组设置多个值——多 个"state…"values，例如＆mdash有未定义的结果。各个值之间用|分开。例如: &lt;activity android:windowSoftInputMode="stateVisible|adjustResize" . . . &gt;

在这设置的值(除"stateUnspecified"和"adjustUnspecified"以外)将覆盖在主题中设置的值

值 描述

"stateUnspecified" 软键盘的状态(是否它是隐藏或可见)没有被指定。系统将选择一个合适的状态或依赖于主题的设置。这个是为了软件盘行为默认的设置。

"stateUnchanged" 软键盘被保持无论它上次是什么状态，是否可见或隐藏，当主窗口出现在前面时。

"stateHidden" 当用户选择该Activity时，软键盘被隐藏——也就是，当用户确定导航到该Activity时，而不是返回到它由于离开另一个Activity。

"stateAlwaysHidden" 软键盘总是被隐藏的，当该Activity主窗口获取焦点时。

"stateVisible" 软键盘是可见的，当那个是正常合适的时(当用户导航到Activity主窗口时)。

"stateAlwaysVisible" 当用户选择这个Activity时，软键盘是可见的——也就是，也就是，当用户确定导航到该Activity时，而不是返回到它由于离开另一个Activity。

"adjustUnspecified" 它不被指定是否该Activity主窗口调整大小以便留出软键盘的空间，或是否窗口上的内容得到屏幕上当前的焦点是可见的。系统将自动选择这些模式中一种 主要依赖于是否窗口的内容有任何布局视图能够滚动他们的内容。如果有这样的一个视图，这个窗口将调整大小，这样的假设可以使滚动窗口的内容在一个较小的区 域中可见的。这个是主窗口默认的行为设置。

"adjustResize" 该Activity主窗口总是被调整屏幕的大小以便留出软键盘的空间。

"adjustPan" 该Activity主窗口并不调整屏幕的大小以便留出软键盘的空间。相反，当前窗口的内容将自动移动以便当前焦点从不被键盘覆盖和用户能总是看到输入内容 的部分。这个通常是不期望比调整大小，因为用户可能关闭软键盘以便获得与被覆盖内容的交互操作。

<strong>Android EditText 属性汇总</strong>

android:layout_gravity="center_vertical"

设置控件显示的位置：默认top，这里居中显示，还有bottom

android:hint="请输入数字！"

设置显示在空间上的提示信息

android:numeric="integer"

设置只能输入整数，如果是小数则是：decimal

android:singleLine="true"

设置单行输入，一旦设置为true，则文字不会自动换行。

android:password="true"

设置只能输入密码

android:textColor = "#ff8c00"

字体颜色

android:textStyle="bold"

字体，bold, italic, bolditalic

android:textSize="20dip"

大小

android:capitalize = "characters"

以大写字母写

android:textAlign="center"

EditText没有这个属性，但TextView有

android:textColorHighlight="#cccccc"

被选中文字的底色，默认为蓝色

android:textColorHint="#ffff00"

设置提示信息文字的颜色，默认为灰色

android:textScaleX="1.5"

控制字与字之间的间距

android:typeface="monospace"

字型，normal, sans, serif, monospace

android:background="@null"

空间背景，这里没有，指透明

<strong>android:layout_weight="1"</strong>

权重，控制控件之间的地位,在控制控件显示的大小时蛮有用的。

android:textAppearance="?android:attr/textAppearanceLargeInverse"

文字外观，这里引用的是系统自带的一个外观，？表示系统是否有这种外观，否则使用默认的外观。不知道这样理解对不对？

通过EditText的layout xml文件中的相关属性来实现:

1. 密码框属性 android:password="true" 这条可以让EditText显示的内容自动为星号，输入时内容会在1秒内变成*字样。

2. 纯数字 android:numeric="true" 这条可以让输入法自动变为数字输入键盘，同时仅允许0-9的数字输入

3. 仅允许 android:capitalize="cwj1987" 这样仅允许接受输入cwj1987，一般用于密码验证

<strong>下面是一些扩展的风格属性</strong>

android:editable="false" 设置EditText不可编辑

android:singleLine="true" 强制输入的内容在单行

android:ellipsize="end" 自动隐藏尾部溢出数据，一般用于文字内容过长一行无法全部显示时

RelativeLayout布局

android:layout_marginTop="25dip" //顶部距离

android:gravity="left" //空间布局位置

android:layout_marginLeft="15dip //距离左边距

// 相对于给定ID控件

android:layout_above 将该控件的底部置于给定ID的控件之上;

android:layout_below 将该控件的底部置于给定ID的控件之下;

android:layout_toLeftOf 将该控件的右边缘与给定ID的控件左边缘对齐;

android:layout_toRightOf 将该控件的左边缘与给定ID的控件右边缘对齐;

android:layout_alignBaseline 将该控件的baseline与给定ID的baseline对齐;

android:layout_alignTop 将该控件的顶部边缘与给定ID的顶部边缘对齐;

android:layout_alignBottom 将该控件的底部边缘与给定ID的底部边缘对齐;

android:layout_alignLeft 将该控件的左边缘与给定ID的左边缘对齐;

android:layout_alignRight 将该控件的右边缘与给定ID的右边缘对齐;

// 相对于父组件

android:layout_alignParentTop 如果为true,将该控件的顶部与其父控件的顶部对齐;

android:layout_alignParentBottom 如果为true,将该控件的底部与其父控件的底部对齐;

android:layout_alignParentLeft 如果为true,将该控件的左部与其父控件的左部对齐;

android:layout_alignParentRight 如果为true,将该控件的右部与其父控件的右部对齐;

// 居中

android:layout_centerHorizontal 如果为true,将该控件的置于水平居中;

android:layout_centerVertical 如果为true,将该控件的置于垂直居中;

android:layout_centerInParent 如果为true,将该控件的置于父控件的中央;

// 指定移动像素

android:layout_marginTop 上偏移的值;

android:layout_marginBottom 下偏移的值;

android:layout_marginLeft 左偏移的值;

android:layout_marginRight 　 右偏移的值;

android:id --- 为控件指定相应的ID

android:text --- 指定控件当中显示的文字，需要注意的是，这里尽量使用strings.xml文件当中的字符串

android:grivity --- 指定控件的基本位置，比如说居中，居右等位置这里指的是控件中的文本位置并不是控件本身。

android:textSize --- 指定控件当中字体的大小

android:background --- 指定该控件所使用的背景色，RGB命名法

android:width --- 指定控件的宽度

android:height --- 指定控件的高度

android:padding* --- 指定控件的内边距，也就是说控件当中的内容

android:sigleLine --- 如果设置为真的话，则控件的内容在同一行中进行显示

下边是相对布局属性的说明：RelativeLayout

android:layout_above 将该控件的底部至于给定ID控件之上

android:layout_below 将该控件的顶部至于给定ID的控件之下

android:layout_toLeftOf 将该控件的右边缘和给定ID的控件左边缘对齐

android:layout_toRightOf 将该控件的左边缘和给定ID的控件的右边缘对齐

android:layout_alignBaseline 该控件的baseline和给定ID的控件的baseline对齐

android:layout_alignBottom 将该控件的底部边缘与给定ID控件的底部边缘对齐

android:layout_alignLeft 将该控件的左边缘与给定ID控件的左边缘对齐

android:layout_alignRight 将该控件的右边缘与给定ID控件的右边缘对齐

android:layout_alignTop 将该控件的顶部边缘与给定ID控件的顶部对齐

android:alignParentBottom 如果该值为true,则将该控件的底部和父控件的底部对齐

android:layout_alignParentLeft 如果该值为true,则将该控件左边与父控件的左边对齐

android:layout_alignParentRight 如果该值为true,则将该控件的右边与父控件的右边对齐

android:layout_alignParentTop 如果该值为true,则将该控件的顶部与父控件的顶部对齐

android:layout_centerHorizontal 如果为真，该控件将被至于水平方向的中央

android:layout_centerInParent 如果为真，该控件将被至于父控件水平方向和垂直方向的中央

android:layout_centerVertical 如果为真，该控件将被至于垂直方向的中央

android:layout_marginLeft此属性用来设置控件之间的间隙（控件和控件之间和内边距不同）

android:padding="3dip"说明了四边的内边距是3dip

TableLayout

&lt;TableLayout xmlns:android="http://schemas.android.com/apk/res/android"

android:orientation="vertical"

android:layout_width="fill_parent"

android:layout_height="fill_parent"

android:stretchColumns="0"

&gt;&lt;/TableLayout&gt;

android:stretchColumns="0"第一列作为拉伸列填满整行

Java中修饰符总结：

1、访问控制修饰符

public的访问级别是最高的，其次是protected、默认和private

成员变量和成员方法可以处于4个访问级别中的一个：公开、受保护、默认或私有

顶层类可以处于公开或默认级别，顶层类不能被protected和private修饰

局部变量不能被访问控制修饰符修饰

2、abstract修饰符

抽象类不能被实例化

抽象类中可以没有抽象方法，但包含了抽象方法的类必须被定义为抽象方法

如果子类没有实现父类中所有的抽象方法，子类也必须定义为抽象类

抽象类不能被定义为private、final、和static类型

没有抽象的构造方法

抽象方法没有方法体

3、final修饰符

用final修饰的类不能被继承

用final修饰的方法不能被子类的方法覆盖

private类型的方法都默认为是final方法，因而不能被子类的方法覆盖

final变量必须被显式初始化，并且只能被赋值一次值

4、static修饰符

静态变量在内存中只有一个拷贝，在类的所有实例中共享

在静态方法中不能直接访问实例方法和实例变量

在静态方法中不能使用this和super关键字

静态方法不能被abstract修饰

静态方法和静态变量都可以通过类名直接访问

当类被加载时，静态代码块只能被执行一次。类中不同的静态方法代码块按他们在类中出现的顺序被依次执行

当多个修饰符连用时，修饰符的顺序可以颠倒，不过作为普遍遵守的编程规范，通常把访问控制修饰符放在首位，其次是static或abstact修饰符，接着就是其他的修饰符

5、以下修饰符连用是无意义的，会导致编译错误：

abstract与private

abstract与final

abstract与static

&nbsp;

摘自51CTO