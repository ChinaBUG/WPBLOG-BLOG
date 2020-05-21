---
ID: 2421
post_title: 'jquery &#038; mootools &#8211; 简单的幻灯展示插件(simple slideshow)'
author: ChinaBUG
post_excerpt: |
  0、html
  &lt;div id="slideshow"&gt;
  &lt;img src="image1.jpg" alt="Slideshow Image 1" /&gt;
  &lt;img src="image2.jpg" alt="Slideshow Image 2" /&gt;
  &lt;img src="image3.jpg" alt="Slideshow Image 3" /&gt;
  &lt;img src="image4.jpg" alt="Slideshow Image 4" /&gt;
  &lt;/div&gt;
  1、jQuery slideshow
  FROM:A Simple jQuery Slideshow
  2、Mootools slideshow
  FROM:Create a Simple Slideshow Using MooTools
  3、SimpleSlider
  FROM:http://ruyadorno.github.io/SimpleSlider/
layout: post
permalink: >
  http://blog.ipodmp.com/2013/04/jquery-mootools-simple-slideshow.html
published: true
post_date: 2013-04-22 16:06:47
---
0、html

&lt;div id="slideshow"&gt;
&lt;img src="image1.jpg" alt="Slideshow Image 1" /&gt;
&lt;img src="image2.jpg" alt="Slideshow Image 2" /&gt;
&lt;img src="image3.jpg" alt="Slideshow Image 3" /&gt;
&lt;img src="image4.jpg" alt="Slideshow Image 4" /&gt;
&lt;/div&gt;

1、jQuery slideshow

FROM:<a href="http://jonraasch.com/blog/a-simple-jquery-slideshow">A Simple jQuery Slideshow</a>

&lt;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript"&gt;
/***
Simple jQuery Slideshow Script
Released by Jon Raasch (jonraasch.com) under FreeBSD license: free to use or modify, not responsible for anything, etc.  Please link out to me if you like it :)
***/
function slideSwitch() {
var $active = $('#slideshow IMG.active');
if ( $active.length == 0 ) $active = $('#slideshow IMG:last');
// use this to pull the images in the order they appear in the markup
var $next =  $active.next().length ? $active.next():$('#slideshow IMG:first');
// uncomment the 3 lines below to pull the images in random order
$active.addClass('last-active');
$next.css({opacity: 0.0})
.addClass('active')
.animate({opacity: 1.0}, 1000, function() {$active.removeClass('active last-active');});
}
$(function(){setInterval( "slideSwitch()", 5000 );});
&lt;/script&gt;
&lt;style type="text/css"&gt;
/*** set the width and height to match your images **/
#slideshow {
position:relative;
height:350px;
}
#slideshow IMG {
position:absolute;
top:0;
left:0;
z-index:8;
opacity:0.0;
}
#slideshow IMG.active {
z-index:10;
opacity:1.0;
}
#slideshow IMG.last-active {
z-index:9;
}
&lt;/style&gt;

2、Mootools slideshow

#2.1 FROM:<a href="http://davidwalsh.name/mootools-slideshow">Create a Simple Slideshow Using MooTools</a>

&lt;style&gt;
<code>#slideshow { width:512px; height:384px; position:relative; }
#slideshow img { display:block; position:absolute; top:0; left:0; z-index:1; }</code>
&lt;/style&gt;&lt;script&gt;
window.addEvent('domready',function() {
var showDuration = 3000;
var container = $('slideshow');
var images = container.getElements('img');
var currentIndex = 0;
var interval;
images.each(function(img,i){
if(i &gt; 0) img.set('opacity',0);
});
var show = function() {
images[currentIndex].fade('out');
images[currentIndex = currentIndex &lt; images.length - 1 ? currentIndex+1 : 0].fade('in');
};
window.addEvent('load',function(){interval = show.periodical(showDuration);});
});
&lt;/script&gt;

#2.2 FROM https://github.com/JohannesFischer/SimpleSlideShow

[code lang="javscript"]
/**
 * SlideShow
 * requirements: MooTools 1.2
 */
var SlideShow = new Class({
	Implements: [Events, Options],
    options: {
        fadeOut: false,
        fxDuration: 500,
        fxTransition: 'quart:out',
        interval: 5000,
		// onChange: $empty();
        showTitle: false
    },
    initialize: function(element, options){
        this.element = $(element);
        this.setOptions(options);
        this.current = 0;
        this.images = this.getImages();
        this.initImages();
        if(this.options.showTitle){
            this.title = new Element('p', {
                'class' : 'title'
            }).inject(this.element);
            this.setTitle();
        }
        this.slide.periodical(this.options.interval, this);
    },
	change: function(){
		this.fireEvent('change');
	},
    getImages: function(){
        return this.element.getElements('img');
    },
    initImages: function(){
        this.images.each(function(img, i){
            img.setStyles({
                opacity: i == 0 ? 1 : 0,
                zIndex: i == 0 ? 1 : 0
            });
        });
    },
    slide: function(){
        var next = this.current + 1 &lt; this.images.length ? this.current + 1 : 0;
        this.images[next].setStyles({
            opacity: 0,
            zIndex: 2
        });
        if(this.options.fadeOut){
            new Fx.Tween(this.images[this.current], {
                duration: this.options.fxDuration,
                transition: this.options.fxTransition
            }).start('opacity', 0);
        }
        new Fx.Tween(this.images[next], {
            duration: this.options.fxDuration,
            transition: this.options.fxTransition
        }).start('opacity', 1).chain(function(){
            this.images[this.current].setStyles({
                opacity: 0,
                zIndex: 0
            });
            this.images[next].setStyle('z-index', 1);
            this.current = next;
            this.setTitle();
        }.bind(this));
		this.change();
    },
    setTitle: function(){
        if(!this.options.showTitle){
            return;
        }
        this.title.set('html', this.images[this.current].get('title'));
    }
});[/code]

2014.03.25 补充

3、SimpleSlider：

[code lang="javascript"]
&lt;script src=&quot;simpleslider.min.js&quot;&gt;&lt;/script&gt;
&lt;script&gt;
    var imgSlider = new SimpleSlider( document.getElementById('slideshow') );
&lt;/script&gt;
[/code]

具体请查看：

http://ruyadorno.github.io/SimpleSlider/

下载：https://github.com/ruyadorno/SimpleSlider