---
ID: 892
post_title: jQuery Tutorials for Designers
author: ChinaBUG
post_excerpt: |
  这个jQuery教程是我看到的教程中比较简单而且效果显著的，你可以不用看很多基础知识，却知道这样的效果要怎样做。简单高效噢。
  本文来自 WebDesignerWall.com 这个网站里面的内容非常的好，推荐有需要的经常去看噢。
  ...
  How to get the element?
  Writing jQuery function is relatively easy (thanks to the wonderful documentation). The key point you have to learn is how to get the exact element that you want to apply the effects.
  
  $("#header") = get the element with id="header"
  $("h3") = get all &lt;h3&gt; element
  $("div#content .photo") = get all element with nested in the &lt;div id="content"&gt;
  $("ul li") = get all &lt;li&gt; element nested in all &lt;ul&gt;
  $("ul li:first") = get only the first &lt;li&gt; element of the &lt;ul&gt;
layout: post
permalink: >
  http://blog.ipodmp.com/2011/02/jquery-tutorials-for-designers.html
published: true
post_date: 2011-02-28 14:17:37
---
虫曰：
这个jQuery教程是我看到的教程中比较简单而且效果显著的，你可以不用看很多基础知识，却知道这样的效果要怎样做。简单高效噢。

本文来自 <a href="http://www.webdesignerwall.com/tutorials/jquery-tutorials-for-designers/">WebDesignerWall.com</a> 这个网站里面的内容非常的好，推荐有需要的经常去看噢。
<h3>How jQuery works?</h3>
First you need to download a copy of <a href="http://jquery.com/">jQuery</a> and insert it in your html page (preferably within the <code>&lt;head&gt;</code> tag). Then you need to write functions to tell jQuery what to do. The diagram below explains the detail how jQuery work:

<a href="http://www.webdesignerwall.com/wp-content/uploads/2008/02/jquery-how-it-works.gif"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/jquery-how-it-works-sm.gif" alt="how jquery works" /></a>
<h3>How to get the element?</h3>
Writing jQuery function is relatively easy (thanks to the wonderful <a href="http://docs.jquery.com/Main_Page">documentation</a>). The key point you have to learn is how to get the exact element that you want to apply the effects.
<ul>
	<li><code>$("#header")</code> = get the element with id="header"</li>
	<li><code>$("h3")</code> = get all &lt;h3&gt; element</li>
	<li><code>$("div#content .photo")</code> = get all element with nested in the &lt;div id="content"&gt;</li>
	<li><code>$("ul li")</code> = get all &lt;li&gt; element nested in all &lt;ul&gt;</li>
	<li><code>$("ul li:first")</code> = get only the first &lt;li&gt; element of the &lt;ul&gt;</li>
</ul>
<h3><em>1.</em> Simple slide panel</h3>
Let’s start by doing a simple slide panel. You’ve probably seen a lot of this, where you click on a link and a panel slide up/down. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/simple-slide-panel.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/simple-slide-panel.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample1.gif" alt="sample" /></a>

When an elment with is clicked, it will slideToggle (up/down) the &lt;div id="panel"&gt; element and then toggle a CSS to the &lt;a&gt; element. The .active class will toggle the background position of the arrow image (by CSS).
<pre><code>$(document).ready(function(){

	$(".btn-slide").click(function(){
	  $("#panel").slideToggle("slow");
	  $(this).toggleClass("active");
	});

});</code></pre>
<h3><em>2.</em> Simple disappearing effect</h3>
This sample will show you how to make something disappear when an image button is clicked. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/simple-disappear.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/simple-disappear.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample2.gif" alt="sample" /></a>

When the &lt;img&gt; is clicked, it will find its parent element &lt;div&gt; and animate its opacity=hide with slow speed.
<pre><code>$(document).ready(function(){

	$(".pane .delete").click(function(){
	  $(this).parents(".pane").animate({ opacity: "hide" }, "slow");
	});

});</code></pre>
<h3><em>3</em> Chain-able transition effects</h3>
Now let’s see the power of jQuery’s chainability. With just several lines of code, I can make the box fly around with scaling and fading transition. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/chainable-effects.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/chainable-effects.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/multi-animation.gif" alt="sample" /></a>

