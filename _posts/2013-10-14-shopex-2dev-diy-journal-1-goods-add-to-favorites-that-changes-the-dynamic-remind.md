---
ID: 2599
post_title: >
  ShopEX二次开发DIY日记1之前台商品添加到收藏夹的提示改为动态对话框(MessageBox)提醒
author: ChinaBUG
post_excerpt: |
  虫曰：
  《二次开发DIY日记》系列由ChinaBUG企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。
  -
  我们在查看商品详情时，在右边加入购物车边上有一个收藏此商品，点击之后就会告诉你添加收藏成功。那么，如果希望点击收藏此商品之后用对话框提醒会员呢？
  DIY一下吧，具体修改步骤如下：
  首先，我们需要找到控制这个功能的代码，经过研究位于statics\script\goodscupcake.js文件内，我们打开goodscupcake.js文件会发现这个文件已经加密哈，其实就是简单的去掉行什么的哈，我们可以使用工具解开，我一般使用站长之家的工具JavaScript/HTML格式化来解码，这边需要提醒的是最好使用比较好的解压工具噢，要不解出来的代码会有点问题，在代码简写的情况下会出现错误噢。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/10/shopex-2dev-diy-journal-1-goods-add-to-favorites-that-changes-the-dynamic-remind.html
published: true
post_date: 2013-10-14 17:03:54
---
虫曰：

《<a href="http://blog.ipodmp.com/?s=二次开发DIY日记">二次开发DIY日记</a>》系列由<a href="http://blog.ipodmp.com/about-chinabug/">ChinaBUG</a>企划，根据开发过程中客户需求做的修改而延伸出来的开发要点，将有很多的需求点可能在实际应用中并不会需要到，在本系列之中我们将会有所考虑的给予分析解答，主要目的只为了更好的说明如何根据不同的需求点来DIY我们的程序。

-

我们在查看商品详情时，在右边加入购物车边上有一个收藏此商品，点击之后就会告诉你添加收藏成功。那么，如果希望点击收藏此商品之后用对话框提醒会员呢？

DIY一下吧，具体修改步骤如下：

首先，我们需要找到控制这个功能的代码，经过研究位于statics\script\goodscupcake.js文件内，我们打开goodscupcake.js文件会发现这个文件已经加密哈，其实就是简单的去掉行什么的哈，我们可以使用工具解开，我一般使用站长之家的工具<a href="http://tool.chinaz.com/Tools/JsFormat.aspx">JavaScript/HTML格式化</a>来解码，这边需要提醒的是最好使用比较好的解压工具噢，要不解出来的代码会有点问题，在代码简写的情况下会出现错误噢。

格式化之后看起来就很舒服了，我们找到

e.nodeValue = "已加入收藏";

这一行，在他下面增加如下代码：

MessageBox.show('已加入收藏');

然后点击普通压缩，然后把代码复制到goodscupcake.js文件里面，保存上传到你的网站上，刷新，如果没问题的话，你点击收藏此商品之后会出现一个绿色背景的小对话框，告诉你已加入收藏了。

怎样试了吗？没骗你吧~

如果你有尝试的话，那么你肯定很快就会发现一个问题，什么你没发现问题？那就是你没有尝试啦。

刷新一下该页，你会发现一个问题，提示框又出现了！

可是明明你没有点击，怎么会出现提示呢？其实这个是程序自动触发的，如果你觉得这个提示没有问题的话，那么就不用看了，如果觉得这个是一个大问题那么我们下面就来解决吧。

我们知道这个是系统自动触发的，那么我们很简单的就可以想到把这个给删除了呗。

请接着刚才修改的地方继续往下找到：

$$("li[star]").each(function(f) {

这一行，然后在下面你会看到

if (c.contains(e)) {
a(f, "on")
}

然后，在if前面增加/*再在}后面增加*/形成如下形式

/*        if (c.contains(e)) {
a(f, "on")
}*/

这个是注释掉这段代码，因为我们不知道未来你会不会需要用到，那就先保留吧。

然后保存，上传，刷新，然后你会发现，还真的不再提醒了，这次是真的不再提醒了，因为你会发现动态提示没了，收藏此商品按钮边上的提示也没了。

你需要这边的文字提示要出现嘛？不需要的话就没事了噢。需要的话。你只好往下看了噢。

这些文字提醒没了，怪不习惯的，怎么让他显示出来呢？

都说是DIY了，那么动手吧。

我们的目标是 --- 增加一个参数来控制要不要显示！

我们在代码中往上找，直接找到

var a = function(g, h, f) {
if (h == "on") {

处，然后有基础的同学就知道我们需要在这边做修改啦。我添加了一个参数叫做ff然后上面的代码就变成了下面的样子了：

var a = function(g, h, f, ff) {
if (h == "on") {

然后找到我们刚才修改的

MessageBox.show('已加入收藏');

处，增加一个判断，如下：

if (ff != true) MessageBox.show('已加入收藏');

然后，嗯，还有一个调用的需要修改，别着急，请往下找：

if (c.contains(e)) {

处，这个在上一步被我们给注释掉了，这次就把/**/给删除吧，然后修改为下面的代码：

if (c.contains(e)) {
a(f, "on", e, true)
}
f.addEvent("click",
function(g) {
g.stop();
a(f, b[f.className], e, false)
})

看到没，凡是a(...)调用的地方都要增加一个参数，保存，上传，刷新吧，你会发现，天空干净了~

附录：具体修改之后的代码

...//其他无关的略

var a = function(g, h, f, ff) {
if (h == "on") {
g.className = "star-on";
var e = g.firstChild;
while (e.nodeType != 3) {
e = e.firstChild
}
e.nodeValue = "已加入收藏";
if (ff != true) MessageBox.show('已加入收藏')
} else {
g.className = "star-off";
var e = g.firstChild;
while (e.nodeType != 3) {
e = e.firstChild
}
e.nodeValue = "收藏此商品"
}
if (!f) {
return
}
d.write($splat((d.read("S[GFAV]") || "").split(","))[b[h + "_"]](f).clean().join(","));
new Request().post("index.php?member-" + f + "-ajax" + b[h] + "Fav.html", $H({
t: $time()
}))
};
var c = $splat((d.read("S[GFAV]") || "").split(","));
$$("li[star]").each(function(f) {
var e = f.get("star");
if (c.contains(e)) {
a(f, "on", e, true)
}
f.addEvent("click",
function(g) {
g.stop();
a(f, b[f.className], e, false)
})
})

...//其他无关的略