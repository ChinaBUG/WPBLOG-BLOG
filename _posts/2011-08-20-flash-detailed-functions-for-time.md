---
ID: 1735
post_title: FLASH-有关时间函数详解
author: ChinaBUG
post_excerpt: |
  虫曰：
  这边主要要记录一个是PHP里面的time()获取的时间秒数，是一个unix的时间戳，在FLASH中要如何获取同样的值，具体代码如下：
  now_date = new Date();
  int(now_date.getTime()/1000)
layout: post
permalink: >
  http://blog.ipodmp.com/2011/08/flash-detailed-functions-for-time.html
published: true
post_date: 2011-08-20 09:49:11
---
虫曰：

这边主要要记录一个是PHP里面的time()获取的时间秒数，是一个unix的时间戳，在FLASH中要如何获取同样的值，具体代码如下：

now_date = new Date();

int(now_date.getTime()/1000)
<p style="text-align: center;">-----------------------------------</p>
<p style="text-align: center;">---- 下面转摘 -----</p>
&nbsp;

Date (对象)

Date 对象能够使你获得相对于国际标准时间（格林威治标准时间，现在被称为 UTC-Universal Coordinated Time）或者是 Flash 播放器正运行的操作系统的时间和日期。要使用Date对象的方法，你就必须先创建一个Date对象的实体（Instance）。

Date 对象必须使用 Flash 5 或以后版本的播放器。

Date 对象的方法并不是静态的，但是在使用时却可以应用于所指定的单独实体。

Date 对象的方法简介：
<table width="574" border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="151">getDate</td>
<td valign="top" width="417">根据本地时间获取当前日期(本月的几号)</td>
</tr>
<tr>
<td valign="top" width="151">getDay</td>
<td valign="top" width="417">根据本地时间获取今天是星期几(0-Sunday,1-Monday...)</td>
</tr>
<tr>
<td valign="top" width="151">getFullYear</td>
<td valign="top" width="417">根据本地时间获取当前年份(四位数字)</td>
</tr>
<tr>
<td valign="top" width="151">getHours</td>
<td valign="top" width="417">根据本地时间获取当前小时数(24小时制,0-23)</td>
</tr>
<tr>
<td valign="top" width="151">getMilliseconds</td>
<td valign="top" width="417">根据本地时间获取当前毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">getMinutes</td>
<td valign="top" width="417">根据本地时间获取当前分钟数</td>
</tr>
<tr>
<td valign="top" width="151">getMonth</td>
<td valign="top" width="417">根据本地时间获取当前月份(注意从0开始:0-Jan,1-Feb...)</td>
</tr>
<tr>
<td valign="top" width="151">getSeconds</td>
<td valign="top" width="417">根据本地时间获取当前秒数</td>
</tr>
<tr>
<td valign="top" width="151">getTime</td>
<td valign="top" width="417">获取UTC格式的从1970.1.10:00以来的毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">getTimezoneOffset</td>
<td valign="top" width="417">获取当前时间和UTC格式的偏移值(以分钟为单位)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCDate</td>
<td valign="top" width="417">获取UTC格式的当前日期(本月的几号)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCDay</td>
<td valign="top" width="417">获取UTC格式的今天是星期几(0-Sunday,1-Monday...)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCFullYear</td>
<td valign="top" width="417">获取UTC格式的当前年份(四位数字)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCHours</td>
<td valign="top" width="417">获取UTC格式的当前小时数(24小时制,0-23)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCMilliseconds</td>
<td valign="top" width="417">获取UTC格式的当前毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">getUTCMinutes</td>
<td valign="top" width="417">获取UTC格式的当前分钟数</td>
</tr>
<tr>
<td valign="top" width="151">getUTCMonth</td>
<td valign="top" width="417">获取UTC格式的当前月份(注意从0开始:0-Jan,1-Feb...)</td>
</tr>
<tr>
<td valign="top" width="151">getUTCSeconds</td>
<td valign="top" width="417">获取UTC格式的当前秒数</td>
</tr>
<tr>
<td valign="top" width="151">getYear</td>
<td valign="top" width="417">根据本地时间获取当前缩写年份(当前年份减去1900)</td>
</tr>
<tr>
<td valign="top" width="151">setDate</td>
<td valign="top" width="417">设置当前日期(本月的几号)</td>
</tr>
<tr>
<td valign="top" width="151">setFullYear</td>
<td valign="top" width="417">设置当前年份(四位数字)</td>
</tr>
<tr>
<td valign="top" width="151">setHours</td>
<td valign="top" width="417">设置当前小时数(24小时制,0-23)</td>
</tr>
<tr>
<td valign="top" width="151">setMilliseconds</td>
<td valign="top" width="417">设置当前毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">setMinutes</td>
<td valign="top" width="417">设置当前分钟数</td>
</tr>
<tr>
<td valign="top" width="151">setMonth</td>
<td valign="top" width="417">设置当前月份(注意从0开始:0-Jan,1-Feb...)</td>
</tr>
<tr>
<td valign="top" width="151">setSeconds</td>
<td valign="top" width="417">设置当前秒数</td>
</tr>
<tr>
<td valign="top" width="151">setTime</td>
<td valign="top" width="417">设置UTC格式的从1970.1.10:00以来的毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">setUTCDate</td>
<td valign="top" width="417">设置UTC格式的当前日期(本月的几号)</td>
</tr>
<tr>
<td valign="top" width="151">setUTCFullYear</td>
<td valign="top" width="417">设置UTC格式的当前年份(四位数字)</td>
</tr>
<tr>
<td valign="top" width="151">setUTCHours</td>
<td valign="top" width="417">设置UTC格式的当前小时数(24小时制,0-23)</td>
</tr>
<tr>
<td valign="top" width="151">setUTCMilliseconds</td>
<td valign="top" width="417">设置UTC格式的当前毫秒数</td>
</tr>
<tr>
<td valign="top" width="151">setUTCMinutes</td>
<td valign="top" width="417">设置UTC格式的当前分钟数</td>
</tr>
<tr>
<td valign="top" width="151">setUTCMonth</td>
<td valign="top" width="417">设置UTC格式的当前月份(注意从0开始:0-Jan,1-Feb...)</td>
</tr>
<tr>
<td valign="top" width="151">setUTCSeconds</td>
<td valign="top" width="417">设置UTC格式的当前秒数</td>
</tr>
<tr>
<td valign="top" width="151">setYear</td>
<td valign="top" width="417">设置当前缩写年份(当前年份减去1900)</td>
</tr>
<tr>
<td valign="top" width="151">toString</td>
<td valign="top" width="417">将日期时间值转换成"日期/时间"形式的字符串值</td>
</tr>
<tr>
<td valign="top" width="151">Date.UTC</td>
<td valign="top" width="417">返回指定的UTC格式日期时间的固定时间值</td>
</tr>
</tbody>
</table>
创建新的 Date 对象

