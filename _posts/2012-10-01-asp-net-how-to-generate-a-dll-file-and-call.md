---
ID: 2192
post_title: >
  ASP.NET-如何生成DLL文件，并且调用
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2012/10/asp-net-how-to-generate-a-dll-file-and-call.html
published: true
post_date: 2012-10-01 14:57:40
---
很多时候,我们需要将.cs文件单独编译成.dll文件, 操作如下:

打开命令窗口-&gt;输入cmd到控制台-&gt;cd C:/WINDOWS/Microsoft.NET/Framework/v1.1.4322

转到vs.net安装的该目录下-&gt;执行csc命令

csc /target:library File.cs

-&gt;在该目录下产生一个对应名字的.dll文件(前提:把.cs文件放到C:/WINDOWS/Microsoft.NET/Framework/v1.1.4322目录下)

csc命令的方式很多,请参考以下

译 File.cs 以产生 File.exe

csc File.cs 编译 File.cs 以产生 File.dll

csc /target:library File.cs 编译 File.cs 并创建 My.exe

csc /out:My.exe File.cs 通过使用优化和定义 DEBUG 符号，编译当前目录中所有的 C# 文件。输出为 File2.exe

csc /define:DEBUG /optimize /out:File2.exe *.cs 编译当前目录中所有的 C# 文件，以产生 File2.dll 的调试版本。不显示任何徽标和警告

csc /target:library /out:File2.dll /warn:0 /nologo /debug *.cs 将当前目录中所有的 C# 文件编译为 Something.xyz（一个 DLL）

csc /target:library /out:Something.xyz *.cs 编译 File.cs 以产生 File.dll

csc /target:library File.cs这个就是我们使用最多的一个命令，其实可以简单的写成csc /t:library File.cs，另外的一个写法是 csc /out:mycodebehind.dll /t:library mycodebehind.cs，这个可以自己指定输出的文件名。

csc /out:mycodebehind.dll /t:library mycodebehind.cs mycodebehind2.cs，这个的作用是把两个cs文件装到一个.dll文件里

举例(摘于网络)

一、 动态链接库

什么是动态链接库？DLL三个字母对于你来说一定很熟悉吧，它是Dynamic Link Library 的缩写形式，动态链接库 (DLL) 是作为共享函数库的可执行文件。动态链接提供了一种方法，使进程可以调用不属于其可执行代码的函数。函数的可执行代码位于一个 DLL 中，该 DLL 包含一个或多个已被编译、链接并与使用它们的进程分开存储的函数。DLL 还有助于共享数据和资源。多个应用程序可同时访问内存中单个 DLL 副本的内容。

和大多数程序员一样，你一定很使用过DLL吧。也曾感受到它的带给你程序设计和编码上的好错吧今天我想和大家探讨一个主题：如何在C#创建和调用DLL(动态链接库), 其实在很大意义上而讲，DLL让我更灵活的组织编写我们的应用程序，作为软件设计者，可一个根据它来达到很高的代码重用效果。下面我来介绍一下在C#中如何创建和调用DLL。

二、准备工作

我们需要对我们接下来要做的事情做个简单的介绍，在本文我们将利用C#语言创建一个名为 MyDLL.DLL的动态链接库，在这个动态链接库文件中我们将提供两个功能一个是对两个参数交换他们的值，另一个功能是求两个参数的最大公约数。然后创建一个应用程序使用这个DLL。运行并输出结果。

三、创建DLL

让我们创建以下三个C#代码文件：

1、  MySwap.cs
<div>
<div>
<div>[csharp] <a title="view plain" href="http://blog.csdn.net/chopper7278/article/details/2627667#">view plain</a><a title="copy" href="http://blog.csdn.net/chopper7278/article/details/2627667#">copy</a><a title="print" href="http://blog.csdn.net/chopper7278/article/details/2627667#">print</a><a title="?" href="http://blog.csdn.net/chopper7278/article/details/2627667#">?</a></div>
</div>
<ol>
	<li>using System;</li>
	<li></li>
	<li></li>
	<li></li>
	<li>namespace MyMethods</li>
	<li></li>
	<li>{</li>
	<li></li>
	<li></li>
	<li></li>
	<li>     public class SwapClass</li>
	<li></li>
	<li>     {</li>
	<li></li>
	<li></li>
	<li></li>
	<li>          public static bool Swap(ref long i,ref long j)</li>
	<li></li>
	<li>          {</li>
	<li></li>
	<li></li>
	<li></li>
	<li>               i = i+j;</li>
	<li></li>
	<li>               j = i-j;</li>
	<li></li>
	<li>               i = i-j;</li>
	<li></li>
	<li>               return true;</li>
	<li></li>
	<li>           }</li>
	<li></li>
	<li></li>
	<li></li>
	<li>       }</li>
	<li></li>
	<li></li>
	<li></li>
	<li>}</li>
