---
ID: 2341
post_title: >
  FLASH-在Flash里面获取与设置客户端的小馅饼(cookie)
author: ChinaBUG
post_excerpt: |
  HTTPCookies.as:
  import flash.external.ExternalInterface;
  public class HTTPCookies
  {
  public static function getCookie(key:String):*
  {
  return ExternalInterface.call("getCookie", key);
  }
  
  public static function setCookie(key:String, val:*):void
  {
  ExternalInterface.call("setCookie", key, val);
  }
  }
layout: post
permalink: >
  http://blog.ipodmp.com/2013/02/flash-get-flash-inside-and-set-up-a-client-patties-cookie.html
published: true
post_date: 2013-02-17 15:39:49
---
今天在开发游戏的时候需要用到这个，找了一下还真的有，记录一下吧！

请在flash里面加载下面的代码：

HTTPCookies.as:
<pre><code>import flash.external.ExternalInterface;

public class HTTPCookies
{
    public static function getCookie(key:String):*
    {
        return ExternalInterface.call("getCookie", key);
    }

    public static function setCookie(key:String, val:*):void
    {
        ExternalInterface.call("setCookie", key, val);
    }
}

请在客户端调用flash文件的页面内增加下面的代码：
</code></pre>
HTTPCookies.js:

function getCookie(key){
var cookieValue = null;
if (key){
var cookieSearch = key + "=";
if (document.cookie){
var cookieArray = document.cookie.split(";");
for (var i = 0; i &lt; cookieArray.length; i++){
var cookieString = cookieArray[i];
// skip past leading spaces
while (cookieString.charAt(0) == ' '){
cookieString = cookieString.substr(1);
}
// extract the actual value
if (cookieString.indexOf(cookieSearch) == 0){
cookieValue = cookieString.substr(cookieSearch.length);
}
}
}
}
return cookieValue;
}
function setCookie(key, val){
if (key){
var date = new Date();
if (val != null){
// expires in one year
date.setTime(date.getTime() + (365*24*60*60*1000));
document.cookie = key + "=" + val + "; expires=" + date.toGMTString();
}else{
// expires yesterday
date.setTime(date.getTime() - (24*60*60*1000));
document.cookie = key + "=; expires=" + date.toGMTString();
}
}
}

然后你就可以在flash文档里面使用getCookie和setCookie了。

附录：
<h2>Instructions</h2>
<section>
<ol id="intelliTxt">
	<li>
<ul>
	<li>1
<div>
<div itemprop="step">

Open the ActionScript editor and open the Web project for your website. Double-click the coding file you want to use to read and display the cookie.

</div>
</div></li>
	<li>2
<div>
<div itemprop="step">

Read the data from the cookie. The following code reads a cookie named "mycookie" from the user's browser:

cookie = SharedObject.getLocal("mycookie");

</div>
</div></li>
	<li>
<div id="DMINSTR" data-type="adTracking"></div></li>
	<li>3
<div>
<div itemprop="step">

Display the cookie's information on the Web page. You display the content using one of your ActionScript text boxes. The following code writes the cookie data to a text box named "showstring":

showstring.text = cookie;

</div>
</div></li>
</ul>
</li>
</ol>
</section>
<div>
Read more: <a href="http://www.ehow.com/how_12136779_read-display-php-cookie-value-as3.html#ixzz2L8oT28CM">How to Read &amp; Display a PHP Cookie Value in AS3 | eHow.com</a> <a href="http://www.ehow.com/how_12136779_read-display-php-cookie-value-as3.html#ixzz2L8oT28CM">http://www.ehow.com/how_12136779_read-display-php-cookie-value-as3.html#ixzz2L8oT28CM</a></div>