---
ID: 618
post_title: API-GoogleMAP API应用例子
author: ChinaBUG
post_excerpt: ""
layout: page
permalink: http://blog.ipodmp.com/google-map-api
published: true
post_date: 2010-05-30 15:51:01
---
<h1>完整的GoogleMAP API应用例子</h1>
&lt;script src="<a href="http://www.google.cn/maps?file=api&amp;v=2&amp;key=ABQIAAAABVjJOIYRK5jV3SfeHJAL_hRQIUnFeUFOP7u5SIHePa_6gSiywRQEjgpkgxlCWO58ipC9PKUrIf5d3w&amp;hl=zh-CN">http://www.google.cn/maps?file=api&amp;v=2&amp;key=ABQIAAAABVjJOIYRK5jV3SfeHJAL_hRQIUnFeUFOP7u5SIHePa_6gSiywRQEjgpkgxlCWO58ipC9PKUrIf5d3w&amp;hl=zh-CN</a>" type="text/javascript"&gt;&lt;/script&gt;&lt;script type="text/javascript"&gt;
function initialize() {
if (GBrowserIsCompatible()) {
var map = new GMap2(document.getElementById("map"));
var center = new GLatLng(24.492577131620997,118.12789678573608);
map.addControl(new GSmallMapControl());
map.setCenter(center, 13);
SHtml = "&lt;span style='font-size:12px;'&gt;乐屋雅蒂厦门总代&lt;br/&gt;地址：厦门市嘉禾路福隆国际300号3001室&lt;br/&gt;联系电话：&lt;strong&gt;0592－5232900&lt;/strong&gt;&lt;/span&gt;"
map.openInfoWindow(center, SHtml,{noCloseOnClick:true});
var marker = new GMarker(center, {draggable: false});
map.addOverlay(marker);
}
}
window.onload=function(){
initialize();
}
&lt;/script&gt;&lt;/p&gt;
&lt;div id="map" style="width: 700px; height: 350px"&gt;&amp;nbsp;&lt;/div&gt;

----------------------------------------------
<h1>要如何得出一个地点的GLatLng值？</h1>
用下面的代码，点击需要的地点就会显示该地点的GLatLng值。例子来源 <a href="http://code.google.com/intl/zh-CN/apis/maps/documentation/javascript/v2/examples/event-arguments.html">Google 地图 JavaScript API 示例: 事件参数</a>

&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
"<a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd</a>"&gt;
&lt;html xmlns="<a href="http://www.w3.org/1999/xhtml">http://www.w3.org/1999/xhtml</a>" xmlns:v="urn:schemas-microsoft-com:vml"&gt;
&lt;head&gt;
&lt;meta http-equiv="content-type" content="text/html; charset=utf-8"/&gt;
&lt;title&gt;Google 地图 JavaScript API 示例: 事件参数&lt;/title&gt;
&lt;script src="<a href="http://www.google.cn/maps?file=api&amp;amp;v=2&amp;amp;key=ABQIAAAAzr2EBOXUKnm_jVnk0OJI7xSosDVG8KKPE1-m51RBrvYughuyMxQ-i1QfUnH94QxWIa6N4U6MouMmBA&amp;hl=zh-CN">http://www.google.cn/maps?file=api&amp;amp;v=2&amp;amp;key=ABQIAAAAzr2EBOXUKnm_jVnk0OJI7xSosDVG8KKPE1-m51RBrvYughuyMxQ-i1QfUnH94QxWIa6N4U6MouMmBA&amp;hl=zh-CN</a>"
type="text/javascript"&gt;&lt;/script&gt;
&lt;script type="text/javascript"&gt;


function initialize() {
if (GBrowserIsCompatible()) {
var map = new GMap2(document.getElementById("map_canvas"));
map.setCenter(new GLatLng(39.917,116.397), 13);

GEvent.addListener(map,"click", function(overlay,latlng) {
var myHtml = "GLatLng 为： " + latlng + ",&lt;br&gt;GPoint 为： " + map.fromLatLngToDivPixel(latlng) + "，&lt;br&gt;缩放级别为：" + map.getZoom();
map.openInfoWindow(latlng, myHtml);
});
map.addControl(new GSmallMapControl());
map.addControl(new GMapTypeControl());
}
}

&lt;/script&gt;
&lt;/head&gt;
&lt;body onload="initialize()" onunload="GUnload()"&gt;
&lt;div id="map_canvas" style="width: 970px; height: 680px"&gt;&lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;

----------------------------------------------
<h1>其他的一些应用噢</h1>
map.addControl(new GSmallMapControl());
map.addControl(new GMapTypeControl());
map.addControl(new GOverviewMapControl(new GSize(100,100)));

// JavaScript Document
var map = null;

function createMarker(point, pointName, pointURL) {
var marker = new GMarker(point);
GEvent.addListener(marker, "click", function() {
marker.openInfoWindowHtml("&lt;div class=\"marker\"&gt;&lt;h3&gt;" + pointName + "&lt;/h3&gt;&lt;p&gt;&lt;a href=\"/" + pointURL + "/\" target=\"_parent\"&gt;View more about this region&lt;/a&gt;&lt;/p&gt;&lt;/div&gt;");
});
return marker;
}

function load() {
if (GBrowserIsCompatible()) {
map = new GMap2(document.getElementById("map_canvas"));
map.addControl(new GSmallMapControl());
map.addControl(new GMapTypeControl());
map.addControl(new GOverviewMapControl(new GSize(100,100)));
map.setCenter(new GLatLng(35.843715, -86.167054), 6);
GDownloadUrl("/xml/map.xml", function(data) {
var xml = GXml.parse(data);
var markers = xml.documentElement.getElementsByTagName("marker");
for (var i = 0; i &lt; markers.length; i++) {
var point = new GLatLng(parseFloat(markers[i].getAttribute("lat")),
parseFloat(markers[i].getAttribute("lng")));

map.addOverlay(createMarker(point,markers[i].getAttribute("pointName"),markers[i].getAttribute("pointURL")));

}
});
}
}

=====
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;markers&gt;
&lt;marker lat="35.903234" lng="-84.499283" pointName="East Tennessee" pointURL="east"/&gt;
&lt;marker lat="35.677936" lng="-87.002106" pointName="Middle Tennessee" pointURL="middle"/&gt;
&lt;marker lat="35.676263" lng="-88.786011" pointName="West Tennessee" pointURL="west"/&gt;
&lt;/markers&gt;
=====
<a href="http://tnvacation.com/new_08/map.html">http://tnvacation.com/new_08/map.html</a>

=====

参考网站：

1、<a href="http://www.nowamagic.net/projects/projects.php">简明现代魔法--软件项目</a>