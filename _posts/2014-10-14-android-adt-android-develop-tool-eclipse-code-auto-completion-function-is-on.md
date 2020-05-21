---
ID: 3668
post_title: >
  Android-ADT(Android Develop
  Tool)/Eclipse的代码自动补全功能开启
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/10/android-adt-android-develop-tool-eclipse-code-auto-completion-function-is-on.html
published: true
post_date: 2014-10-14 15:53:08
---
最近在开发Android程序的时候发觉为啥别人的编辑器可以自动补全代码，为啥我的却不行~~

好吧，我承认我老土了~

手动的补全操作是：在需要的地方按住Alt键然后再按/符号（默认输入“ .” 后才有代码补全提示），一个个按过去会累死人的噢。
<blockquote><em>自动补全的快捷键：ALT+/</em></blockquote>
那么怎么自动化一点噢？

其实好简单的，只需要做一下设置即可。具体如下：

对于Java代码需要设置如下位置：

Window\Preferences\Java\Editor\Content Assist

找到Auto activation里面Auto activation triggers for Java项，后面的值改为你需要的即可，比如改为：
<blockquote><em>.,_abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ</em></blockquote>
应用即可。

而对于XML文件来说，上面的设置是管不到的，所以需要从下面位置来设置：

Window\Preferences\XML\XML Files\Editor\Content Assist

找到Auto Activation/Automatically make suggestions/Prompt when these characters are inserted项，设置为：
<blockquote><em>&lt;=:_abcdefghijklmnopqrstuvwxyz</em></blockquote>
应用保存即可。