---
ID: 1722
post_title: >
  FLASH-MD5加密(与PHP的MD5效果一致)
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/flash-md5-encryption-the-same-effect-with-phps-md5.html
published: true
post_date: 2011-08-14 19:42:40
---
本站U仆下载：<a href="http://u.ipodmp.com/viewfile.php?file_id=251&amp;file_key=F8hlq3Zx" target="_blank">FLASH md5加密下载一</a>

最近在FLASH游戏项目中需要用到MD5加密URL，自己编？不可能的嘛~~Google还是可以的，找到这个好东西，效果跟PHP的md5方法的加密结果是一致的，在这边推荐跟保留一下吧。

官方网站：<a href="http://pajhome.org.uk/crypt/md5/" target="_blank">Paj's Home: Cryptography: JavaScript MD5</a>

使用方法：

把下载来的md5.as放在swf同一个，同一级目录内，然后在第一帧内写入 #include "md5.as"然后就可以在需要的地方使用

hex_md5("blog.ipodmp.com")

这个是MD5的加密方法噢。你还可以使用SHA1加密方法：

hex_sha1("blog.ipodmp.com")

还有什么不懂就留言或者去官方查看。

附录：

这边还有一个也是一样的噢不过代码不一样滴，效果是一样的。补充哈

<a href="http://www.webtoolkit.info/demo/actionscript-md5" target="_blank">Actionscript MD5</a>