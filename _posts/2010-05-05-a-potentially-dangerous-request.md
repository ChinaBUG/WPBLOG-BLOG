---
ID: 504
post_title: A potentially dangerous Request
author: ChinaBUG
post_excerpt: >
  A potentially dangerous Request.Form
  value was detected from the client
layout: post
permalink: >
  http://blog.ipodmp.com/2010/05/a-potentially-dangerous-request.html
published: true
post_date: 2010-05-05 14:10:53
---
　　自从安装了VS2010正式版以来，程序出现的问题还真多哈~

　　今天，出现这个鸟错误，恩，我在该页已经设置 validateRequest="false"了，可还是不行。

　　根据<a href="http://www.asp.net/learn/whitepapers/request-validation/" target="_blank">ASP.NET</a>说明来做还是不行。百思不得其解，忽然发现

<strong>　　Version Information:</strong> Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.0.30319.1

　　是最新版的~~我记得原先升级之前是2.0的吧？！

　　不管，随便，还原了再说噢~结果，问题解决了，真是很神奇的一刻。

　　以后，不能让系统智能来决定一切哈~

附录：

错误信息如下：
<h1>Server Error in '/' Application.
<hr size="1" /></h1>
<h2><em>A potentially dangerous Request.Form value was detected from the client (GridView1$ctl02$TextBox1=" &lt;item item_url="../u...").</em></h2>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><strong>Description: </strong>Request Validation has detected a potentially dangerous client input value, and processing of the request has been aborted. This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack. To allow pages to override application request validation settings, set the requestValidationMode attribute in the httpRuntime configuration section to requestValidationMode="2.0". Example: &lt;httpRuntime requestValidationMode="2.0" /&gt;. After setting this value, you can then disable request validation by setting validateRequest="false" in the Page directive or in the &lt;pages&gt; configuration section. However, it is strongly recommended that your application explicitly check all inputs in this case. For more information, see http://go.microsoft.com/fwlink/?LinkId=153133.</span></div>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><strong>Exception Details: </strong>System.Web.HttpRequestValidationException: A potentially dangerous Request.Form value was detected from the client (GridView1$ctl02$TextBox1=" &lt;item item_url="../u...").

<strong>Source Error:</strong>

</span>
<table width="100%" bgcolor="#ffffcc">
<tbody>
<tr>
<td><code>The source code that generated this unhandled exception can only be shown when compiled in debug mode. To enable this, please follow one of the below steps, then request the URL:1. Add a "Debug=true" directive at the top of the file that generated the error. Example:

  &lt;%@ Page Language="C#" Debug="true" %&gt;

or:

2) Add the following section to the configuration file of your application:

&lt;configuration&gt;
   &lt;system.web&gt;
       &lt;compilation debug="true"/&gt;
   &lt;/system.web&gt;
&lt;/configuration&gt;

Note that this second technique will cause all files within a given application to be compiled in debug mode. The first technique will cause only that particular file to be compiled in debug mode.

Important: Running applications in debug mode does incur a memory/performance overhead. You should make sure that an application has debugging disabled before deploying into production scenario.

</code></td>
</tr>
</tbody>
</table>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;">
<strong>Stack Trace:</strong></span></div>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"> 

</span>
<table width="100%" bgcolor="#ffffcc">
<tbody>
<tr>
<td><code>
<pre>[HttpRequestValidationException (0x80004005): A potentially dangerous Request.Form value was detected from the client (GridView1$ctl02$TextBox1="	&lt;item item_url="../u...").]
   System.Web.HttpRequest.ValidateString(String value, String collectionKey, RequestValidationSource requestCollection) +8730740
   System.Web.HttpRequest.ValidateNameValueCollection(NameValueCollection nvc, RequestValidationSource requestCollection) +122
   System.Web.HttpRequest.get_Form() +114
   System.Web.HttpRequest.get_HasForm() +8896111
   System.Web.UI.Page.GetCollectionBasedOnMethod(Boolean dontReturnNull) +97
   System.Web.UI.Page.DeterminePostBackMode() +69
   System.Web.UI.Page.ProcessRequestMain(Boolean includeStagesBeforeAsyncPoint, Boolean includeStagesAfterAsyncPoint) +8431
   System.Web.UI.Page.ProcessRequest(Boolean includeStagesBeforeAsyncPoint, Boolean includeStagesAfterAsyncPoint) +253
   System.Web.UI.Page.ProcessRequest() +78
   System.Web.UI.Page.ProcessRequestWithNoAssert(HttpContext context) +21
   System.Web.UI.Page.ProcessRequest(HttpContext context) +49
   ASP.admin_admin_config_ads_aspx.ProcessRequest(HttpContext context) +37
   System.Web.CallHandlerExecutionStep.System.Web.HttpApplication.IExecutionStep.Execute() +100
   System.Web.HttpApplication.ExecuteStep(IExecutionStep step, Boolean&amp; completedSynchronously) +75</pre>
 

</code></td>
</tr>
</tbody>
</table>
<div><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"></span></div>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><hr size="1" /><strong>Version Information:</strong> Microsoft .NET Framework Version:4.0.30319; ASP.NET Version:4.0.30319.1 </span>