语法：

new Date();

new Date(year [, month [, date [, hour [, minute [, second [, millisecond ]]]]]] );

参数：

year 　　　　是一个 0 到 99 之间的整数，对应于 1900 到 1999 年，或者为四位数字指定确定的年份；

month　　　　是一个 0 (一月) 到 11 (十二月) 之间的整数，这个参数是可选的；

date　 　　　是一个 1 到 31 之间的整数，这个参数是可选的；

hour 　　　　是一个 0 (0:00am) 到 23 (11:00pm) 之间的整数，这个参数是可选的；

minute 　　　是一个 0 到 59 之间的整数，这个参数是可选的；

second 　　　是一个 0 到 59 之间的整数，这个参数是可选的；

millisecond　是一个 0 到 999 之间的整数，这个参数是可选的；

注释：

对象。新建一个 Date 对象。

播放器支持：

Flash 5 或以后的版本。

例子：

下面是获得当前日期和时间的例子：

now = new Date();

下面创建一个关于国庆节的 Date 对象的例子：

national_day = new Date (49, 10, 1);

下面是新建一个 Date 对象后，利用 Date 对象的 getMonth、getDate、和 getFullYear方法获取时间，然后在动态文本框中输出的例子。

myDate = new Date();

dateTextField = (mydate.getMonth() + "/" + myDate.getDate() + "/" + mydate.getFullYear());

Date &gt; Date.getDate

Date.getDate

语法：myDate.getDate();

参数：无

注释：

方法。根据本地时间获取当前日期(本月的几号)，返回值是 1 到 31 之间的一个整数。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getDay

Date.getDay

语法：myDate.getDay();

参数：无

注释：

方法。根据本地时间获取今天是星期几(0-星期日，1-星期一...)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getFullYear

Date.getFullYear

语法：myDate.getFullYear();

参数：无

注释：

方法。根据本地时间获取当前年份(四位数字，例如 2000)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

例子：

下面的例子新建了一个 Date 对象，然后在输出窗口输出用 getFullYear 方法获得的年份：

myDate = new Date();

trace(myDate.getFullYear());

Date &gt; Date.getHours

Date.getHours

语法：myDate.getHours();

参数：无

注释：

方法。根据本地时间获取当前小时数(24小时制，返回值为0-23之间的整数)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getMilliseconds

Date.getMilliseconds

语法：myDate.getMilliseconds();

参数：无

注释：

方法。根据本地时间获取当前毫秒数(返回值是 0 到 999 之间的一个整数)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getMinutes

Date.getMinutes

语法：myDate.getMinutes();

参数：无

注释：

方法。根据本地时间获取当前分钟数(返回值是 0 到 59 之间的一个整数)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getMonth

