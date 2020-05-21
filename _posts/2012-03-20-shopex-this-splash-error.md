---
ID: 1986
post_title: 'ShopEX-$this->splash不能工作一例'
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/03/shopex-this-splash-error.html
published: true
post_date: 2012-03-20 14:31:52
---
shopex在开发时，经常使用$this-&gt;splash来做错误跳转或者成功跳转，但是，这次我自己开发了一个ctl.other.php控制器，却发现，$this-&gt;splash出现错误，不能工作了！

找呀找，都没看到代码的错误~

后来一层层的排除，最后才发现仅仅是我增加了一个ctl_other()初始化方法造成的，把这个方法删除，问题解决了。

具体为什么会出现这个问题，期待高手指导哈。