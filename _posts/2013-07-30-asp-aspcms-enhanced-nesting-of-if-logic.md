---
ID: 2523
post_title: >
  ASP-ASPCMS功能增强之IF嵌套逻辑判断
author: ChinaBUG
post_excerpt: |
  哈哈哈~~仰天大笑三声，靠~终于抽空处理了这个问题~话说这个if条件无法嵌套的问题是aspcms的一个大问题之一呀，怎么能不拥有嵌套判断呢？！不知道当时设计的人是怎么想的，然后网上找了一下，全部都是不能用的代码！如果有的话请告知我吧，反正我是没找到能用的！在官方网站上的能不能用的我不知道，反正目前还没注册成功可以查看。
  穷人还是指望自己多研究一点吧，今天终于抽空处理了一下，原来是这么简单哈~修改如下：
  PS：请转摘的人注意不想留作者信息的可以删除，但是请保持代码的完整性，别转摘了结果格式乱了不说，代码还丢三落四。
  请打开inc/AspCms_MainClass.asp在最下面End Class%>之上，写下如下函数（其实这个函数就是修改parseIf的，思路跟命名是参照网上的文章弄得）
layout: post
permalink: >
  http://blog.ipodmp.com/2013/07/asp-aspcms-enhanced-nesting-of-if-logic.html
published: true
post_date: 2013-07-30 16:49:59
---
哈哈哈~~仰天大笑三声，靠~终于抽空处理了这个问题~话说这个if条件无法嵌套的问题是aspcms的一个大问题之一呀，怎么能不拥有嵌套判断呢？！不知道当时设计的人是怎么想的，然后网上找了一下，全部都是不能用的代码！如果有的话请告知我吧，反正我是没找到能用的！在官方网站上的能不能用的我不知道，反正目前还没注册成功可以查看。

穷人还是指望自己多研究一点吧，今天终于抽空处理了一下，原来是这么简单哈~修改如下：

PS：请转摘的人注意不想留作者信息的可以删除，但是请保持代码的完整性，别转摘了结果格式乱了不说，代码还丢三落四。

请打开inc/AspCms_MainClass.asp在最下面End Class%&gt;之上，写下如下函数（其实这个函数就是修改parseIf的，思路跟命名是参照网上的文章弄得）
<pre>    'IF嵌套 2013.07.30
    '{if1:[navlist:lastone]&lt;&gt;1}&lt;div&gt;{if2:[navlist:sortid] MOD 2=0}BAI{else2}HEI{end if2}&lt;/div&gt;{end if1}
    Public Function parseIfx(str)
        if not isExistStr(content,"{if"&amp;str&amp;":") then Exit Function        
        dim matchIf,matchesIf,strIf,strThen,strThen1,strElse1,labelRule2,labelRule3
        dim ifFlag,elseIfArray,elseIfSubArray,elseIfArrayLen,resultStr,elseIfLen,strElseIf,strElseIfThen,elseIfFlag
        labelRule="{if"&amp;str&amp;":([\s\S]+?)}([\s\S]*?){end\s+if"&amp;str&amp;"}":labelRule2="{elseif"&amp;str&amp;"":labelRule3="{else"&amp;str&amp;"}":elseIfFlag=false
        regExpObj.Pattern=labelRule
        set matchesIf=regExpObj.Execute(content)
        for each matchIf in matchesIf             
            strIf=matchIf.SubMatches(0):strThen=matchIf.SubMatches(1)
            if instr(strThen,labelRule2)&gt;0 then
                elseIfArray=split(strThen,labelRule2):elseIfArrayLen=ubound(elseIfArray):elseIfSubArray=split(elseIfArray(elseIfArrayLen),labelRule3)
                resultStr=elseIfSubArray(1)
                Execute("if "&amp;strIf&amp;" then resultStr=elseIfArray(0)")
                for elseIfLen=1 to elseIfArrayLen-1
                    strElseIf=getSubStrByFromAndEnd(elseIfArray(elseIfLen),":","}","")
                    strElseIfThen=getSubStrByFromAndEnd(elseIfArray(elseIfLen),"}","","start")
                    Execute("if "&amp;strElseIf&amp;" then resultStr=strElseIfThen")
                    Execute("if "&amp;strElseIf&amp;" then elseIfFlag=true else  elseIfFlag=false")
                    if elseIfFlag then exit for
                next
                Execute("if "&amp;getSubStrByFromAndEnd(elseIfSubArray(0),":","}","")&amp;" then resultStr=getSubStrByFromAndEnd(elseIfSubArray(0),""}"","""",""start""):elseIfFlag=true")
                content=replace(content,matchIf.value,resultStr)
            else 
                if instr(strThen,"{else"&amp;str&amp;"}")&gt;0 then 
                    strThen1=split(strThen,labelRule3)(0)
                    strElse1=split(strThen,labelRule3)(1)
                    Execute("if "&amp;strIf&amp;" then ifFlag=true else ifFlag=false")
                    if ifFlag then content=replace(content,matchIf.value,strThen1) else content=replace(content,matchIf.value,strElse1)
                else    
                    Execute("if "&amp;strIf&amp;" then ifFlag=true else ifFlag=false")
                    if ifFlag then content=replace(content,matchIf.value,strThen) else content=replace(content,matchIf.value,"")
                end if
            end if
            elseIfFlag=false
        next
        set matchesIf=nothing
    End Function</pre>
然后查找Public Function parseCommon()这个函数，往下面找到

parseGbook()
'echo "&lt;hr&gt;"&amp;content&amp;"&lt;hr&gt;"
parseIf()
'echo "parseCommon() 执行完毕"

然后请在下面增加如下代码：

parseIfx("1")
parseIfx("2")
parseIfx("3")
......

你想要多少层的嵌套就写多少个，很明确了吧？！

然后就可以在模板文件之中调用了~调用语法在函数定义前有，这边就再重复一次吧：

{if1:[navlist:lastone]&lt;&gt;1}&lt;div&gt;{if2:[navlist:sortid] MOD 2=0}BAI{else2}HEI{end if2}&lt;/div&gt;{end if1}

PS：[navlist:lastone]是我自己修改的，表明菜单的最后项。