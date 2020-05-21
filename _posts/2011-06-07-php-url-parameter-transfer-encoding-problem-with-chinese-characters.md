---
ID: 1277
post_title: >
  PHP-URL参数含汉字的传输编码问题
author: ChinaBUG
post_excerpt: |
  下面是自动调整编码的函数哈（来源:PHP自动识别字符集并完成转码）：
  //识别汉字编码,因为YBlog用的是utf-8,如果引用通告发过来的是gb2312的编码的话,需要可以识别并完成编码转换
  function safeEncoding($string,$outEncoding = 'UTF-8')
  {
  $encoding = "UTF-8";
  for($i=0;$i<strlen($string);$i++)
  {
  if(ord($string{$i})<128)
  continue;
  if((ord($string{$i})&224)==224)
  {
  //第一个字节判断通过
  $char = $string{++$i};
  if((ord($char)&128)==128)
  {
  //第二个字节判断通过
  $char = $string{++$i};
  if((ord($char)&128)==128)
  {
  $encoding = "UTF-8";
  break;
  }
  }
  }
  if((ord($string{$i})&192)==192)
  {
  //第一个字节判断通过
  $char = $string{++$i};
  if((ord($char)&128)==128)
  {
  //第二个字节判断通过
  $encoding = "GB2312";
  break;
  }
  }
  }
  if(strtoupper($encoding) == strtoupper($outEncoding))
  return $string;
  else
  return iconv($encoding,$outEncoding,$string);
  }
  然后在需要的地发调用这个函数哈~~这样还不会正确，还需要做多一步，就是下面的红色代码哈，代码如下：
  ......
  mysql_query('set names UTF8'); //这儿是最关键的哈~
  $result = mysql_query("insert into choujiangjilu(s_member_id,runtime,meno,no_id) values(".$sMid.",unix_timestamp(),\"" . safeEncoding($jl,'UTF-8') . "\",".$no_id.")");
  ......
layout: post
permalink: >
  http://blog.ipodmp.com/2011/06/php-url-parameter-transfer-encoding-problem-with-chinese-characters.html
published: true
post_date: 2011-06-07 21:23:45
---
今天在做项目的时候需要用到从URL里面获取中文参数，结果怎么调试都不行汗~~

我先是统一调整了文件的格式，还是不行，实在不行怎么办？

那就不理他了~~全部统一转换成一种吧，上网招了一下还真的现成的函数提供，立马抄下来。

测试一下，汗，还是不行，怎么办？

想想算了，代码没办法实现，那就看看系统提供了功能没有，一查，哈，还真的有~

修改了一下，编码正确了~~代码如下哈：

下面这个是我调用的路径哈

http://..../shop/view/member/flash_opt.php?sMid=1&amp;act=insert&amp;no=1&amp;sjl=第一次测试哈

下面是自动调整编码的函数哈（来源:<a href="http://www.yhustc.com/PHP%E8%BD%AC%E7%A0%81%E5%87%BD%E6%95%B0.html" target="_blank">PHP自动识别字符集并完成转码</a>）：

//识别汉字编码,因为YBlog用的是utf-8,如果引用通告发过来的是gb2312的编码的话,需要可以识别并完成编码转换  
function safeEncoding($string,$outEncoding = 'UTF-8')  
{  
    $encoding = "UTF-8";  
    for($i=0;$i&lt;strlen($string);$i++)  
    {  
        if(ord($string{$i})&lt;128)  
            continue;  
 
        if((ord($string{$i})&amp;224)==224)  
        {  
            //第一个字节判断通过  
            $char = $string{++$i};  
            if((ord($char)&amp;128)==128)  
            {  
                //第二个字节判断通过  
                $char = $string{++$i};  
                if((ord($char)&amp;128)==128)  
                {  
                    $encoding = "UTF-8";  
                    break;  
                }  
            }  
        }  
        if((ord($string{$i})&amp;192)==192)  
        {  
            //第一个字节判断通过  
            $char = $string{++$i};  
            if((ord($char)&amp;128)==128)  
            {  
                //第二个字节判断通过  
                $encoding = "GB2312";  
                break;  
            }  
        }  
    }  
      
    if(strtoupper($encoding) == strtoupper($outEncoding))  
        return $string;  
    else 
        return iconv($encoding,$outEncoding,$string);  
}

然后在需要的地发调用这个函数哈~~这样还不会正确，还需要做多一步，就是下面的红色代码哈，代码如下：

   ......
   <strong><span style="color: #ff0000;">mysql_query('set names UTF8'); //这儿是最关键的哈~</span></strong>
    $result = mysql_query("insert into choujiangjilu(s_member_id,runtime,meno,no_id) values(".$sMid.",unix_timestamp(),\"" . <span style="text-decoration: underline;">safeEncoding($jl,'UTF-8')</span> . "\",".$no_id.")");
   ......

到此测试通过哈~~~有问题记得互相交流哈~