</ol>
</div>
<pre>using System;
namespace MyMethods 
{
     public class SwapClass 
     {
          public static bool Swap(ref long i,ref long j) 
          { 
               i = i+j;
               j = i-j;
               i = i-j;
               return true; 
           }
       }
}
2、MyMaxCD.cs</pre>
<div>
<div>
<div>[csharp] <a title="view plain" href="http://blog.csdn.net/chopper7278/article/details/2627667#">view plain</a><a title="copy" href="http://blog.csdn.net/chopper7278/article/details/2627667#">copy</a><a title="print" href="http://blog.csdn.net/chopper7278/article/details/2627667#">print</a><a title="?" href="http://blog.csdn.net/chopper7278/article/details/2627667#">?</a></div>
</div>
<ol>
	<li>using System;</li>
	<li>namespace MyMethods</li>
	<li>  {</li>
	<li>       public class MaxCDClass</li>
	<li>       {</li>
	<li>            public static long MaxCD(long i, long j)</li>
	<li>            {</li>
	<li>                 long a,b,temp;</li>
	<li>                 if(i&gt;j)</li>
	<li>                 {</li>
	<li>                      a = i;</li>
	<li>                     b = j;</li>
	<li>                 }</li>
	<li>                 else</li>
	<li>                 {</li>
	<li>                      b = i;</li>
	<li>                      a = j;</li>
	<li>                 }</li>
	<li>                 temp = a % b;</li>
	<li>                 while(temp!=0)</li>
	<li>                {</li>
	<li>                     a = b;</li>
	<li>                     b = temp;</li>
	<li>                     temp = a % b;</li>
	<li>                }</li>
	<li>                return b;</li>
	<li>             }</li>
	<li>        }</li>
	<li> }</li>
</ol>
</div>
<pre>using System;
namespace MyMethods
{
     public class MaxCDClass
     {
          public static long MaxCD(long i, long j)
          {
               long a,b,temp;
               if(i&gt;j)
               {
                    a = i;
                    b = j;
               }
               else
               {
                    b = i;
                    a = j;
               }
               temp = a % b;
               while(temp!=0)
               {
                    a = b;
                    b = temp;
                    temp = a % b;
               }
               return b;
            }
       }
}
　需要注意的是：我们在制作这两个文件的时候可以用Visual Studio.NET或者其他的文本编辑器，就算是记事本也可以。这两个文件虽然不在同一个文件里面，但是他们是属于同一个namespace（名称空间）这对以后我们使用这两个方法提供了方便。当然他们也可以属于不同的名称空间，这是完全可以的，但只是在我们应用他们的时候就需要引用两个不同的名称空间，所以作者建议还是写在一个名称空间下面比较好。</pre>
接下来的任务是把这两个cs文件变成我们需要的DLL文件。方法是这样的：在安装了Microsoft.NET Framework的操作系统上，我们可以在Windows所在目录下找到Microsoft.NET目录。在这个目录下面提供了C#的编译器，CSC.EXE运行：csc /target:library /out:MyDLL.DLL MySwap.cs MyMaxCD.cs，完成后可在本目录下面找到我们刚才生成的MyDLL.DLL文件/target:library 编译器选项通知编译器输出 DLL 文件而不是 EXE 文件。后跟文件名的 /out 编译器选项用于指定 DLL 文件名。如果/out后面不跟文件名编译器使用第一个文件 (MySwap.cs) 作为 DLL 文件名。生成的文件为MySwap.DLL文件。

OK!我们创建动态链接库文件的任务完成了，现在是我们享受劳动成果的时候了，下面我将介绍如何使用我们所创建的动态链接库文件。   四、使用DLL   我们简单写一个小程序来测试一下我们刚才写的两个方法是否正确，好吧，跟我来：

