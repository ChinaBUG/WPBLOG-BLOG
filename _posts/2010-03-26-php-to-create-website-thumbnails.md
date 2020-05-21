---
ID: 379
post_title: PHP to create website thumbnails
author: ChinaBUG
post_excerpt: |
  使用PHP创建网站缩略图
  关键代码：
  $browser = new COM("InternetExplorer.Application");
  $handle = $browser->HWND;
  $browser->Visible = true;
  $browser->FullScreen=true;
layout: post
permalink: >
  http://blog.ipodmp.com/2010/03/php-to-create-website-thumbnails.html
published: true
post_date: 2010-03-26 10:40:04
---
Bing.com has an awesome feature that is popping up more and more all over the internet. Most websites charge for this, as they should. It isn’t something that can be done on a normal hosting plan, but can only be done (this way) on a Windows server, where Internet Explorer is installed and you have access to. There are several ways to do this on Linux servers, but I am going to cover the Windows version.

In order to create an image for a thumbnail, we need to take a screen shot of webpage. PHP GD class allows us to take screen shots of different windows. PHP also allows us to control different windows, like Internet Explorer using the COM class. So basically what we need to do is have PHP open up a window in Internet Explorer, navigate to a website, and then take a picture of that window, then save it/display it. The tutorial shown on php.net shows exactly how to do this:

<!--p<br-->$browser = new COM("InternetExplorer.Application");
$handle = $browser-&gt;HWND;
$browser-&gt;Visible = true;
$browser-&gt;Navigate("http://www.libgd.org");

/* Still working? */
while ($browser-&gt;Busy) {
com_message_pump(4000);
}
$im = imagegrabwindow($handle, 0);
$browser-&gt;Quit();
imagepng($im, "iesnap.png");
imagedestroy($im);
?&gt;
So this is SUPPOSED to load libgd.org and take a snap shot and save it to “iesnap.png”, and it DOES work, but only if your server allows it to work. If it doesn’t you just get a black/blank picture.

Windows rarely lets one program access another program via the desktop and control it. By default, Apache isn’t allowed to open Internet Explorer on windows, so we have to add an exception.

On Vista, click the start button, and in the search box type “Services”.
At the very top, there should be a link under Programs called Services. Click that link.
It will open up all running services, and you will see Apache running (mine is Apache 2.2, and is the first entry).
Right click on it and select “Properties”
Navigate to the “Log On” tab
Click the box for “Allow service to interact with the desktop”
Save it and close the properties
Right click on Apache again
This time, click restart, and Apache will be restarted with the new features
Now run your script again! Vista pops up a little box alerting the user that a program is interact with the desktop. Just ignore it and it will disappear in a few seconds. After the program is complete, go to your web folder and you will see iesnap.png is now an image of a webpage!

Time to make some tweaks! You will notice that it grabs the ENTIRE screen, meaning the tool bars and everything. We will need to make a few adjustments to get rid of those. The first one I made was to make the browser show as a full screen:

$browser = new COM("InternetExplorer.Application");
$handle = $browser-&gt;HWND;
$browser-&gt;Visible = true;
$browser-&gt;FullScreen=true;

That last line will make it display in the fullscreen mode, which takes care of the tool bars at the top, but I still see the footer and the side bar that I want to get rid of. In order to do this, we need to crop a little, but GD doesn’t have a simple cropping method. What I did was copied part of this picture to a new picture and resized it. Keep in mind, my display settings are going to be different, and will require you to look at your picture and test it out:

<!--p<br-->$url = $_GET['url'];
$w = $_GET['w'];
$h = $_GET['h'];
if(empty($w))
$w=300;
if(empty($h))
$h=300;
if(empty($url))
$url="www.notan00b.com";
if($w&gt;1263)
$w=1263;
if($h&gt;780)
$h=1263;
$browser = new COM("InternetExplorer.Application");
$handle = $browser-&gt;HWND;
$browser-&gt;Visible = true;
$browser-&gt;FullScreen=true;
$browser-&gt;Navigate("http://".$url);

/* Still working?*/
while ($browser-&gt;Busy) {
com_message_pump(4000);
}
$im = imagegrabwindow($handle, 0);
$browser-&gt;Quit();
imagepng($im, "iesnap.png");
$dest = imagecreatetruecolor($w,$h);

// Copy
imagecopyresized($dest, $im, 0, 0, 0, 0, $w, $h,1263,780);

// Output and free from memory
header('Content-Type: image/gif');
imagegif($dest);

imagedestroy($dest);
imagedestroy($im);
?&gt;
There we have it. Even with the GET variables so you can reuse this script for AJAX or any HTML document. My images were 1280×800. To remove the bars I scaled it down to 1263×780, and they were removed almost perfectly, but it will be different for everyone.

I would post an example of it, but I am only hosted… I don’t actually pay for a VPS or private server just for one little blog. Sorry folks. If it was up to me, and I needed this service, I would probably pay a company to do it for me for a simple little website. If you have a VPS with enough memory, this may work for you. Good luck, let me know if you have any questions.