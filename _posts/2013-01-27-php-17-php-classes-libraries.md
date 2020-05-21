---
ID: 2293
post_title: PHP-17个非常有用的PHP类和库
author: ChinaBUG
post_excerpt: >
  在我们日常程序开发当中，使用一个好的而且成熟的PHP类，可以减少很多手工编码，通过这些通用API的使用，可以大大减轻我们的开发工作。今天，我们将重点介绍了一些很少见却又非常实用的PHP类、库和组件，这将有助于您提高程序功能，更重要是减少应用程序的开发时间。
layout: post
permalink: >
  http://blog.ipodmp.com/2013/01/php-17-php-classes-libraries.html
published: true
post_date: 2013-01-27 21:39:19
---
<div>
<div>
<div>

虫曰：如果英文比较好的，推荐查看完整版的PHP类库，<a href="http://www.hotscripts.com/blog/50-php-classes-libraries/">50+有用的PHP类库</a>

在我们日常程序开发当中，使用一个好的而且成熟的PHP类，可以减少很多手工编码，通过这些通用API的使用，可以大大减轻我们的开发工作。今天，我们将重点介绍了一些很少见却又非常实用的PHP类、库和组件，这将有助于您提高程序功能，更重要是减少应用程序的开发时间。

<strong>一、数据库</strong>

<strong>1、ADOdb – 数据库抽象类</strong>

官网地址：<a href="http://adodb.sourceforge.net/" target="_blank">http://adodb.sourceforge.net/</a>

ADOdb是一个PHP数据库抽象类，它支持数据库包括：MySQL,、PostgreSQL、Oracle、 MS SQL、SQLite等，它基本上涵盖了目前最流行的数据库，而且完全开源和免费，可以方便快捷的应用到您的程序当中，它还具有非常强的可移植性，最重要的是它有中文使用方法！

&nbsp;

2、<strong>PHP DB Class – MySQL数据库类</strong>

官网地址：<a href="http://slaout.linux62.org/php/index.html" target="_blank">http://slaout.linux62.org/php/index.html </a>

PHP DB Class是一个方便的PHP / MySQL开发类，它非常简单和灵活，而且代码很少。它还提供了调试功能，您只需添加简单的参数，就可以查询相关数据表，以及输出调试过程中出现的错误。

&nbsp;

3、<strong>SQLCache – 缓存数据库查询结果类</strong>

下载地址：<a href="http://www.phpclasses.org/package/2646-PHP-Cache-database-query-results-in-files-.html" target="_blank">http://www.phpclasses.org/package/2646-PHP-Cache-database-query-results-in-files-.html</a>

SQLCache只有一个PHP类文件，它主要作用是缓存SQL数据库查询结果，这样做的目的是为了避免增加数据库访问压力，减少重复查询语言的执行，从而加快网站访问速度。

&nbsp;

4、<strong>IAM Backup – MySQL数据库备份和恢复类 </strong>

下载地址：<a href="http://freshmeat.net/projects/iambackup/">http://freshmeat.net/projects/iambackup/</a>

IAM Backup是一个MySQL数据库备份和恢复类，它支持gzip在线压缩文件，提高数据库备份和恢复性能。

&nbsp;

<strong>5、DataGrid – 数据库输出显示控件</strong>

下载地址：<a href="http://www.apphp.com/php-datagrid/index.php" target="_blank">http://www.apphp.com/php-datagrid/index.php</a>

DataGrid是一个使用PHP开发的数据库显示控件，它简单、新颖、功能强大，而且是专门为Web开发人员而准备的。DataGrid绑定数据库后，只需要修改数据库，就可以修改输出方式，也就是说只用修改数据，而不用管如何去显示！

&nbsp;

<strong>二、安全</strong>

<strong>1、PhpCaptcha – 生成图片验证码</strong>

下载地址：<a href="http://www.ejeliot.com/pages/2" target="_blank">http://www.ejeliot.com/pages/2</a>

PhpCaptcha可以生成图片验证码，该类需要PHP 4版本以上的GD1或2支持，还而要FreeType字体的支持。

&nbsp;

2、<strong>用户输入安全处理类</strong>

下载地址：<a href="http://codeassembly.com/How-to-sanitize-your-php-input/" target="_blank">http://codeassembly.com/How-to-sanitize-your-php-input/ </a>

