---
ID: 3421
post_title: 'SharePoint-调用客户端组件时远程服务器返回错误: (400) 错误的请求'
author: ChinaBUG
post_excerpt: |
  “/call3”应用程序中的服务器错误。
  远程服务器返回错误: (400) 错误的请求。
layout: post
permalink: >
  http://blog.ipodmp.com/2014/06/sharepoint-when-the-client-component-is-called-the-remote-server-returned-an-error-400-bad-request.html
published: true
post_date: 2014-06-30 10:51:46
---
<h1>“/call3”应用程序中的服务器错误。</h1>

<hr size="1" width="100%" />

<h2><i>远程服务器返回错误: (400) 错误的请求。</i></h2>
<span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><span style="font-family: Arial, Helvetica, Geneva, SunSans-Regular, sans-serif;"><b> 说明: </b>执行当前 Web 请求期间，出现未经处理的异常。请检查堆栈跟踪信息，以了解有关该错误以及代码中导致错误的出处的详细信息。
<b> 异常详细信息: </b>System.Net.WebException: 远程服务器返回错误: (400) 错误的请求。
<b>源错误:</b></span></span>
<table width="100%" bgcolor="#ffffcc">
<tbody>
<tr>
<td>
<pre>行 269:                 */
行 270:                //throw new Exception(AcsMetadataParser.GetStsUrl(targetRealm));
<span style="color: red;">行 271:                oauth2Response = client.Issue(AcsMetadataParser.GetStsUrl(targetRealm), oauth2Request) as OAuth2AccessTokenResponse;
</span>行 272:            }
行 273:            catch (WebException wex)</pre>
</td>
</tr>
</tbody>
</table>
&nbsp;

之前在开发ShraePoint2013的<a title="OAUTH2信任登录OFFICE365微软账号直连登录SharePonit2013程序 " href="http://item.taobao.com/item.htm?id=38693204265" target="_blank">信任授权登录</a>的时候使用客户端组件调用的时候死活是出现这个错误，结果，经过排查之所以会出现这个错误原因是<span style="color: red;">targetRealm</span>参数为空呀，汗一个~~