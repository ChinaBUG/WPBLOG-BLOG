---
ID: 877
post_title: ASP.NET-ServerVariables
author: ChinaBUG
post_excerpt: |
  使用ASP.NET(VB.NET)获取ServerVariables的值：如物理地址、来源网址、当前执行脚本文件名、服务器名等等。
  具体参数有：
  ALL_HTTP，ALL_RAW，APPL_MD_PATH，APPL_PHYSICAL_PATH，AUTH_PASSWORD，AUTH_TYPE，AUTH_USER，CERT_COOKIE，CERT_FLAGS，CERT_ISSUER，CERT_KEYSIZE，CERT_SECRETKEYSIZE，CERT_SERIALNUMBER，CERT_SERVER_ISSUER，CERT_SERVER_SUBJECT，CERT_SUBJECT，CONTENT_LENGTH，CONTENT_TYPE，GATEWAY_INTERFACE，HTTP_<HeaderName>，HTTPS，HTTPS_KEYSIZE，HTTPS_SECRETKEYSIZE，HTTPS_SERVER_ISSUER，HTTPS_SERVER_SUBJECT，INSTANCE_ID，INSTANCE_META_PATH，LOCAL_ADDR，LOGON_USER，PATH_INFO，PATH_TRANSLATED，QUERY_STRING，REMOTE_ADDR，REMOTE_HOST，REMOTE_USER，REQUEST_METHOD，SCRIPT_NAME，SERVER_NAME，SERVER_PORT，SERVER_PORT_SECURE，SERVER_PROTOCOL，SERVER_SOFTWARE，URL
layout: post
permalink: >
  http://blog.ipodmp.com/2011/01/asp-net-servervariables.html
published: true
post_date: 2011-01-13 10:22:37
---
<h2>ServerVariables</h2>
<strong>ServerVariables </strong>集合检索预定的环境变量。
<h4>语法</h4>
<pre><strong>Request.ServerVariables (</strong><em>server environment</em> <em>variable</em><strong>)
 </strong></pre>