<strong>Line 1</strong>: when the &lt;a&gt; is clicked
<strong>Line 2</strong>: animate the &lt;div id="box"&gt; opacity=0.1, left property until it reaches 400px, with speed 1200 (milliseconds)
<strong>Line 3</strong>: then opacity=0.4, top=160px, height=20, width=20, with speed "slow"
<strong>Line 4</strong>: then opacity=1, left=0, height=100, width=100, with speed "slow"
<strong>Line 5</strong>: then opacity=1, left=0, height=100, width=100, with speed "slow"
<strong>Line 6</strong>: then top=0, with speed "fast"
<strong>Line 7</strong>: then slideUp (default speed = "normal")
<strong>Line 8</strong>: then slideDown, with speed "slow"
<strong>Line 9</strong>: return false will prevent the browser jump to the link anchor
<pre><code>$(document).ready(function(){

	$(".run").click(function(){

	  $("#box").animate({opacity: "0.1", left: "+=400"}, 1200)
	  .animate({opacity: "0.4", top: "+=160", height: "20", width: "20"}, "slow")
	  .animate({opacity: "1", left: "0", height: "100", width: "100"}, "slow")
	  .animate({top: "0"}, "fast")
	  .slideUp()
	  .slideDown("slow")
	  return false;

	});

});</code></pre>
<h3><em>4a.</em> Accordion #1</h3>
Here is a sample of accordion. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/accordion1.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/accordion1.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample3.gif" alt="sample" /></a>

The first line will add a CSS class "active" to the first &lt;H3&gt; element within the &lt;div&gt; (the "active" class will shift the background position of the arrow icon). The second line will hide all the &lt;p&gt; element that is not the first within the &lt;div&gt;.

When the &lt;h3&gt; element is clicked, it will slideToggle the next &lt;p&gt; and slideUp all its siblings, then toggle the.
<pre><code>$(document).ready(function(){

	$(".accordion h3:first").addClass("active");
	$(".accordion p:not(:first)").hide();

	$(".accordion h3").click(function(){

	  $(this).next("p").slideToggle("slow")
	  .siblings("p:visible").slideUp("slow");
	  $(this).toggleClass("active");
	  $(this).siblings("h3").removeClass("active");

	});

});</code></pre>
<h3><em>4b.</em> Accordion #2</h3>
This example is very similar to accordion#1, but it will let you specify which panel to open as default. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/accordion2.html">demo</a>)</em>

In the CSS stylesheet, set <code>.accordion p</code> to <code>display:none</code>. Now suppose you want to open the third panel as default. You can write as <code>$(".accordion2 p").eq(2).show();</code> (eq = equal). Note that the indexing starts at zero.
<pre><code>$(document).ready(function(){

	$(".accordion2 h3").eq(2).addClass("active");
	$(".accordion2 p").eq(2).show();

	$(".accordion2 h3").click(function(){
	  $(this).next("p").slideToggle("slow")
	  .siblings("p:visible").slideUp("slow");
	  $(this).toggleClass("active");
	  $(this).siblings("h3").removeClass("active");
	});

});</code></pre>
<h3><em>5a.</em> Animated hover effect #1</h3>
This example will create a nice animated hover effect with fade in/out. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/animated-hover1.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/animated-hover1.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample4.gif" alt="sample" /></a>

When the menu link is mouseovered, it will find the next &lt;em&gt; and animate its opacity and top position.
<pre><code>$(document).ready(function(){

	$(".menu a").hover(function() {
	  $(this).next("em").animate({opacity: "show", top: "-75"}, "slow");
	}, function() {
	  $(this).next("em").animate({opacity: "hide", top: "-85"}, "fast");
	});

});</code></pre>
<h3><em>5b.</em> Animated hover effect #2</h3>
This example will get the menu link<code>title</code> attribute, store it in a variable, and then append to the &lt;em&gt; tag. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/animated-hover2.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/animated-hover2.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample4b.gif" alt="sample" /></a>

The first line will append an empty <code>&lt;em&gt;</code> to the menu <code>&lt;a&gt;</code> element.