Date.getMonth

语法：myDate.getMonth();

参数：无

注释：

方法。根据本地时间获取当前月份(注意从0开始:0-一月,1-二月...)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getSeconds

Date.getSeconds

语法：myDate.getSeconds();

参数：无

注释：

方法。根据本地时间获取当前秒数(返回值是 0 到 59 之间的一个整数)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getTime

Date.getTime

语法：myDate.getTime();

参数：无

注释：

方法。按UTC格式返回从1970年1月1日0:00am起到现在的毫秒数。使用这个方法可以描述不同时区里的同一瞬间的时间。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getTimezoneOffset

Date.getTimezoneOffset

语法：mydate.getTimezoneOffset();

参数：无

注释：

方法。获取当前时间和UTC格式的偏移值(以分钟为单位)。

播放器支持：Flash 5 或以后版本。

例子：

下面的例子将返回北京时间与UTC时间之间的偏移值。

new Date().getTimezoneOffset();

结果如下：

480 (8 小时 * 60 分钟/小时 = 480 分钟)

Date &gt; Date.getUTCDate

Date.getUTCDate

语法：myDate.getUTCDate();

参数：无

注释：

方法。获取UTC格式的当前日期(本月的几号)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCDay

Date.getUTCDay

语法：myDate.getUTCDate();

参数：无

注释：

方法。获取UTC格式的今天是星期几(0-星期日，1-星期一...)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCFullYear

Date.getUTCFullYear

语法：myDate.getUTCFullYear();

参数：无

注释：

方法。获取UTC格式的当前年份(四位数字)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCHours

Date.getUTCHours

语法：myDate.getUTCHours();

参数：无

注释：

方法。获取UTC格式的当前小时数(24小时制,返回值为0-23之间的一个整数)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCMilliseconds

Date.getUTCMilliseconds

语法：myDate.getUTCMilliseconds();

参数：无

注释：

方法。获取UTC格式的当前毫秒数(返回值是 0 到 999 之间的一个整数)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCMinutes

Date.getUTCMinutes

语法：myDate.getUTCMinutes();

参数：无

注释：

方法。获取UTC格式的当前分钟数(返回值是 0 到 59 之间的一个整数)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCMonth

Date.getUTCMonth

语法：myDate.getUTCMonth();

参数：无

注释：

方法。获取UTC格式的当前月份(注意从0开始:0-一月,1-二月...)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getUTCSeconds

Date.getUTCSeconds

语法：myDate.getUTCSeconds();

参数：无

注释：

方法。获取UTC格式的当前秒数(返回值是 0 到 59 之间的一个整数)。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.getYear

Date.getYear

语法：myDate.getYear();

参数：无

注释：

方法。根据本地时间获取当前缩写年份(当前年份减去1900)。本地时间由 Flash 播放器所运行的操作系统决定。例如 2000 年用100来表示。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setDate

Date.setDate

语法：myDate.setDate(date);

参数：date 为 1 到 31 之间的一个整数。

注释：

方法。根据本地时间设置当前日期(本月的几号)。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setFullYear

Date.setFullYear

语法：myDate.setFullYear(year [, month [, date]] );

参数：

year 指定的四位整数代表指定年份，二位数字并不代表年份，如99不表示1999，只表示公元99年

month 是一个从 0 (一月) 到 11 (十二月) 之间的整数，这个参数是可选的。

date 是一个从 1 到 31 之间的整数，这个参数是可选的。

注释：

方法。根据本地时间设定年份。如果设定了 month 和 date 参数，将同时设定月份和日期。本地时间由 Flash 播放器所运行的操作系统决定。设定之后 getUTCDay 和 getDay 方法所获得的值将出现相应的变化。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setHours

Date.setHours

语法：myDate.setHours(hour);

参数：hour 是一个从 0 (0:00am) 到 23 (11:00pm) 之间的整数。

注释：

方法。根据本地时间设置当前小时数。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setMilliseconds

Date.setMilliseconds

语法：myDate.setMilliseconds(millisecond);

参数：millisecond 是一个从 0 到 999 之间的整数。

注释：

方法。根据本地时间设置当前毫秒数。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setMinutes

Date.setMinutes

语法：myDate.setMinutes(minute);

参数：minute 是一个从 0 到 59 之间的整数。

注释：

方法。根据本地时间设置当前分钟数。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setMonth

Date.setMonth

语法：myDate.setMonth(month [, date ]);

参数：

month 是一个从 0 (一月) 到 11 (十二月)之间的整数

date 是一个从 1 到 31 之间的整数，这个参数是可选的。

注释：

方法。根据本地时间设置当前月份数，如果选用了 date 参数，将同时设定日期。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setSeconds

Date.setSeconds

语法：myDate.setSeconds(second);