<h4>参数</h4>
<dl><dt><em>服务器环境变量</em> </dt><dd>指定要检索的服务器环境变量名。可以使用下面列出的值。
<table>
<tbody>
<tr valign="top">
<td><strong>变量</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr valign="top">
<td>ALL_HTTP</td>
<td>客户端发送的所有 HTTP 标题文件。</td>
</tr>
<tr valign="top">
<td>ALL_RAW</td>
<td>检索未处理表格中所有的标题。ALL_RAW 和 ALL_HTTP 不同，ALL_HTTP 在标题文件名前面放置 HTTP_ prefix，并且标题名称总是大写的。使用 ALL_RAW 时，标题名称和值只在客户端发送时才出现。</td>
</tr>
<tr valign="top">
<td>APPL_MD_PATH</td>
<td>检索 ISAPI DLL 的 (WAM) Application 的元数据库路径。</td>
</tr>
<tr valign="top">
<td>APPL_PHYSICAL_PATH</td>
<td>检索与元数据库路径相应的物理路径。IIS 通过将 APPL_MD_PATH 转换为物理（目录）路径以返回值。</td>
</tr>
<tr valign="top">
<td>AUTH_PASSWORD</td>
<td>该值输入到客户端的鉴定对话中。只有使用基本鉴定时，该变量才可用。</td>
</tr>
<tr valign="top">
<td>AUTH_TYPE</td>
<td>这是用户访问受保护的脚本时，服务器用于检验用户的验证方法。</td>
</tr>
<tr valign="top">
<td>AUTH_USER</td>
<td>未被鉴定的用户名。</td>
</tr>
<tr valign="top">
<td>CERT_COOKIE</td>
<td>客户端验证的唯一 ID，以字符串方式返回。可作为整个客户端验证的签字。</td>
</tr>
<tr valign="top">
<td>CERT_FLAGS</td>
<td>如有客户端验证，则 bit0 为 1。如果客户端验证的验证人无效（不在服务器承认的 CA 列表中），bit1 被设置为 1。</td>
</tr>
<tr valign="top">
<td>CERT_ISSUER</td>
<td>用户验证中的颁布者字段（O=MS，OU=IAS，CN=user name，C=USA）。</td>
</tr>
<tr valign="top">
<td>CERT_KEYSIZE</td>
<td>安全套接字层连接关键字的位数，如 128。</td>
</tr>
<tr valign="top">
<td>CERT_SECRETKEYSIZE</td>
<td>服务器验证私人关键字的位数。如 1024。</td>
</tr>
<tr valign="top">
<td>CERT_SERIALNUMBER</td>
<td>用户验证的序列号字段。</td>
</tr>
<tr valign="top">
<td>CERT_SERVER_ISSUER</td>
<td>服务器验证的颁发者字段。</td>
</tr>
<tr valign="top">
<td>CERT_SERVER_SUBJECT</td>
<td>服务器验证的主字段。</td>
</tr>
<tr valign="top">
<td>CERT_SUBJECT</td>
<td>客户端验证的主字段。</td>
</tr>
<tr valign="top">
<td>CONTENT_LENGTH</td>
<td>客户端发出内容的长度。</td>
</tr>
<tr valign="top">
<td>CONTENT_TYPE</td>
<td>内容的数据类型。同附加信息的查询一起使用，如 HTTP 查询 GET、 POST <strong> </strong>和 PUT。</td>
</tr>
<tr valign="top">
<td>GATEWAY_INTERFACE</td>
<td>服务器使用的 CGI 规格的修订。格式为 CGI/revision。</td>
</tr>
<tr valign="top">
<td>HTTP_&lt;<em>HeaderName</em>&gt;</td>
<td><em>HeaderName</em> 存储在标题文件中的值。未列入该表的标题文件必须以 HTTP_ 作为前缀，以使 <strong>ServerVariables</strong> 集合检索其值。<strong>注意</strong> 服务器将 <em>HeaderName</em> 中的下划线（_）解释为实际标题中的破折号。例如，如果您指定 HTTP_MY_HEADER，服务器将搜索以 MY-HEADER 为名发送的标题文件。</td>
</tr>
<tr valign="top">
<td>HTTPS</td>
<td>如果请求穿过安全通道（SSL），则返回 ON。如果请求来自非安全通道，则返回 OFF。</td>
</tr>
<tr valign="top">
<td>HTTPS_KEYSIZE</td>
<td>安全套接字层连接关键字的位数，如 128。</td>
</tr>
<tr valign="top">
<td>HTTPS_SECRETKEYSIZE</td>
<td>服务器验证私人关键字的位数。如 1024。</td>
</tr>
<tr valign="top">
<td>HTTPS_SERVER_ISSUER</td>
<td>服务器验证的颁发者字段。</td>
</tr>
<tr valign="top">
<td>HTTPS_SERVER_SUBJECT</td>
<td>服务器验证的主字段。</td>
</tr>
<tr valign="top">
<td>INSTANCE_ID</td>
<td>文本格式 IIS 实例的 ID。如果实例 ID 为 1，则以字符形式出现。使用该变量可以检索请求所属的（元数据库中）Web 服务器实例的 ID。</td>
</tr>
<tr valign="top">
<td>INSTANCE_META_PATH</td>
<td>响应请求的 IIS 实例的元数据库路径。</td>
</tr>
<tr valign="top">
<td>LOCAL_ADDR</td>
<td>返回接受请求的服务器地址。如果在绑定多个 IP 地址的多宿主机器上查找请求所使用的地址时，这条变量非常重要。</td>
</tr>
<tr valign="top">
<td>LOGON_USER</td>
<td>用户登录 Windows NT® 的帐号。</td>
</tr>
<tr valign="top">
<td>PATH_INFO</td>
<td>客户端提供的额外路径信息。可以使用这些虚拟路径和 PATH_INFO 服务器变量访问脚本。如果该信息来自 URL，在到达 CGI 脚本前就已经由服务器解码了。</td>
</tr>
<tr valign="top">
<td>PATH_TRANSLATED</td>
<td>PATH_INFO 转换后的版本，该变量获取路径并进行必要的由虚拟至物理的映射。</td>
</tr>
<tr valign="top">
<td>QUERY_STRING</td>
<td>查询 HTTP 请求中问号（?）后的信息。</td>
</tr>
<tr valign="top">
<td>REMOTE_ADDR</td>
<td>发出请求的远程主机的 IP 地址。</td>
</tr>
<tr valign="top">
<td>REMOTE_HOST</td>
<td>发出请求的主机名称。如果服务器无此信息，它将设置为空的 MOTE_ADDR 变量。</td>
</tr>
<tr valign="top">
<td>REMOTE_USER</td>
<td>用户发送的未映射的用户名字符串。该名称是用户实际发送的名称，与服务器上验证过滤器修改过后的名称相对。</td>
</tr>
<tr valign="top">
<td>REQUEST_METHOD</td>
<td>该方法用于提出请求。相当于用于 HTTP 的 GET、HEAD、POST 等等。</td>
</tr>
<tr valign="top">
<td>SCRIPT_NAME</td>
<td>执行脚本的虚拟路径。用于自引用的 URL。</td>
</tr>
<tr valign="top">
<td>SERVER_NAME</td>
<td>出现在自引用 UAL 中的服务器主机名、DNS 化名或 IP 地址。</td>
</tr>
<tr valign="top">
<td>SERVER_PORT</td>
<td>发送请求的端口号。</td>
</tr>
<tr valign="top">
<td>SERVER_PORT_SECURE</td>
<td>包含 0 或 1 的字符串。如果安全端口处理了请求，则为 1，否则为 0。</td>
</tr>
<tr valign="top">
<td>SERVER_PROTOCOL</td>
<td>请求信息协议的名称和修订。格式为 <em>protocol</em>/<em>revision</em> 。</td>
</tr>
<tr valign="top">
<td>SERVER_SOFTWARE</td>
<td>应答请求并运行网关的服务器软件的名称和版本。格式为 <em>name</em>/<em>version</em> 。</td>
</tr>
<tr valign="top">
<td>URL</td>
<td>提供 URL 的基本部分。</td>
</tr>
</tbody>
</table>
</dd></dl>====　分割线　====