When the link is mouseovered, it will get the<code>title</code> attribute, store it in a variable "hoverText", and then set the <code>&lt;em&gt;</code> text content with the hoverText’s value.
<pre><code>$(document).ready(function(){

	$(".menu2 a").append("&lt;em&gt;&lt;/em&gt;");

	$(".menu2 a").hover(function() {
	  $(this).find("em").animate({opacity: "show", top: "-75"}, "slow");
	  var hoverText = $(this).attr("title");
	  $(this).find("em").text(hoverText);
	}, function() {
	  $(this).find("em").animate({opacity: "hide", top: "-85"}, "fast");
	});

});</code></pre>
<h3><em>6.</em> Entire block clickable</h3>
This example will show you how to make the entire block element clickable as seen on my <a href="http://bestwebgallery.com/">Best Web Gallery</a>’s sidebar tabs. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/block-clickable.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/block-clickable.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample5.gif" alt="sample" /></a>

Suppose you have a <code>&lt;ul&gt;</code> list with and you want to make the nested <code>&lt;li&gt;</code> clickable (entire block). You can assign the click function to ".pane-list li"; and when it is clicked, the function will find the <code>&lt;a&gt;</code> element and redirect the browser location to its <code>href</code> attribute value.
<pre><code>$(document).ready(function(){

	$(".pane-list li").click(function(){
	  window.location=$(this).find("a").attr("href"); return false;
	});

});</code></pre>
<h3><em>7.</em> Collapsible panels</h3>
Let’s combine the techniques from the previous examples and create a serie of collapsible panels (similar to the Gmail inbox panels). Notice I also used the same technique on Web Designer Wall comment list and <a href="http://www.next2friends.com/">Next2Friends</a> message inbox? <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/collapsible-panels.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/collapsible-panels.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample6.gif" alt="sample" /></a>

<strong>First line</strong>: hide all &lt;div&gt; after the first one.
<strong>Second line</strong>: hide all &lt;li&gt; element after the 5th
<strong>Third part</strong>: when the &lt;p&gt; is clicked, slideToggle the next &lt;div&gt;
<strong>Fourth part</strong>: when the &lt;a&gt; button is clicked, slideUp all &lt;div&gt;
<strong>Fifth part</strong>: when the &lt;a&gt; is clicked, hide this, show &lt;a&gt;, and slideDown all &lt;li&gt; after the fifth one.
<strong>Sixth part</strong>: when the &lt;a&gt; is clicked, hide this, show &lt;a&gt;, and slideUp all &lt;li&gt; after the 5th.
<pre><code>$(document).ready(function(){

	//hide message_body after the first one
	$(".message_list .message_body:gt(0)").hide();

	//hide message li after the 5th
	$(".message_list li:gt(4)").hide();

	//toggle message_body
	$(".message_head").click(function(){
	  $(this).next(".message_body").slideToggle(500)
	  return false;
	});

	//collapse all messages
	$(".collpase_all_message").click(function(){
	  $(".message_body").slideUp(500)
	  return false;
	});

	//show all messages
	$(".show_all_message").click(function(){
	  $(this).hide()
	  $(".show_recent_only").show()
	  $(".message_list li:gt(4)").slideDown()
	  return false;
	});

	//show recent messages only
	$(".show_recent_only").click(function(){
	  $(this).hide()
	  $(".show_all_message").show()
	  $(".message_list li:gt(4)").slideUp()
	  return false;
	});

});</code></pre>
<h3><em>8.</em> Imitating the WordPress Comment Backend</h3>
I think most of you have probably seen the WordPress Ajax comment management backend. Well, let’s imitate it with jQuery. In order to animate the background color, you need include the <a href="http://plugins.jquery.com/project/color"><strong>Color Animations</strong></a> plugin. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/wordpress-comments.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/wordpress-comments.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample7.gif" alt="sample" /></a>

<strong>First line</strong>: will add "alt" class to even &lt;div&gt; (to assign the grey background on every other &lt;div &gt;)
<strong>Second part</strong>: when &lt;a&gt; is clicked, alert a message, and then animate the backgroundColor and opacity of &lt;div&gt;
<strong>Third part</strong>: when &lt;a&gt; is clicked, first animate the backgroundColor of &lt;div&gt; to yellow, then white, and addClass "spam"
<strong>Fourth part</strong>: when &lt;a&gt; is clicked, first animate the backgroundColor of &lt;div&gt; to green, then white, and removeClass "spam"
<strong>Fifth part</strong>: when &lt;a&gt; is clicked, animate the backgroundColor to red and opacity ="hide"
<pre><code>//don't forget to include the Color Animations plugin
//&lt;script type="text/javascript" src="jquery.color.js"&gt;&lt;/script&gt;