参数：second 是一个从 0 到 59 之间的整数。

注释：

方法。根据本地时间设置当前秒数。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setTime

Date.setTime

语法：myDate.setTime(millisecond);

参数：millisecond 是一个从 0 到 999 之间的整数。

注释：

方法。用毫秒数来设定指定的日期。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCDate

Date.setUTCDate

语法：myDate.setUTCDate(date);

参数：date 是一个从 1 到 31 之间的整数。

注释：

方法。按UTC格式设定日期，使用本方法将不会影响 Date 对象的其他字段的值，但是 getUTCDay 和 getDay 方法会返回日期更改过后相应的新值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCFullYear

Date.setUTCFullYear

语法：myDate.setUTCFullYear(year [, month [, date]]);

参数：

year 代表年份的四位整数，如 2000

month 一个从 0 (一月) 到 11 (十二月)之间的整数，可选参数。

date 一个从 1 到 31 之间的整数，可选参数。

注释：

方法。按UTC格式设定年份，如果使用了可选参数，还同时设定月份和日期。设定过后 getUTCDay 和 getDay 方法会返回一个相应的新值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCHours

Date.setUTCHours

语法：myDate.setUTCHours(hour [, minute [, second [, millisecond]]]));

参数：

hour 是一个从 0 (0:00am) 到 23 (11:00pm)之间的整数。

minute 是一个从 0 到 59 之间的整数，可选参数。

second 是一个从 0 到 59 之间的整数，可选参数。

millisecond 是一个从 0 到 999 之间的整数，可选参数。

注释：

方法。设定UTC格式的小时数，如果是用可选参数，同时会设定分钟、秒和毫秒值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCMilliseconds

Date.setUTCMilliseconds

语法：myDate.setUTCMilliseconds(millisecond);

参数：millisecond 是一个从 0 到 999 之间的整数。

注释：

方法。设定UTC格式的毫秒数。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCMinutes

Date.setUTCMinutes

语法：myDate.setUTCMinutes(minute [, second [, millisecond]]));

参数：

minute 是一个从 0 到 59 之间的整数，可选参数。

second 是一个从 0 到 59 之间的整数，可选参数。

millisecond 是一个从 0 到 999 之间的整数，可选参数。

注释：

方法。设定UTC格式的分钟数，如果是用可选参数，同时会设定秒和毫秒值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCMonth

Date.setUTCMonth

语法：myDate.setUTCMonth(month [, date]);

参数：

month 是一个从 0 (一月) 到 11 (十二月)之间的整数

date 是一个从 1 到 31 之间的整数，这个参数是可选的。

注释：

方法。设定UTC格式的月份，同时可选设置日期。设定后 getUTCDay 和 getDay 方法会返回相应的新值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setUTCSeconds

Date.setUTCSeconds

语法：myDate.setUTCSeconds(second [, millisecond]));

参数：

second 是一个从 0 到 59 之间的整数，可选参数。

millisecond 是一个从 0 到 999 之间的整数，可选参数。

注释：

方法。设定UTC格式的秒数，如果是用可选参数，同时会设定毫秒值。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.setYear

Date.setYear

语法：myDate.setYear(year);

参数：year 是一个代表年份的四位整数，如 2000。

注释：

方法。根据本地时间设定年份。本地时间由 Flash 播放器所运行的操作系统决定。

播放器支持：Flash 5 或以后版本。

Date &gt; Date.toString

Date.toString

语法：myDate.toString();

参数：无

注释：

方法。将日期时间值转换成"日期/时间"形式的字符串值

播放器支持：Flash 5 或以后版本。

例子：

下面的例子将国庆节的 national_day 对象输出成可读的字符串：

var national_day = newDate(49, 9, 1, 10, 00);

trace (national_day.toString());

Output (for Pacific Standard Time):

结果为：Sat Oct 1 10:00:00 GMT+0800 1949

Date &gt; Date.UTC

Date.UTC

语法：Date.UTC(year, month [, date [, hour [, minute [, second [, millisecond ]]]]]);

参数：

year 代表年份的四位整数，如 2000

month 一个从 0 (一月) 到 11 (十二月)之间的整数。

date 一个从 1 到 31 之间的整数，可选参数。

hour 是一个从 0 (0:00am) 到 23 (11:00pm)之间的整数。

minute 是一个从 0 到 59 之间的整数，可选参数。

second 是一个从 0 到 59 之间的整数，可选参数。

millisecond 是一个从 0 到 999 之间的整数，可选参数。

注释：

方法。返回指定时间距 1970 年 1 月 1 日 0:00am 的毫秒数。这是一个静态的方法，不需要特定的对象。它能够创建一个新的 UTC 格式的 Date 对象，而 new Date() 所创建的是本地时间的 Date 对象。

播放器支持：Flash 5 或以后版本。