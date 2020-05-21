---
ID: 3255
post_title: >
  mobile-如何开发一个移动跨平台库：代码移植
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/03/mobile-how-to-develop-a-cross-platform-mobile-library-code-transplant.html
published: true
post_date: 2014-03-16 18:35:14
---
摘自：<a href="http://mobile.51cto.com/hot-431737.htm">如何开发一个移动跨平台库</a>
<h3>代码移植</h3>
另一个考虑过的选择是，只维护一个代码库，然后用适当的工具把代码翻译为平台对应的语言。这种选择也有它的缺陷：
<ul>
	<li>生成代码效率不会像原生开发者写的那样高。</li>
	<li>翻译过程很容易引入Bug，而且必须手动修复。</li>
	<li>导入的二进制文件很难被翻译，因为大多数的工具只能翻译源代码。</li>
</ul>
以下是几种移动平台代码移植工具。遗憾的是，没有一种能满足需求：
<ul>
	<li><a href="http://code.google.com/p/j2objc/" target="_blank" rel="nofollow"><strong>J2ObjC
</strong></a>Google 开发的工具，用于翻译 Java 代码为 Objective-C 。看起来质量比较高（与以下其他相比）。目前为止，它能把部分 Java 类翻译为 Objective-C ，但开发还没有完成。不幸的是，它目前翻译不了 Java 的 HTTP 请求，但是如果我们为每个平台单独实现这部分功能，也有实现的可能。这个项目从2012年9月建立至今。</li>
	<li><a href="https://github.com/appcelerator/hyperloop" rel="nofollow"><strong>Hyperloop
</strong></a>将 JavaScript 翻译为平台原生代码的工具。目前为止，它只支持 iOS ，而且并不稳定，但他们的计划是扩展到所有流行的平台。这个项目从2013年8月建立至今。</li>
	<li><a href="http://code.google.com/p/objc2j/" target="_blank" rel="nofollow"><strong>ObjC2J
</strong></a>将 Objective-C 翻译为 Java 的工具。这本来也是个很好的思路，但不幸的是，它还不够成熟，含有很多bug，经常输出不能编译的代码。</li>
	<li><a href="http://xmlvm.org/" target="_blank" rel="nofollow"><strong>XMLVM
</strong></a>将 JVM 字节码交叉编译为 Objective-C 的工具。这个工具不仅不够完善，用起来很复杂，并且需要下载/导入很多legacy jar。</li>
	<li><a href="http://www.apportable.com/" target="_blank" rel="nofollow"><strong>Apportable
</strong></a>将 iOS 应用转化为 Android 应用的工具。不幸的是，它达不到我们的要求，因为它只能翻译整个应用，无法翻译库，而且直接输出 .apk（Android 应用安装包）文件。</li>
	<li><a href="http://oss.readytalk.com/avian/" target="_blank" rel="nofollow"><strong>Avian
</strong></a>轻量级的 Java 虚拟机，可以嵌入 iOS app bundle 并运行 Java 代码。这个方案满足不了需求，因为想要让 iOS 上跑的 UI 代码与虚拟机中跑的 Java 库代码交互非常困难。</li>
	<li><a href="http://code.google.com/p/in-the-box/" target="_blank" rel="nofollow"><strong>in the box
</strong></a>在 iOS 上运行的移植 Dalvik 虚拟机和 Android Gingerbread (2.3) API。这个选项被否决了，因为这个项目已经失效。</li>
</ul>