一个简单实用的类，可以保证用户输入的数据是安全的，它通过检查$ _GET、$ _POST、$ _REQUEST及$ _COOKIE提交的数据，并过滤掉其中的危险字符，确保它们提交的数据符合程序要求。

&nbsp;

3、<strong>HTML Purifier</strong>

下载地址：<a href="http://www.ecisp.cn/download/htmlpurifier-4.2.0.zip">http://www.ecisp.cn/download/htmlpurifier-4.2.0.zip</a>

HTML Purifier是一个标准的HTML过滤类，使用PHP5编写。 它具有删除、验证、设置安全的白名单代码、及过滤除清恶意代码（如），它也可以验证当前HTML文件是否符合标准。

&nbsp;

4、<strong>phpAES - PHP加密类 </strong>

下载地址：<a href="http://www.ecisp.cn/download/htmlpurifier-4.2.0.zip" target="_blank">http://www.ecisp.cn/download/phpAES.zip</a>

phpAES可以实现128、192和256位AES加密，它不需要mcrypt扩展，可以用于任何PHP程序中，它使用100％的PHP开发，并完全符合FIPS 197的标准。

&nbsp;

<strong>三、图像处理</strong>

1、<strong>PHPTHUMB - PHP缩略图</strong>

下载地址：<a href="http://phpthumb.gxdlabs.com/" target="_blank">http://phpthumb.gxdlabs.com/</a>

PHPTHUBM是一个轻量级的图像处理类，它主要的功能是生成缩略图，它具有通过调整宽度和高度等比缩放图片、建立新图、剪切或旋转图像。

&nbsp;

2、<strong>WideImage- 图片处理类</strong>

下载地址：<a href="http://wideimage.sourceforge.net/demos/" target="_blank">http://wideimage.sourceforge.net/demos/</a>

WideImage是一种使用PHP5面向对像编写的图像处理类，它是一个纯PHP类，优点是不需要GD2就可以处理任何图片，该类具有常见的图像操作功能，并且简单易用。

&nbsp;

3、<strong>PHP 将文本生成图像类</strong>

下载地址：<a href="http://www.daftlogic.com/projects-text-to-image.htm" target="_blank">http://www.daftlogic.com/projects-text-to-image.htm</a>

这个类可以将文本转换成图片，比如将电子邮件地址转换成图片，或者将数字电话号码转换成图片等，这可以帮助减少您的信息被互联网非法收集。

&nbsp;

<strong>四、文件处理</strong>

1、<strong>TCPDF – 生成PDF文件</strong>

下载地址：<a href="http://www.tcpdf.org/" target="_blank">http://www.tcpdf.org/</a>

TCPDF是一个生成PDF文档的类，而且是目前互联网中唯一的生成PDF的PHP类，支持UTF - 8编码、支持双向加密PDF文件算法。

&nbsp;

2、<strong>parseCSV</strong>

下载地址：<a href="http://code.google.com/p/parsecsv-for-php/" target="_blank">http://code.google.com/p/parsecsv-for-php/</a>

parseCSV是一个用于读取CSV文件的PHP类， 它能够轻松处理CSV数据，它支持识别逗号、双引号和空格分割的数据。

&nbsp;

<strong>3、导出EXCEL文件类</strong>

下载地址：<a href="http://phpexcel.codeplex.com/" target="_blank">http://phpexcel.codeplex.com/</a>

一个轻量级的、简单而快速的PHP数据导出到Excel文件类，它支持设置EXCEL文件的标题（作者、标题、描述、...）、多个工作表、不同的字体和样式、单元格边框样式、填充、渐变等功能，还可以添加图片到电子表格等，

&nbsp;

<strong>五、图表和图形</strong>

1、<strong>XML/SWF Charts  - 图表生成类  </strong>

下载地址：<a href="http://www.maani.us/xml_charts/" target="_blank">http://www.maani.us/xml_charts/</a>

XML/SWF Charts是一个简单但功能强大图表生成工具，它能从XML文件读取生成具有吸引力的数据图，XML数据源可以使用任何语言脚本生成，如（PHP、ASP、JSP等）

&nbsp;

2、<strong>jpGraph  - 图表生成类 </strong>

下载地址：<a href="http://jpgraph.net/" target="_blank">http://jpgraph.net/</a>

JpGraph是一个使用PHP5面向对象开发的图形库，它可以生成常用的数据图表，可很容易的整合到您的PHP脚本中。

</div>
</div>
</div>