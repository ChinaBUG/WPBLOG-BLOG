---
ID: 339
post_title: >
  Flash CS4:无法找到对动作脚本2.0
  进行类型检查所需的文件toplevel.as问题
author: ChinaBUG
post_excerpt: >
  关于Flash
  CS4输出错误信息“无法找到对动作脚本
  2.0
  进行类型检查所需的文件“toplevel.as”。请确保目录“$(LocalData)/Classes”在动作脚本首选参数的全局类路径中列出。”
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/flash-cs4-output-error-message-unable-to-find-action-script-conducting.html
published: true
post_date: 2010-03-05 14:31:07
---
　　在使用Flash CS4的时候，按Ctrl+Enter预览时，常常会输出错误信息，“无法找到对动作脚本 2.0 进行类型检查所需的文件“toplevel.as”。请确保目录“$(LocalData)/Classes”在动作脚本首选参数的全局类路径中列出“。

　　开始都忍了，可是有段时间工作上要做Flash，天天n次出错，实在受不了。在网上我看搜索不到好的解决办法，只好着手自己解决了下。

　　方法：

　　打开Flash CS4，选择编辑 &gt; 首选参数，选择ActionScript项，选择ActionScript 2.0 设置...，点"+"号，添加新路径，输入C:\Program Files\Adobe\Adobe Flash CS4\Common\First Run\Classes（您的FLASH安装目录），然后把Flash CS3的”toplevel.as“文件拷贝到此路径下，现在应该解决了！
　　toplevel.as下载：(请不要用下载工具，访问此页后用浏览器下载。)

　　<a href="http://cid-3d228983c0afaf80.skydrive.live.com/self.aspx/Documents/toplevel.zip" target="_blank">Skydrive下载</a>