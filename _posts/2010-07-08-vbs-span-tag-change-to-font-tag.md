---
ID: 662
post_title: VBS-SPAN标签替换成FONT标签
author: ChinaBUG
post_excerpt: |
  　　FCKEditor编辑器在生成HTML格式的时候使用的是SPAN标签，加上STYLE属性来控制字体大小、颜色等，可惜FLASH中的渲染却不支持这个格式（有知道如何支持的请联系我噢）。
  　　这个函数就是用来过滤并替换SPAN标签并转换为FONT标签，不过不完善，只能实现简单的替换，另外自动添加属性的功能，因为基础差，不懂得如何写正则，有知道的也敬请指导。
layout: post
permalink: >
  http://blog.ipodmp.com/2010/07/vbs-span-tag-change-to-font-tag.html
published: true
post_date: 2010-07-08 13:59:44
---
　　FCKEditor编辑器在生成HTML格式的时候使用的是SPAN标签，加上STYLE属性来控制字体大小、颜色等，可惜FLASH中的渲染却不支持这个格式（有知道如何支持的请联系我噢）。

　　这个函数就是用来过滤并替换SPAN标签并转换为FONT标签，不过不完善，只能实现简单的替换，因为基础差，不懂得如何写正则，有完美解决方案的也敬请指导。

Function span2font(strcontent)
     dim re
     Set re=new RegExp
     re.IgnoreCase =true
     re.Global=True
     strcontent=replace(strcontent,"font-","")
     strcontent=replace(strcontent,";"," ")
     strcontent=replace(strcontent,": ","=")
     strcontent=replace(strcontent,":","=")
     re.pattern="&lt;span style=""([^&gt;]+?)""&gt;(.+?)"
     strcontent=re.replace(strcontent,"&lt;font $1&gt;$2")
     strcontent=replace(strcontent,"&lt;/span&gt;","&lt;/font&gt;")
     re.pattern="(\s+\w+)\=([^&gt;\""\s]+)"
     strcontent=re.replace(strcontent,"$1=""$2""")
     set re=Nothing
     span2font = strcontent
End Function

span2font("&lt;span style=""font-size: larger;color: #ff0000""&gt;关于&lt;/span&gt;&lt;span style=""font-weight: bold;""&gt;&lt;/span&gt;&lt;span style=""color: #ff0000""&gt;虫子&lt;/span&gt;")