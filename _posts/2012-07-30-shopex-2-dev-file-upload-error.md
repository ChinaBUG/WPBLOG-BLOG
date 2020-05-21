---
ID: 2151
post_title: ShopEX-文件上传错误提示
author: ChinaBUG
post_excerpt: |
  ShopEX提示：'上传的文件大小超出了服务器的空间大小'
  UPLOAD_ERR_INI_SIZE
  ShopEX提示：'上传的文件大小超出浏览器限制'
  UPLOAD_ERR_FORM_SIZE
  ShopEX提示：'文件仅部分被上传'
  UPLOAD_ERR_PARTIAL
  ShopEX提示：'没有找到要上传的文件'
  UPLOAD_ERR_NO_FILE
  ShopEX提示：'文件写入到临时文件夹出错'
  UPLOAD_ERR_NO_TMP_DIR
layout: post
permalink: >
  http://blog.ipodmp.com/2012/07/shopex-2-dev-file-upload-error.html
published: true
post_date: 2012-07-30 11:05:16
---
如果您在使用ShopEX时出现如下提示，则请找服务商解决噢，具体按照下面的说明做哈

ShopEX提示：'上传的文件大小超出了服务器的空间大小'
UPLOAD_ERR_INI_SIZE
其值为 1，上传的文件超过了 php.ini 中 upload_max_filesize 选项限制的值。

ShopEX提示：'上传的文件大小超出浏览器限制'
UPLOAD_ERR_FORM_SIZE
其值为 2，上传文件的大小超过了 HTML 表单中 MAX_FILE_SIZE 选项指定的值。

ShopEX提示：'文件仅部分被上传'
UPLOAD_ERR_PARTIAL
其值为 3，文件只有部分被上传。

ShopEX提示：'没有找到要上传的文件'
UPLOAD_ERR_NO_FILE
其值为 4，没有文件被上传。

ShopEX提示：'服务器临时文件夹丢失'

ShopEX提示：'文件写入到临时文件夹出错'
UPLOAD_ERR_NO_TMP_DIR
其值为 6，找不到临时文件夹。PHP 4.3.10 和 PHP 5.0.3 引进。

ShopEX提示：
UPLOAD_ERR_CANT_WRITE
其值为 7，文件写入失败。PHP 5.1.0 引进。