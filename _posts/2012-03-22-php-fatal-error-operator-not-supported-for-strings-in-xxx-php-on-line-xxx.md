---
ID: 1988
post_title: 'PHP-Fatal error: [] operator not supported for strings in xxx.php on line xxx'
author: ChinaBUG
post_excerpt: 'Fatal error: [] operator not supported for strings in G:\PHPnow\htdocs\plugins\app\app4exchange\mdl.app4exchange.php on line 111'
layout: post
permalink: >
  http://blog.ipodmp.com/2012/03/php-fatal-error-operator-not-supported-for-strings-in-xxx-php-on-line-xxx.html
published: true
post_date: 2012-03-22 10:34:35
---
悲剧呀，话说，不能占小便宜，也不能偷懒噢，前些天为了偷懒随意写了一个方法，还随意的指定了参数，今天扩展了一个功能，好死不死又增加了一个参数，结果悲剧了~~冲突了，出现下面的提示：

<strong>Fatal error</strong>: [] operator not supported for strings in <strong>G:\PHPnow\htdocs\plugins\app\app4exchange\mdl.app4exchange.php</strong> on line <strong>111</strong>

好神奇~~

花费了三个小时，才找到原因，TNN的好邪恶，参数里面出现了这个变量，偶设定为$f是一个字符型参数，然后我在方法里面还把$f当做数组变量来赋值~~神经一样的脑袋

改一下就好了~~