&lt;SCRIPT LANGUAGE="vb" runat="server"&gt;
Public Sub AllServerVariables()
  Dim intCtr as integer
  Dim coll As System.Collections.Specialized.NameValueCollection
  coll = System.Web.HttpContext.Current.Request.ServerVariables
  For intCtr = 0 To coll.keys.count - 1
   Response.Write(coll.keys(intCtr)  &amp; ": ")
   Response.Write(coll.item(intCtr))
   response.write("&lt;BR&gt;")
  Next
End Sub

'Request one by name
Public Function GetServerVariable(VariableName as string) as string
 return Request.ServerVariables(VariableName)
End Function
'You may prefer to just focus on a few of interest
'by hard coding their names in a function such as below
Public Function ApplicationPath as string
 return Request.ServerVariables("APPL_PHYSICAL_PATH")
End Function
Public Function LoggedOnUser as string
 return Request.ServerVariables("LOGON_USER")
End Function
Public Function CurrentPage() as string
 return Request.ServerVariables("SCRIPT_NAME")
End Function
&lt;/SCRIPT&gt;
&lt;%
'demo
response.write("&lt;B&gt;All Variables:&lt;/B&gt;&lt;BR&gt;")
AllServerVariables()
response.write("&lt;P&gt;")
response.write("&lt;b&gt;requesting full path to page by name of variable&lt;/b&gt;&lt;BR&gt;")
response.write(GetServerVariable("PATH_TRANSLATED"))
response.write("&lt;P&gt;")
response.write("&lt;B&gt;Requesting Current Page via user-defined function:&lt;/B&gt;&lt;BR&gt;")
response.write(CurrentPage)
%&gt;