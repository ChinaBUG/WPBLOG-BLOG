---
ID: 1612
post_title: >
  密码保护：ShopEX-调试技巧之类相关的操作
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2011/07/shopex-debugging-techniques-and-the-like-related-to-the-operation.html
published: true
post_date: 2011-07-15 20:20:38
---
在做二次开发时，经常要用到一些类的方法，要是这些类不存在怎么办呢？

在PHP中，我们使用class_exists方法来检查

if(!class_exists('cct_product')){

print_r('不存在');

require(CORE_DIR.'/shop/controller/ctl.product.php');
}else{
print_r('存在');
}
exit;

另外
<div>bool <strong><strong>method_exists</strong></strong> ( object <tt>$object</tt> , string <tt>$method_name</tt> )</div>
如果 <em><tt>method_name</tt></em> 所指的方法在 <em><tt>object</tt></em> 所指的对象类中已定义，则返回 <strong><tt>TRUE</tt></strong>，否则返回 <strong><tt>FALSE</tt></strong>。