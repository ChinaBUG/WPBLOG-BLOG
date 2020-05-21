---
ID: 269
post_title: 用DataFormatString格式化GridView
author: ChinaBUG
post_excerpt: >
  有个时间要在gridview中显示,但是保持着数据库中的是标准时间,很长,而且只需要显示日期,就想要格式化字符串,可是设置了DataFormatString就是不起作用,后来一查,原来要设置"行为"中......
layout: post
permalink: >
  http://blog.ipodmp.com/2009/11/asp-net-dataformatstring-for-gridview.html
published: true
post_date: 2009-11-27 13:54:10
---
　　有个时间要在gridview中显示,但是保持着数据库中的是标准时间,很长,而且只需要显示日期,就想要格式化字符串,可是设置了DataFormatString就是不起作用,后来一查,原来要设置"行为"中

　　HtmlEncode = false
　　DataFormatString="{0:格式字符串}"

　　在DataFormatString 中的 {0} 表示数据本身，而在冒号后面的格式字符串代表所们希望数据显示的格式；

数字、货币格式：
　　在指定的格式符号后可以指定小数所要显示的位数。例如原来的数据为「1.56」，若格式设定为 {0:N1}，则输出为「1.5」。其常用的数值格式如下表所示：

格式字符串 输入 结果
"{0:C}" 12345.6789 $12,345.68
"{0:C}" -12345.6789 ($12,345.68)
"{0:D}" 12345 12345
"{0:D8}" 12345 00012345
"{0:E}" 12345.6789 1234568E+004
"{0:E10}" 12345.6789 1.2345678900E+004
"{0:F}" 12345.6789 12345.68
"{0:F0}" 12345.6789 12346
"{0:G}" 12345.6789 12345.6789
"{0:G7}" 123456789 1.234568E8
"{0:N}" 12345.6789 12,345.68
"{0:N4}" 123456789 123,456,789.0000
"Total: {0:C}" 12345.6789 Total: $12345.68

常用的日期时间格式：

格式 说明 输出格式
d 精简日期格式 MM/dd/yyyy
D 详细日期格式 dddd, MMMM dd, yyyy
f 完整格式 (long date + short time) dddd, MMMM dd, yyyy HH:mm
F 完整日期时间格式
(long date + long time)
dddd, MMMM dd, yyyy HH:mm:ss
g 一般格式 (short date + short time) MM/dd/yyyy HH:mm
G 一般格式 (short date + long time) MM/dd/yyyy HH:mm:ss
m,M 月日格式 MMMM dd
s 适中日期时间格式 yyyy-MM-dd HH:mm:ss
t 精简时间格式 HH:mm
T 详细时间格式 HH:mm:ss

最后写一下中国常用的格式

{0:yyyy-MM-dd}