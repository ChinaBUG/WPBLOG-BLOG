---
ID: 627
post_title: >
  FLASH整站制作细节(需要准备学习什么知识储备)
author: ChinaBUG
post_excerpt: |
  FLASH整站制作过程中需要的一些知识跟一些基础跟一些注意的事项分享。
  最近在做FLASH整站开发，想到一点，更新一点吧。
  有什么意见就MAIL我吧。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/06/flash-making-details.html
published: true
post_date: 2010-06-04 13:58:13
---
FLASH整站制作过程中需要的一些知识跟一些基础跟一些注意的事项分享。

最近在做FLASH整站开发，想到一点，更新一点吧。

有什么意见就MAIL我吧。

本站的语法参考目录

I.<a title="永久链接到：FLASH脚本命令参考" href="http://blog.ipodmp.com/archives/flash-script-command-reference/" rel="bookmark">FLASH脚本命令参考</a>

---------------------------------

1、一定不要随便在对象前添加<strong>_root</strong>或者<strong>stage</strong>关键字

_root和stage都是代表舞台对象，在FLASH整站开发中，经常要在SWF中调用SWF那么就会出现找不到对象的存在，所以别乱添加，我的做法是在载入页设定_root或者stage，而在被载入页内，都不添加。

如果你非要加怎么办？我也不知道，反正我不懂得怎么处理，手册上也没看过，案例也没看到过，你有这方面的资料，可以推荐给我。

2、变量、对象一定要看关键点所在位置

在AS编写中，一定要注意，不是全部的代码都是写第一帧里，也不是都写最后一帧里。如果代码是直接写在MC上的话，那么具体情况这边就不说哈。

在时间轴上，变量一定不要在初始化之前使用，这个很明显的，却会因为制作的时候，时间轴上分开，就会犯错，所以没必要时，一定要在同一帧内定义跟赋值。所以最好的办法就是建立一个层，专门来写代码，这样不会因为对象内需要用到的变量在初始化前使用。

专门新建一个层写代码就会出现对象找不到的现象，所以这就是要说的第二点。一个MC做动画，定义完名称，设定3个关键帧，那么就会出现三个对象，只不过是同一个，不同状态而已，那么功能要作用在个状态，则代码就要写在哪里的关键帧，不能都写第一帧，那样只有第一个关键帧状态内的对象能工作，而其他则没反应。

3、对于常用的元件，最好做成单独的swf文件调用

没什么好说的，好处比坏处多。灵活性、可维护性绝对比做在一起的大杂烩来的高。

4、对象隐藏代码：

menus.subMenu._visible = false|true;

5、拖动功能：

下面代码写在拖动对象上。

onClipEvent (mouseMove)
{
Mouse.hide();
startDrag ("_root.拖动对象", true, 50, 50, 500, 370);
}

6、数组初始化，不出现未定义错误（undefined）-- <span style="text-decoration: underline;"><strong>同2点</strong></span>

var image:Array = new Array ();

image.push(image_value);

注：在数组定义时，出现一个错误，就是undefined，不知道是什么回事，情况描述如下：使用var image:Array = new Array ();然后在循环中赋值，结果在循环外面读不到数据。除非再循环里面对这个变量做初始化操作，入image[0]=1;这样，然后再赋正确的值，好奇怪。

<span style="text-decoration: underline;"><span style="color: #888888;">2010.05.31 恩，经过奋战，发现是我自己的错误哈，使用XML数据，返回函数内赋值，还没赋值之前，我需要的代码就已经工作，自然读不到数据啦。</span></span>

7、善用_global关键字

在文件调用中，明明指定好路径，可是怎么也不能运行自定义函数，怎么办呢？改不了路径，我们就修改函数的作用范围，提升作用范围吧，_global关键字就是用来定义全局的。用法如下：

_global.SetText = function(info){
infobox._visible = true;
if(info!=""){
info = info;
}else{
info = "暂无店铺信息,请等待更新.";
}
infobox.alltext.text = info;
}