MyClient.cs
<div>
<div>
<div>[csharp] <a title="view plain" href="http://blog.csdn.net/chopper7278/article/details/2627667#">view plain</a><a title="copy" href="http://blog.csdn.net/chopper7278/article/details/2627667#">copy</a><a title="print" href="http://blog.csdn.net/chopper7278/article/details/2627667#">print</a><a title="?" href="http://blog.csdn.net/chopper7278/article/details/2627667#">?</a></div>
</div>
<ol>
	<li>using System;</li>
	<li></li>
	<li>using MyMethods; //这里我们引用刚才定义的名称空间，如果刚才的两个文件我们写在两个不同的名称空间</li>
	<li></li>
	<li></li>
	<li></li>
	<li>class MyClient</li>
	<li></li>
	<li>{</li>
	<li></li>
	<li>     public static void Main(string[] args)</li>
	<li></li>
	<li>     {</li>
	<li></li>
	<li>         if (args.Length != 2)</li>
	<li></li>
	<li>         {</li>
	<li></li>
	<li>              Console.WriteLine("Usage: MyClient &lt;num1&gt; &lt;num2&gt;");</li>
	<li></li>
	<li>              return;</li>
	<li></li>
	<li>         }</li>
	<li></li>
	<li>          long num1 = long.Parse(args[0]);</li>
	<li></li>
	<li>          long num2 = long.Parse(args[1]);</li>
	<li></li>
	<li>          SwapClass.Swap(ref num1,ref num2);</li>
	<li></li>
	<li>　　 // 请注意，文件开头的 using 指令使您得以在编译时使用未限定的类名来引用 DLL 方法</li>
	<li></li>
	<li>          Console.WriteLine("The result of swap is num1 = {0} and num2 ={1}",num1, num2);</li>
	<li></li>
	<li></li>
	<li></li>
	<li>          long maxcd = MaxCDClass.MaxCD(num1,num2);</li>
	<li></li>
	<li></li>
	<li></li>
	<li>          Console.WriteLine("The MaxCD of {0} and {1} is {2}",num1, num2, maxcd);</li>
	<li></li>
	<li>     }</li>
	<li></li>
	<li>}</li>
</ol>
</div>
<pre>using System; 

using MyMethods; //这里我们引用刚才定义的名称空间，如果刚才的两个文件我们写在两个不同的名称空间

class MyClient 

{

     public static void Main(string[] args) 

     {

         if (args.Length != 2) 

         {

              Console.WriteLine("Usage: MyClient &lt;num1&gt; &lt;num2&gt;"); 

              return; 

         }

          long num1 = long.Parse(args[0]); 

          long num2 = long.Parse(args[1]); 

          SwapClass.Swap(ref num1,ref num2);

　　 // 请注意，文件开头的 using 指令使您得以在编译时使用未限定的类名来引用 DLL 方法

          Console.WriteLine("The result of swap is num1 = {0} and num2 ={1}",num1, num2);

          long maxcd = MaxCDClass.MaxCD(num1,num2);

          Console.WriteLine("The MaxCD of {0} and {1} is {2}",num1, num2, maxcd); 

     }

}</pre>
若要生成可执行文件 MyClient.exe，请使用以下命令行：

csc /out:MyClient.exe /reference:MyLibrary.DLL MyClient.cs

/out 编译器选项通知编译器输出 EXE 文件并且指定输出文件名 (MyClient.exe)。/reference 编译器选项指定该程序所引用的 DLL 文件。

五、执行

若要运行程序，请输入 EXE 文件的名称，文件名的后面跟两个数字，例如：MyClient 123 456

六、输出

The result of swap is num1 = 456 and num2 = 123

The MaxCD of 456 and 123 is 3

七、小结

动态链接具有下列优点：

１、节省内存和减少交换操作。很多进程可以同时使用一个 DLL，在内存中共享该 DLL 的一个副本。相反，对于每个用静态链接库生成的应用程序，Windows 必须在内存中加载库代码的一个副本。

２、节省磁盘空间。许多应用程序可在磁盘上共享 DLL 的一个副本。相反，每个用静态链接库生成的应用程序均具有作为单独的副本链接到其可执行图像中的库代码。 　　　　３、升级到 DLL 更为容易。DLL 中的函数更改时，只要函数的参数和返回值没有更改，就不需重新编译或重新链接使用它们的应用程序。相反，静态链接的对象代码要求在函数更改时重新链接应用程序。

４、提供售后支持。例如，可修改显示器驱动程序 DLL 以支持当初交付应用程序时不可用的显示器。

５、支持多语言程序。只要程序遵循函数的调用约定，用不同编程语言编写的程序就可以调用相同的 DLL 函数。程序与 DLL 函数在下列方面必须是兼容的：函数期望其参数被推送到堆栈上的顺序，是函数还是应用程序负责清理堆栈，以及寄存器中是否传递了任何参数。

６、提供了扩展 MFC 库类的机制。可以从现有 MFC 类派生类，并将它们放到 MFC 扩展 DLL 中供 MFC 应用程序使用。

７、使国际版本的创建轻松完成。通过将资源放到 DLL 中，创建应用程序的国际版本变得容易得多。可将用于应用程序的每个语言版本的字符串放到单独的 DLL 资源文件中，并使不同的语言版本加载合适的资源。

使用 DLL 的一个潜在缺点是应用程序不是独立的；它取决于是否存在单独的 DLL 模块。