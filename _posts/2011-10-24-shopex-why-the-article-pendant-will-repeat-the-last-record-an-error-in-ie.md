---
ID: 1778
post_title: >
  ShopEX疑难-为什么文章挂件最后的记录会重复，在IE下出错
author: ChinaBUG
post_excerpt: |
  错误描述：
  添加四个文章挂件，结果第四个挂件在显示文章时会出现最后一条记录重复输出！
  如：四个挂件名为a1、a2、a3、a4并排排列着，则显示时，a4挂件的最后一条文章记录会出现两次，这个问题在ie6、ie7、360等下面均会出现，但是，查看生成的代码却会发现，他只有一个代码！即生成的代码是正确的，显示的时候是不正确的。很神奇吧？！
  为什么会出现这个原因，其实我也不知道，但是我知道怎么解决这个奇怪的问题！
layout: post
permalink: >
  http://blog.ipodmp.com/2011/10/shopex-why-the-article-pendant-will-repeat-the-last-record-an-error-in-ie.html
published: true
post_date: 2011-10-24 15:51:38
---
ShopEX疑难-为什么文章挂件最后的记录会重复，在IE下出错

错误描述：

添加四个文章挂件，结果第四个挂件在显示文章时会出现最后一条记录重复输出！

如：四个挂件名为a1、a2、a3、a4并排排列着，则显示时，a4挂件的最后一条文章记录会出现两次，这个问题在ie6、ie7、360等下面均会出现，但是，查看生成的代码却会发现，他只有一个代码！即生成的代码是正确的，显示的时候是不正确的。很神奇吧？！

为什么会出现这个原因，其实我也不知道，但是我知道怎么解决这个奇怪的问题！

你想知道吗？

那个解决方法真的好神奇，神奇到我不知道该说什么。

解决方法很简单，就是把主题模板内的注释删除！！！

是的，你没看错，就是你出现这个问题的模板文件里面的全部的&lt;!--...--&gt;里面的东西全部删除即可解决这个问题了。

反正就是模板文件里面不准出现&lt;!--...--&gt;这样的标签啦，神奇吧，有遇到的童靴试试看~~~有更好的解决方法的也请留言交流噢。