$(document).ready(function(){

	$(".pane:even").addClass("alt");

	$(".pane .btn-delete").click(function(){
	  alert("This comment will be deleted!");

	  $(this).parents(".pane").animate({ backgroundColor: "#fbc7c7" }, "fast")
	  .animate({ opacity: "hide" }, "slow")
	  return false;
	});

	$(".pane .btn-unapprove").click(function(){
	  $(this).parents(".pane").animate({ backgroundColor: "#fff568" }, "fast")
	  .animate({ backgroundColor: "#ffffff" }, "slow")
	  .addClass("spam")
	  return false;
	});

	$(".pane .btn-approve").click(function(){
	  $(this).parents(".pane").animate({ backgroundColor: "#dafda5" }, "fast")
	  .animate({ backgroundColor: "#ffffff" }, "slow")
	  .removeClass("spam")
	  return false;
	});

	$(".pane .btn-spam").click(function(){
	  $(this).parents(".pane").animate({ backgroundColor: "#fbc7c7" }, "fast")
	  .animate({ opacity: "hide" }, "slow")
	  return false;
	});

});</code></pre>
<h3><em>9.</em> Image replacement gallery</h3>
Suppose you have a portfolio where you want to showcase multi images without jumping to another page, you can load the JPG into the target element. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/img-replacement.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/img-replacement.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample8.jpg" alt="sample" /></a>

First append an empty &lt;em&gt; to H2.

When a link within the &lt;p&gt; is clicked:
- store its <code>href</code> attribute into a variable "largePath"
- store its <code>title</code> attribute into a variable "largeAlt"
- replace the &lt;img id="largeImg"&gt; <code>scr</code> attribute with the variable "largePath" and replace the <code>alt</code> attribute with the variable "largeAlt"
- Set the <code>em</code> content (within the <code>h2</code>) with the variable largeAlt (plus the brackets)
<pre><code>$(document).ready(function(){

	$("h2").append('&lt;em&gt;&lt;/em&gt;')

	$(".thumbs a").click(function(){

	  var largePath = $(this).attr("href");
	  var largeAlt = $(this).attr("title");

	  $("#largeImg").attr({ src: largePath, alt: largeAlt });

	  $("h2 em").html(" (" + largeAlt + ")"); return false;
	});

});</code></pre>
<h3><em>10.</em> Styling different link types</h3>
With most modern browsers, styling the link selector is very easy. For example, to style the link to .pdf file, you can simply use the following CSS rule: <code>a[href $='.pdf'] { ... }</code>. Unfortunately, IE 6 doesn’t support this (this is another reason why <a href="http://www.webdesignerwall.com/general/trash-all-ie-hacks/">we hate IE</a>!). To get around this, you can do it with jQuery. <em>(view <a href="http://www.webdesignerwall.com/demo/jquery/link-types.html">demo</a>)</em>

<a href="http://www.webdesignerwall.com/demo/jquery/link-types.html"><img src="http://www.webdesignerwall.com/wp-content/uploads/2008/02/sample9.gif" alt="sample" /></a>

The first three lines should be very straight foward. They just a CSS class to the <code>&lt;a&gt;</code> element according to their <code>href</code> attribute.

The second part will get all <code>&lt;a&gt;</code> element that does not have "http://www.webdesignerwall.com" and/or does not start with "#" in the <code>href</code> attribute, then addClass "external" and set target= "_blank".
<pre><code>$(document).ready(function(){

	$("a[@href$=pdf]").addClass("pdf");

	$("a[@href$=zip]").addClass("zip");

	$("a[@href$=psd]").addClass("psd");

	$("a:not([@href*=http://www.webdesignerwall.com])").not("[href^=#]")
	  .addClass("external")
	  .attr({ target: "_blank" });

});</code></pre>