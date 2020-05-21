---
ID: 369
post_title: >
  网页布局中易犯的10个CSS小错误
author: ChinaBUG
post_excerpt: |
  1. 检查HTML元素是否有拼写错误、是否忘记结束标记
  2. 检查CSS是否书写正确
  3. 用删除法确定错误发生的位置
  4. 利用border属性确定出错元素的布局特性
  5. float元素的父元素不能指定clear属性
  6. float元素务必指定width属性
  7. float元素不能指定margin和padding等属性
  8. float元素的宽度之和要小于100%
  9. 是否重设了默认的样式？
  10. 是否忘记了写DTD？
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/easy-page-layout-in-css-committed-10-minor-errors.html
published: true
post_date: 2010-03-24 14:06:29
---
　　即使是CSS高手，也难免在书写CSS代码的时候出一些小错误，或者说，任何一种代码都是如此。小错误却往往造成大问题，浪费很多无辜的时间来调试和排错。查看下面这份<strong>网页布局中易犯的10个CSS小错误</strong>，努力的修正你可能会犯的错误，加速你的前端开发效率。
<h4>1. 检查HTML元素是否有拼写错误、是否忘记结束标记</h4>
　　即使是老手也经常会弄错div的嵌套关系。可以用dreamweaver的验证功能检查一下有无错误。
<h4>2. 检查CSS是否书写正确</h4>
　　检查一下有无拼写错误、是否忘记结尾的 } 等。可以利用CleanCSS来检查 CSS的拼写错误。CleanCSS本是为CSS减肥的工具，但也能检查出拼写错误。
<h4>3. 用删除法确定错误发生的位置</h4>
　　如果错误影响了整体布局，则可以逐个删除div块，直到删除某个div块后显示恢复正常，即可确定错误发生的位置。
<h4>4. 利用border属性确定出错元素的布局特性</h4>
　　使用float属性布局一不小心就会出错。这时为元素添加border属性确定元素边界，错误原因即水落石出。
<h4>5. float元素的父元素不能指定clear属性</h4>
　　MacIE下如果对float的元素的父元素使用clear属性，周围的float元素布局就会混乱。这是MacIE的著名的bug，倘若不知道就会走弯路。
<h4>6. float元素务必指定width属性</h4>
　　很多浏览器在显示未指定width的float元素时会有bug。所以不管float元素的内容如何，一定要为其指定width属性。

　　另外指定元素时尽量使用em而不是px做单位。
<h4>7. float元素不能指定margin和padding等属性</h4>
　　IE在显示指定了margin和padding的float元素时有bug。因此不要对float元素指定margin和padding属性（可以在float元素内部嵌套一个div来设置margin和padding）。也可以使用hack方法为IE指定特别的值。
<h4>8. float元素的宽度之和要小于100%</h4>
　　如果float元素的宽度之和正好是100%，某些古老的浏览器将不能正常显示。因此请保证宽度之和小于99%。
<h4>9. 是否重设了默认的样式？</h4>
　　某些属性如margin、padding等，不同浏览器会有不同的解释。因此最好在开发前首先将全体的margin、padding设置为0、列表样式设置为none等。
<h4>10. 是否忘记了写DTD？</h4>
　　如果无论怎样调整不同浏览器显示结果还是不一样，那么可以检查一下页面开头是不是忘了写下DTD声明。