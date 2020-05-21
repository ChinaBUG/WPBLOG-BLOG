---
ID: 3111
post_title: VB6反编译详解
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2013/12/vb-detailed-decompile-vb6.html
published: true
post_date: 2013-12-24 10:17:03
---
虫曰：

虫子是从vb5入门的，当年vb5挺牛逼的，看着比较适合我这种懒人学习的就学了，也学得不三不四，哎，浪费了青春啊~当年也有vb3/4的版本但是很简陋，到vb5的时候才比较有点样子，还是中文的~想起当年，安装着MSDN然后一个个的看过去，哎，青春已逝

今天，看到有这篇文章，好老之前的文章了，觉得挺不错的噢，阅读一下，转摘保存留念。

我看的文章来自：<a href="http://chenjava.blog.51cto.com/374566/93609">VB6反编译详解</a> 不过他的来源是在<a href="http://bbs.pediy.com/showthread.php?threadid=28715">看雪论坛</a>，原文作者的博客里面已经没有内容了。

~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~ ~..~
<pre><span style="color: #333333;"> 标 题:</span><span style="color: #000000;"> 【原创】VB6反编译详解（一）</span><span style="color: #666666;">
 <span style="color: #333333;">作 者:</span> <span style="color: #000000;">kenmark</span>
 <span style="color: #333333;">时 间:</span> 2006-07-09,16:59:30
 <span style="color: #333333;">链 接:</span> http://bbs.pediy.com/showthread.php?t=28715
 </span>
 VB6反编译详解 by Kenmark-Fenix
 **************************************************
 最新于2006-7-13更新！
 **************************************************
 写本文已经惦记了好几年了，由于一直没有完整的资料和充裕的时间，所以一直没有动手。
 在这里一方面是写给大家看看，另一方面是招募更多有志于反编译VB6的同志们一起来研究学习！
 我的E-MAIL:ken.mingyuan@hotmail.com ken.mingyuan@gmail.com
 我的BLOG: blog.csdn.net/kenmark
 我的QQ：188916915
 十分期待着与大家一起学习！
 ――Kenmark
 VB6是一个半编译半解释的语言，编译后程序主要在运行库MSVBVM60.DLL下转悠，通过与MSVBVM60的互动来完成程序运行的过程。
 1.引入（参考：《VB程序大揭密》我的博客上有转载http://blog.csdn.net/Kenmark/archive/2005/08/11/450985.aspx）
 我们用W32DASM打开一个中型的VB程序来反汇编，我们发现程序中用到的MSGBOX÷FileCopy等理应对应API函数居然一个都没有出现在编 译后程序的IMPORT TABLE里，一般VC和DEPHI都是直接出现在编译后程序的IMPORT TABLE里的，而我们的VB程序用到了如此之多 的API函数居然只使用了一个DLL――MSVBVM60.DLL。
 然后用工具打开MSVBVM60.DLL，一看，输出的函数还真不少，其中有用__vba和rtc开头的也有直接就是函数名的，仔细一看，哇赛，可以说完全是一个windows API的代理，应有尽有:
 rtcRandomize ：Randomize 函数的对应API； 
 rtcMidCharVar ：Mid 函数的对应API； 
 rtcLeftCharVar、rtcRightCharVar ：看出来了吧，这些是Left、Right函数的对应API； 
 rtcUpperCaseVar ：UCase 函数的对应API； 
 rtcKillFiles ：Kill 语句的对应API； 
 rtcFileCopy ：FileCopy 语句的对应API； 
 rtcFileLength ：EOF、FileLen函数的对应API； 
 rtcGetTimer ：Randomize Timer中获取Timer的对应API； 
 rtcShell ：Shell函数的的对应API； 
 rtcMakeDir ：MkDir 语句的对应API； 
 rtcRemoveDir ：RmDir 语句的对应API； 
 rtcDir ：Dir 函数的对应API； 
 rtcSpaceVar ：Space 函数的对应API； 
 原来，所有VB的操作函数都是在调用MSVBVM60.DLL里面实现
 前缀是rtc的是一般的语句和函数
 涉及字符串处理的都叫var,例如：
 __vbaUbound ： UBound 的对应API； 
 __vbaFileOpen ：Open 语句的对应API； 
 __vbaStrCmp ：比较两个字符串：If String1 = String2 Then ...... 
 __vbaVarOr ：Or 运算符的对应API； 
 __vbaRedim ：Redim 语句的对应API； 
 __vbaRedimPreserve ：Redim 语句加上 Preserve 参数的对应API； 
 __vbaGet、vbaPut ：Get、Put语句的对应API…… 
 我们还看到一个DLL：DllFunctionCall，这个就是我们调用其他DLL时需要向MSVBVM60申请的，…………。
 可以说VB的程序是一个包裹在MSVBVM60阴影控制下的孩子，所有的操作都要直接向它请求，而MSVBVM60完全可以称得上是一个代理的机器。从程序开始，到运行中的所有操作，函数调用，错误报告等等，都是由它一手包办的。
 来吧，我们这里不是来介绍它是怎么构成的我们要搞掉它虚伪的外表，把我们的代码从MSVBVM60的封建保护下救出来。
 我的资料大量是参考一个开源的VB程序解析程序（居然也是用VB编写的），这个程序可以完全分析出VB程序（没有加壳）的PROJECT信息以及完全将 FORM变回来，对于代码呢，可以获得SUB MAIN的汇编代码地址，但是不能返回到VB代码，里面还内置了一个假的反汇编器，以后我会说的！
 我提供了它的下载，大家去看看，不知道它的资料是哪里来的十分全面，就是用VB写的比较繁！还有由于界面控制太多，代码有点乱！
 2.程序初始化
 我们用W32DASM打开任何VB程序，跳到ENTRY POINT之后看到的总是一个PUSH *****一个地址，然构调用MSVBVM60.DLL的ThunRTMain
 而在VC程序里，我们至少要看到进程的创建什么的，其实这个VB函数是一切的开端，由它开始来解析整个编译后的程序，完成系统环境的初始化，然后找到真正的程序入口，跳转到程序的领空。
 所以这个PUSH指令压入栈的是VBEXE（姑且这么称呼）初始化结构的开始。我们存下这个地址（这个是一个VA要正确地转换成文件地OFFSET先要-ImageBase然后要对照块表，可以用工具完成，熟悉了一下就能看出来），然后我们跳到那里。
 看看是什么,哇赛，是VB程序的招牌也！“VB5!”是所有VB程序初始化结构入口地址指向的MAGIC字符，找到这里，开始能够读取VB初始化结构了（我们现在做的就是ThunRTMain要完成的）
 我们开始了！
 部分名词解读:
 RVA 相对虚拟地址,一般需要使用RVA2Offset来将其转换成绝对的文件偏移        变量前缀为pr或是a
 VA 虚拟地址,减去IMAGEBASE就是RVA                                        变量前缀为p或是a
 offset 相对结构的偏移                                                    变量前缀为o
 1）VBHEADER
 从PE文件的ENTRY POINT进入后第一个指令是压入一个指针来表示VBHEADER的位置，这个指令为
 push xxxxxxxx地址是经过基址IMAGE_BASE偏移后的地址，所以减去IMAGE_BASE后获得的就是VBHEADER开始的VA然后用PEFile类中的函数加以分析可以得到的是文件中VBHEADER的偏移量。
 这个指令在机器码中是这样表示的：68 C0 11 40 00   
 68是PUSH的机器码，而后是内存存储方式的地址化成汇编语言就是：
 push 004011c0 所以减去基址后就是11C0然后进行VA2OFFSET得到的是11c0的偏移，转向那段数据就能得到VBHEADER。
 这是VBHEADER结构的C语言描述:

 typedef struct
 {
   char Signature[4];    // 四个字节的签名符号，和PEHEADER里的那个signature是类似性质的东西，VB文件都是"VB5!"
   WORD RtBuild;          // 运行时创立的变量（类似编译的时间）
   BYTE LangDLL[14];      // 语言DLL文件的名字（如果是0x2A的话就代表是空或者是默认的）
   BYTE BakLangDLL[14];  // 备份DLL语言文件的名字（如果是0x7F的话就代表是空或者是默认的，改变这个值堆EXE文件的运行没有作用）
   WORD RtDLLVer;        // 运行是DLL文件的版本
   DWORD LangID;          // 语言的ID
   DWORD BakLangID;      // 备份语言的ID（只有当语言ID存在时它才存在）
   DWORD pSubMain;        // RVA（实际研究下来是VA） sub main过程的地址指针（3.）（如果时00000000则代表这个EXE时从FORM窗体文件开始运行的）
   DWORD pProjInfo;      // VA 工程信息的地址指针，指向一个ProjectInfo_t结构（2.）
   DWORD fMDLIntObjs;    // ?详细见"MDL 内部组建的标志表"
   DWORD fMDLIntObjs2;    // ?详细见"MDL 内部组建的标志表"
   DWORD ThreadFlags;    // 线程的标志
   //* 标记的定义（ThreadFlags数值的含义）
   //+-------+----------------+--------------------------------------------------------+
   //| 值    | 名字           | 描述                                                   |
   //+-------+----------------+--------------------------------------------------------+
   //|  0x01 | ApartmentModel | 特别化的多线程使用一个分开的模型                        |
   //|  0x02 | RequireLicense | 特别化需要进行认证(只对OCX)                             |
   //|  0x04 | Unattended     | 特别化的没有GUI图形界面的元素需要初始化                |
   //|  0x08 | SingleThreaded | 特别化的静态区时单线程的                                |
   //|  0x10 | Retained       | 特别化的将文件保存在内存中(只对Unattended)               |
   //+-------+----------------+--------------------------------------------------------+
   //ex: 如果是0x15就表示是一个既有多线程,内存常驻,并且没有GUI元素要初始化
   DWORD ThreadCount;    // 线程个数
   WORD FrmCount;        // 窗体个数
   WORD pExternalComponentCount;    // VA 外部引用个数例如WINSOCK组件的引用
   DWORD ThunkCount;      // ?大概是内存对齐相关的东西
   DWORD pGUITable;      // VA GUI元素表的地址指针（指向一个GUITable_t结构（4.四））
   DWORD pExternalComponentTable;    // VA 外部引用表的地址指针
 //  DWORD pProjDep;      // VA 工程的描述的地址指针（这个其实没有）
   DWORD pComRegData;    // VA COM注册数据的地址指针
   DWORD oProjExename;    // Offset 指向工程EXE名字的字符串
   DWORD oProjTitle;      // Offset 指向工程标题的字符串
   DWORD oHelpFile;      // Offset 指向帮助文件的字符串
   DWORD oProjName;      // Offset 指向工程名的字符串
 }VBHeader_t;

 //* MDL 内部组建的标志表
 //+---------+------------+---------------+
 //|   ID    |    值      | 组建名称      |
 //+---------+------------+---------------+
 //|                           第一个标志 |
 //+---------+------------+---------------+
 //|    0x00 | 0x00000001 | PictureBox    |
 //|    0x01 | 0x00000002 | Label         |
 //|    0x02 | 0x00000004 | TextBox       |
 //|    0x03 | 0x00000008 | Frame         |
 //|    0x04 | 0x00000010 | CommandButton |
 //|    0x05 | 0x00000020 | CheckBox      |
 //|    0x06 | 0x00000040 | OptionButton  |
 //|    0x07 | 0x00000080 | ComboBox      |
 //|    0x08 | 0x00000100 | ListBox       |
 //|    0x09 | 0x00000200 | HScrollBar    |
 //|    0x0A | 0x00000400 | VScrollBar    |
 //|    0x0B | 0x00000800 | Timer         |
 //|    0x0C | 0x00001000 | Print         |
 //|    0x0D | 0x00002000 | Form          |
 //|    0x0E | 0x00004000 | Screen        |
 //|    0x0F | 0x00008000 | Clipboard     |
 //|    0x10 | 0x00010000 | Drive         |
 //|    0x11 | 0x00020000 | Dir           |
 //|    0x12 | 0x00040000 | FileListBox   |
 //|    0x13 | 0x00080000 | Menu          |
 //|    0x14 | 0x00100000 | MDIForm       |
 //|    0x15 | 0x00200000 | App           |
 //|    0x16 | 0x00400000 | Shape         |
 //|    0x17 | 0x00800000 | Line          |
 //|    0x18 | 0x01000000 | Image         |
 //|    0x19 | 0x02000000 | Unsupported   |
 //|    0x1A | 0x04000000 | Unsupported   |
 //|    0x1B | 0x08000000 | Unsupported   |
 //|    0x1C | 0x10000000 | Unsupported   |
 //|    0x1D | 0x20000000 | Unsupported   |
 //|    0x1E | 0x40000000 | Unsupported   |
 //|    0x1F | 0x80000000 | Unsupported   |
 //+---------+------------+---------------+
 //|                          第二个标志  |
 //+---------+------------+---------------+
 //|    0x20 | 0x00000001 | Unsupported   |
 //|    0x21 | 0x00000002 | Unsupported   |
 //|    0x22 | 0x00000004 | Unsupported   |
 //|    0x23 | 0x00000008 | Unsupported   |
 //|    0x24 | 0x00000010 | Unsupported   |
 //|    0x25 | 0x00000020 | DataQuery     |
 //|    0x26 | 0x00000040 | OLE           |
 //|    0x27 | 0x00000080 | Unsupported   |
 //|    0x28 | 0x00000100 | UserControl   |
 //|    0x29 | 0x00000200 | PropertyPage  |
 //|    0x2A | 0x00000400 | Document      |
 //|    0x2B | 0x00000800 | Unsupported   |
 //+---------+------------+---------------+
 //ex: 如果值是0x30F000 (那个被叫做 "静态二进制常量定义在大多数的地方")就是意味着来初始化打印机,窗体,屏幕,剪贴板,组建(0xF000)也有Drive/Dir 组建(0x30000).
 //这是VB工程的一个默认的设置因为这些组建都能从一个模块中获得module (例如,他们是没有图像的除了经常被创造窗体)

 2）工程信息
 从VBHEADER-&gt;ProjectInfo(是一个VA)-IMAGEBASE后在通过RAV2OFFSET得到的是 工程信息 的偏移地址.
 这是PROJECT_INFO结构的C语言描述:
 const long MAX_PATH = 260;  //最长的PATH长度

 typedef struct
 {
   DWORD Signature;        // 结构的签名特性，和魔术字符类似
   DWORD pObjectTable;      // VA 结构指向的组件列表的地址指针（很重要的！（7.））
   DWORD Null1;            // ?没有用的东西
   WORD pStartOfCode;      // VA 代码开始点,类似PEHEAD-&gt;EntryPoint这里告诉了VB代码实际的开始点
   DWORD Flag1;            // 标志1
   DWORD ThreadSpace;      // 多线程的空间?????????????????
   DWORD pVBAExcrptionhandler;  // VA VBA意外处理机器地址指针
   DWORD pNativeCode;      // VA 本地机器码开始位置的地址指针
   WORD oProjectLocation;  // Offset 工程位置?????????????????
   WORD Flag2;              // 标志2
   WORD Flag3;              // 标志3
   BYTE OriginalPathName[MAX_PATH*2];  // 原文件地址,一个字符串,长度最长为MAX_PATH
   BYTE NullSpacer;        // 无用的东西,用来占位置????????????????????
   DWORD pExternalTable;    // VA 引用表的指针地址
   DWORD ExternalCount;    // 引用表大小(个数)
   // sizeof() = 0x23c
 }ProjectInfo_t;
 这个东西基本包含了这个PROJECT的信息，但是主要的意义还是提供了之后的结构的索引，最重要的是它提供了传说中的OBJECT TABLE的入口地址，对于EXE里面有多少FORM,MODULE十分重要。

 3）SubMain
 从VBHEADER-&gt;SubMain可以获得SubMain代码的入口处,但是有些VB程序是从FORM开始运行的所以当这个值为0时就代表此程序不是从SubMain开始运行的而是从FORM开始运行的.

 4）GUITable
 从VBHEADER-&gt;GUITable可以获得GUI元素表的地址指针,也就是指向WINDOWS图形元素的表.
 这是GUITable结构的C语言描述:

 typedef struct
 {
     DWORD lStructSize;      // 这个结构的总大小
     BYTE uuidObjectGUI[15];  // Object GUI的UUID
     DWORD Unknown1;          // ???????????????????????????????????
     DWORD Unknown2;          // ???????????????????????????????????
     DWORD Unknown3;          // ???????????????????????????????????
     DWORD Unknown4;          // ???????????????????????????????????
     DWORD lObjectID;        // 当前工程的组件ID
     DWORD Unknown5;          // ???????????????????????????????????
     DWORD fOLEMisc;          // OLEMisc标志
     BYTE uuidObject[15];    // 组件的UUID
     DWORD Unknown6;          // ???????????????????????????????????
     DWORD Unknown7;          // ???????????????????????????????????
     DWORD pFormPointer;      // VA 指向GUI Object Info结构的地址指针
     DWORD Unknown8;          // ???????????????????????????????????
     // sizeof() =  0x50
 }GUITable_t;
 这个表没有什么重要，主要就是那个pFormPointer比较有用。

 5）ExternalComponentTable
 从VBHEADER-&gt;ExternalComponentTable可以获得外部引用表的地址指针.
 这是ExternalTable以及ExternalLibrary的VB结构以及C语言描述:

 typedef struct
 {
   DWORD Flag;              // 标志
   DWORD pExternalLibrary;  // VA 指向ExternalLibrary结构的地址指针
 }ExternalTable_t;

 typedef struct
 {
   DWORD pLibraryName;      // VA 指向  NTS
   DWORD pLibraryFunction;  // VA 指向  NTS
 }ExternalLibrary_t;

 6)ComRegData and COMRegInfo
 从VBHEADER-&gt;ComRegData可以获得COM注册数据的地址指针.
 这是COMRegData以及COMRegInfo结构的C语言描述:
 typedef struct
 {
     DWORD oRegInfo;                    //Offset 指向COM Interfaces Info结构（COM接口信息）
     DWORD oNTSProjectName;            //Offset 指向Project/Typelib Name（工程名）
     DWORD oNTSHelpDirectory;          //Offset 指向Help Directory（帮助文件目录）
     DWORD oNTSProjectDescription;      //Offset 指向Project Description（工程描述）
     BYTE uuidProjectClsId(15);        //Project/Typelib的CLSID
     DWORD lTlbLcid;                    //Type Library的LCID
     WORD iPadding1;                    //没有用的内存对齐空间1
     WORD iTlbVerMajor;                //Typelib 主版本
     WORD iTlbVerMinor;                //Typelib 次版本
     WORD iPadding2;                    //没有用的内存对齐空间2
     DWORD lPadding3;                  //没有用的内存对齐空间3
     // sizeof() = 0x30
 }COMRegData_t;

 typedef struct
 {
     DWORD oNextObject;            //Offset to COM Interfaces Info
     DWORD oObjectName;            //Offset to Object Name
     DWORD oObjectDescription;      //Offset to Object Description
     DWORD lInstancing;            //Instancing Mode
     DWORD lObjectID;              //Current Object ID in the Project
     BYTE uuidObjectClsID[15];      //CLSID of Object
     DWORD fIsInterface;            // Specifies if the next CLSID is valid
     DWORD oObjectClsID;            // Offset to CLSID of Object Interface
     DWORD oControlClsID;          // Offset to CLSID of Control Interface
     DWORD fIsControl;              // Specifies if the CLSID above is valid
     DWORD lMiscStatus;            // OLEMISC Flags (see MSDN docs)
     BYTE fClassType;              // Class Type
     BYTE fObjectType;              // Flag identifying the Object Type
     WORD iToolboxBitmap32;        // Control Bitmap ID in Toolbox
     WORD iDefaultIcon;            // Minimized Icon of Control Window
     WORD fIsDesigner;              // Specifies whether this is a Designer
     DWORD oDesignerData;          // Offset to Designer Data
     // sizeof() = 0x44
 }COMRegInfo_t;

 'Object Type part of tCOMRegInfo//没有翻译的意义，就没有翻译，只是看看而已
 '+-------+---------------+-------------------------------------------+
 '| Value | Name          | Description                               |
 '+-------+---------------+-------------------------------------------+
 '|  0x02 | Designer      | A Visual Basic Designer for an Add.in     |
 '|  0x10 | Class Module  | A Visual Basic Class                      |
 '|  0x20 | User Control  | A Visual Basic ActiveX User Control (OCX) |
 '|  0x80 | User Document | A Visual Basic User Document              |
 '+-------+---------------+-------------------------------------------+

 *7）ObjectInfo
 这些东西十分重要，是提取VB工程元素的重要标志，从这里我们可以得到这个工程里有多少FORM,多少MODULE,以及他们的重要索引数据，可以说重要性和SECTION HEAD在EXE程序中重要性相同。
 入口的地址是由ProjectInfo_t结构提供的(semi提供关于这里提供的资料少之又少，所以需要自己摸索）
 从ProjectInfo_t开始指向的是一个ObjectTable_t,由ObjectTable_t提供第一个OBJECT_t结构的地址

 typedef struct //这个是OBJECT 的总表，可以索引以后的每个OBJECT
 {
     DWORD lNull1 As Long;                                        //没有用的填充东西
     DWORD aExecProj;                                            //VA指向一块内存结构(研究下来既不没见着这个东西由什么用处
     DWORD aProjectInfo2;                                        //VA指向Project Info 2
     DWORD Const1;                                                //没有用的填充东西
     DWORD Null2;                                                //没有用的填充东西
     DWORD lpProjectObject As Long                                ' 0x14
     DWORD Flag1;                                                //标志1
     DWORD Flag2;                                                //标志2
     DWORD Flag3;                                                //标志3
     DWORD Flag4;                                                //标志4
     WORD fCompileType;                                          //Internal flag used during compilation
     WORD ObjectCount1;                                          //OBEJCT数量1????
     WORD iCompiledObjects;                                      //编译后OBJECT数量
     WORD iObjectsInUse As Integer;                              //Updated in the IDE to correspond the total number ' but will go up or down when initializing/unloading modules.
     DWORD aObject;                                              //VA指向第一个OBJECT_t结构，很重要
     DWORD Null3;                                                //没有用的填充东西
     DWORD Null4;                                                //没有用的填充东西
     DWORD Null5;                                                //没有用的填充东西
     DWORD aProjectName;                                          //执行工程名字的字符串
     DWORD LangID1;                                              //language ID1
     DWORD LangID2;                                              //language ID2
     DWORD Null6;                                                //没有用的填充东西
     DWORD Const3;                                                //没有用的填充东西
     ' 0x54
 }ObjectTable_t;

 type struct//这个就是每个OBJECT的结构，
 {
     DWORD aObjectInfo;          //VA 指向一个ObjectInfo_t类型,来显示这个OBJECT的数据
     DWORD Const1;                //没有用的填充东西
     DWORD aPublicBytes;          //VA 指向公用变量表大小
     DWORD aStaticBytes;          //VA 指向静态变量表地址
     DWORD aModulePublic;        //VA 指向公用变量表
     DWORD aModuleStatic;        //VA 指向静态变量表
     DWORD aObjectName;          //VA 字符串,这个OBJECT的名字
     DWORD ProcCount;            // events, funcs, subs(时间\函数\过程)数目
     DWORD aProcNamesArray;      //VA 一般都是0
     DWORD oStaticVar;            //OFFSET  从aModuleStatic指向的静态变量表偏移
     DWORD ObjectType;            //比较重要显示了这个OBJECT的实行,具体见下表
     DWORD Null3;                //没有用的填充东西
     //sizeof() = 0x30
 }Object_t;

 'Object_t.ObjectTyper 属性...//重要的属性表部分
 '#########################################################
 'form:              0000 0001 1000 0000 1000 0011 --&gt; 18083
 '                   0000 0001 1000 0000 1010 0011 --&gt; 180A3
 '                   0000 0001 1000 0000 1100 0011 --&gt; 180C3
 'module:            0000 0001 1000 0000 0000 0001 --&gt; 18001
 '                   0000 0001 1000 0000 0010 0001 --&gt; 18021
 'class:             0001 0001 1000 0000 0000 0011 --&gt; 118003
 '                   0001 0011 1000 0000 0000 0011 --&gt; 138003
 '                   0000 0001 1000 0000 0010 0011 --&gt; 18023
 '                   0000 0001 1000 1000 0000 0011 --&gt; 18803
 '                   0001 0001 1000 1000 0000 0011 --&gt; 118803
 'usercontrol:       0001 1101 1010 0000 0000 0011 --&gt; 1DA003
 '                   0001 1101 1010 0000 0010 0011 --&gt; 1DA023
 '                   0001 1101 1010 1000 0000 0011 --&gt; 1DA803
 'propertypage:      0001 0101 1000 0000 0000 0011 --&gt; 158003
 '                      | ||     |  |    | |    |
 '[moog]                | ||     |  |    | |    |
 'HasPublicInterface ---+ ||     |  |    | |    |  （有公用的接口）
 'HasPublicEvents --------+|     |  |    | |    |  （有公用的事件）
 'IsCreatable/Visible? ----+     |  |    | |    |  （是否可以创建，可见）
 'Same as "HasPublicEvents" -----+  |    | |    |  
 '[aLfa]                         |  |    | |    |
 'usercontrol (1) ---------------+  |    | |    |  （用户控制）
 'ocx/dll (1) ----------------------+    | |    |  （OCX/DLL）
 'form (1) ------------------------------+ |    |  （是不是FORM是就是1）
 'vb5 (1) ---------------------------------+    |  （是不是VB5是就是1）
 'HasOptInfo (1) -------------------------------+  （有没有额外的信息信息由就是1,决定是不是指向OptionalObjectInfo_t类似与PEHEAD里的Optional信息一样）
 '                                              |
 'module(0) ------------------------------------+   （如果是Module模块就这里是0）

 typedef struct//这个是显示这个OBJECT信息的结构,每一个OBJECT都有一个
 {
     WORD Flag1;
     WORD ObjectIndex;
     DWORD aObjectTable;
     DWORD Null1;
     DWORD aSmallRecord;        // when it is a module this value is -1 [better name?]
     DWORD Const1;
     DWORD Null2;
     DWORD aObject;
     DWORD RunTimeLoaded;      //[can someone verify this?]
     DWORD NumberOfProcs;
     DWORD aProcTable;
     WORD iConstantsCount;      // Number of Constants
     WORD iMaxConstants;        // Maximum Constants to allocate.
     DWORD Flag5;
     WORD Flag6;
     WORD Flag7;
     WORD aConstantPool;
     // sizeof() =  0x38
     'the rest is optional items[OptionalObjectInfo]
 }ObjectInfo_t;

 Private Type tOptionalObjectInfo ' if ((tObject.ObjectType AND &amp;amp;H80)=&amp;amp;H80)

     fDesigner As Long                                      ' 0x00 (0d) If this value is 2 then this object is a designer
     aObjectCLSID As Long                                   ' 0x04
     Null1 As Long                                          ' 0x08
     aGuidObjectGUI As Long                                 ' 0x0C
     lObjectDefaultIIDCount As Long                         ' 0x10  01 00 00 00
     aObjectEventsIIDTable As Long                          ' 0x14
     lObjectEventsIIDCount As Long                          ' 0x18
     aObjectDefaultIIDTable As Long                         ' 0x1C
     ControlCount As Long                                   ' 0x20
     aControlArray As Long                                  ' 0x24
     iEventCount As Integer                                 ' 0x28 (40d) Number of Events
     iPCodeCount As Integer                                 ' 0x2C
     oInitializeEvent As Integer                            ' 0x2C (44d) Offset to Initialize Event from aMethodLinkTable
     oTerminateEvent As Integer                             ' 0x2E (46d) Offset to Terminate Event from aMethodLinkTable
     aEventLinkArray As Long                                ' 0x30  Pointer to pointers of MethodLink
     aBasicClassObject As Long                              ' 0x34 Pointer to an in-memory
     Null3 As Long                                          ' 0x38
     Flag2 As Long                                          ' 0x3C usually null
     ' 0x40 &amp;lt;-- Structure size
 End Type

 Type tProjectInfo2
     lNull1                  As Long                        ' 0x00 (00d)
     aObjectTable            As Long                        ' 0x04 (04d) Pointer to Object Table
     lConst1                 As Long                        ' 0x08 (08d)
     lNull2                  As Long                        ' 0x0C (12d)
     aObjectDescriptorTable  As Long                        ' 0x10 (16d) Pointer to a table of ObjectDescriptors
     lNull3                  As Long                        ' 0x14 (20d)
     aNTSPrjDescription      As Long                        ' 0x18 (24d) Pointer to Project Description
     aNTSPrjHelpFile         As Long                        ' 0x1C (28d) Pointer to Project Help File
     lConst2                 As Long                        ' 0x20 (32d)
     lHelpContextID          As Long                        ' 0x24 (36d) Project Help Context ID
     ' 0x28 (40d) &lt;- Structure size
 End Type

 Type ObjectDescriptor
     lNull1      As Long                                    '0x00 (00d)
     aObjectInfo As Long                                    '0x04 (04d) Pointer to Object Info
     lConst1     As Long                                    '0x08 (08d)
     lNull2      As Long                                    '0x0C (12d)
     lFlag1      As Long                                    '0x10 (16d)
     lNull3      As Long                                    '0x14 (20d)
     aUnknown1   As Long                                    '0x18 (24d)
     lNull4      As Long                                    '0x1C (28d)
     aUnknown2   As Long                                    '0x20 (32d)
     aUnknown3   As Long                                    '0x24 (36d)
     aUnknown4   As Long                                    '0x28 (40d)
     lNull5      As Long                                    '0x2C (44d)
     lNull6      As Long                                    '0x30 (48d)
     lNull7      As Long                                    '0x34 (52d)
     lFlag2      As Long                                    '0x38 (56d)
     fObjectType As Long                                    '0x3C (60d) Flags for this Object
     '0x40 (64d) &lt;- Structure Size
 End Type</pre>
标 题: VB6反编译详解（二）
作 者: kenmark
时 间: 2006-07-20 10:57
链 接: [url]http://bbs.pediy.com/showthread.php?threadid=29307[/url]
详细信息:

****************************************************************************************************
上次说到最重要的部分OBJECT TABLE就断了实在是对不起大家，所以最近加紧研究马上出了第二篇（连载会很长哦，要考验大家的耐心的哦）
****************************************************************************************************

*7）ObjectInfo
这些东西十分重要，是提取VB工程元素的重要标志，从这里我们可以得到这个工程里有多少FORM,多少MODULE,以及他们的重要索引数据，可以说重要性和SECTION HEAD在EXE程序中重要性相同。
我们反编译只是初期水平，现在由于笔者能力有限，我们只讨论到如何完全反会MOD,VBP,FRX和FRM文件，关于其他文件例如.CLS类文件什么的不再本文讨论范围之内。
入口的地址是由ProjectInfo_t结构提供的(semi提供关于这里提供的资料少之又少，所以需要自己摸索）
从ProjectInfo_t开始指向的是一个ObjectTable_t,由ObjectTable_t提供第一个OBJECT_t结构的地址，索引其他的OBJECT和Import Table里索引IID一样。

typedef struct //这个是OBJECT 的总表，可以索引以后的每个OBJECT
{
DWORD lNull1 As Long;//没有用的填充东西
DWORD aExecProj;//VA指向一块内存结构(研究下来既不没见着这个东西由什么用处
DWORD aProjectInfo2;//VA指向Project Info 2
DWORD Const1;//没有用的填充东西
DWORD Null2;//没有用的填充东西
DWORD lpProjectObject As Long' 0x14
DWORD Flag1;//标志1
DWORD Flag2;//标志2
DWORD Flag3;//标志3
DWORD Flag4;//标志4
WORD fCompileType;  //Internal flag used during compilation
WORD ObjectCount1;  //OBEJCT数量1????
WORD iCompiledObjects;  //编译后OBJECT数量
WORD iObjectsInUse As Integer;  //Updated in the IDE to correspond the total number ' but will go up or down when initializing/unloading modules.
DWORD aObject;  //VA指向第一个OBJECT_t结构，很重要
DWORD Null3;//没有用的填充东西
DWORD Null4;//没有用的填充东西
DWORD Null5;//没有用的填充东西
DWORD aProjectName;  //执行工程名字的字符串
DWORD LangID1;  //language ID1
DWORD LangID2;  //language ID2
DWORD Null6;//没有用的填充东西
DWORD Const3;//没有用的填充东西
' 0x54
}ObjectTable_t;

type struct//这个就是每个OBJECT的结构，
{
DWORD aObjectInfo;  //VA 指向一个ObjectInfo_t类型,来显示这个OBJECT的数据
DWORD Const1;//没有用的填充东西
DWORD aPublicBytes;  //VA 指向公用变量表大小
DWORD aStaticBytes;  //VA 指向静态变量表地址
DWORD aModulePublic;//VA 指向公用变量表
DWORD aModuleStatic;//VA 指向静态变量表
DWORD aObjectName;  //VA 字符串,这个OBJECT的名字
DWORD ProcCount;// events, funcs, subs(事件\函数\过程)数目
DWORD aProcNamesArray;  //VA 一般都是0
DWORD oStaticVar;//OFFSET  从aModuleStatic指向的静态变量表偏移
DWORD ObjectType;//比较重要显示了这个OBJECT的实行,具体见下表
DWORD Null3;//没有用的填充东西
//sizeof() = 0x30
}Object_t;

'Object_t.ObjectTyper 属性...//重要的属性表部分
'#########################################################
'form:  0000 0001 1000 0000 1000 0011 --&gt; 18083
'   0000 0001 1000 0000 1010 0011 --&gt; 180A3
'   0000 0001 1000 0000 1100 0011 --&gt; 180C3
'module:0000 0001 1000 0000 0000 0001 --&gt; 18001
'   0000 0001 1000 0000 0010 0001 --&gt; 18021
'class: 0001 0001 1000 0000 0000 0011 --&gt; 118003
'   0001 0011 1000 0000 0000 0011 --&gt; 138003
'   0000 0001 1000 0000 0010 0011 --&gt; 18023
'   0000 0001 1000 1000 0000 0011 --&gt; 18803
'   0001 0001 1000 1000 0000 0011 --&gt; 118803
'usercontrol:   0001 1101 1010 0000 0000 0011 --&gt; 1DA003
'   0001 1101 1010 0000 0010 0011 --&gt; 1DA023
'   0001 1101 1010 1000 0000 0011 --&gt; 1DA803
'propertypage:  0001 0101 1000 0000 0000 0011 --&gt; 158003
'  | || |  || ||
'[moog]| || |  || ||
'HasPublicInterface ---+ || |  || ||  （有公用的接口）
'HasPublicEvents --------+| |  || ||  （有公用的事件）
'IsCreatable/Visible? ----+ |  || ||  （是否可以创建，可见）
'Same as "HasPublicEvents" -----+  || ||
'[aLfa] |  || ||
'usercontrol (1) ---------------+  || ||  （用户控制）
'ocx/dll (1) ----------------------+| ||  （OCX/DLL）
'form (1) ------------------------------+ ||  （是不是FORM是就是1）
'vb5 (1) ---------------------------------+|  （是不是VB5是就是1）
'HasOptInfo (1) -------------------------------+  （有没有额外的信息信息由就是1,决定是不是指向OptionalObjectInfo_t类似与PEHEAD里的Optional信息一样）
'  |
'module(0) ------------------------------------+   （如果是Module模块就这里是0）

typedef struct//这个是显示这个OBJECT信息的结构,每一个OBJECT都有一个
{
WORD Flag1;
WORD ObjectIndex;  //OBJECT的索引????????????????????????
DWORD aObjectTable;//指向OBJECT TABLE??????????????????
DWORD Null1;  //没有用的填充东西
DWORD aSmallRecord;// 如果这个对象是一个模块（module）那么这个数值是-1
DWORD Const1;  //没有用的填充东西
DWORD Null2;  //没有用的填充东西
DWORD aObject;//指向OBJECT??????????????????????????????
DWORD RunTimeLoaded;  //[can someone verify this?]
DWORD NumberOfProcs;  //proc个数
DWORD aProcTable;  //指向proc表
WORD iConstantsCount;  // 常量个数
WORD iMaxConstants;// 最大的要求分配的常量
DWORD Flag5;
WORD Flag6;
WORD Flag7;
WORD aConstantPool;//指向常量池
// sizeof() =  0x38
'the rest is optional items[OptionalObjectInfo]
}ObjectInfo_t;

typedef struct  // 这个是可选的OBJECT_INFO和PEHEADER里的OPTIONAL_HEADER类似，是否有要看每个Object_t里面的ObjectTyper表里的倒数第二个位（详细看上表）
{
DWORD fDesigner;// 如果这个数值是2则表示是一个designer
DWORD aObjectCLSID;  //指向CLSID对象
DWORD Null1;//没有用的填充东西
DWORD aGuidObjectGUI;//?????????????????????????????
DWORD lObjectDefaultIIDCount;//  01 00 00 00 ???????????????????????????
DWORD aObjectEventsIIDTable;//指向对象行为IID表
DWORD lObjectEventsIIDCount;//对象行为IID个数
DWORD aObjectDefaultIIDTable;//指向默认对象IID表
DWORD ControlCount;  //控件个数
DWORD aControlArray;//指向控件表
WORD iEventCount;// 行为的个数，比较重要，知道有几个行为
WORD iPCodeCount;// PCode个数
WORD oInitializeEvent;  // offset从aMethodLinkTable指向初始化行为
WORD oTerminateEvent；  // offset从aMethodLinkTable指向终止行为
DWORD aEventLinkArray;  //Pointer to pointers of MethodLink
DWORD aBasicClassObject;// Pointer to an in-memory
DWORD Null3;//没有用的填充东西
DWORD Flag2;//一般都是空的
//sizeof() = 0x40
}OptionalObjectInfo_t;

typedef struct
{
WORD Flag1;//Integer   ' 0x00
WORD EventCount;//Integer  ' 0x02
DWORD Flag2;//Long  ' 0x04
DWORD aGUID;//Long  ' 0x08
WORD index;//Integer   ' 0x0C
WORD Const1;//Integer  ' 0x0E
DWORD Null1;//Long  ' 0x10
DWORD Null2;//Long  ' 0x14
DWORD aEventTable;//Long' 0x18
BYTE Flag3;//Byte  ' 0x1C
BYTE Const2;//Byte ' 0x1D
WORD Const3;//Integer  ' 0x1E
DWORD aName;//Long  ' 0x20
WORD Index2;//Integer  ' 0x24
WORD Const1Copy;//Integer  ' 0x26
// 0x28  &amp;lt;-- Structure Size
}Control_t;

看到这里我们已经可以通过这个表粗略地勾画出那个工程是怎样的，通过PROJECT_INFO和其他的一些东西，我们已经能够重建VBP文件，而且我们已经知道这个工程到底有多少源文件（FRM,MOD不包括FRX）
接下来，我们来读取EXE文件中保存的数据来重建每个MOD,FRM,CLS,VBP

重建VBP
我们本文不是主要介绍VB工程组文件的内部结构，所以只是一笔带过，VBP文件内部格式和普通的配置文件类似都是(关键词,对应值)对的形式
主要有这么几个字段我们要注意（输出的东西和SEMI一样，关于VERSION会比SEMI详细，将在之后介绍）
Type=Exe（这个就是这个工程是什么我们当然是EXE）
Startup="frmMain"（这个就是工程的启动项目，关于它我们后面要详细介绍）
Description="Visual Basic Decompiler"（这个是描述，(char*)COMRegData+COMRegData-&gt;oNTSProjectDescription）
HelpFile=""（帮助文件(char*)VBHEADER+VBHEADER-&gt;oHelpFile就是其在文件中的偏移）
Name="VBDecompiler"（工程名字，获得方式和HELPFILE类似，只要看VBHEADER里面就有存储其偏移地址）
Title="VBDecompiler"（工程标题，获得方式和HELPFILE类似，只要看VBHEADER里面就有存储其偏移地址）
ExeName32="SemiVBDecompiler"（工程EXE32的名字，获得方式和HELPFILE类似，只要看VBHEADER里面就有存储其偏移地址）
注意:每找到一个OBJECT对应的文件,我们要将其添加到VBP工程里面,具体的方式是:
如果这个OBJECT是FRM文件,只要在VBP文件里面添加一条Form=窗体文件的名字（文件名）（没有空格要求=以后紧跟）
如果这个OBJECT是MOD文件,只要在VBP文件里面添加一条Module=内部标识名;文件名（之间需要一个分号来格开）
如果这个OBJECT是CLASS文件,只要在VBP文件里面添加一条Class=内部标识名;文件名（之间需要一个分号来格开）
这样的话，在VBP文件中维护各个工程文件的工作就大致完成了。
其他重要的项还有例如OBJECT项（添加使用的控件）以及版本信息相关的（在后面的资源反编译一节会着重介绍）
剩下的就是关于编译时的优化选项了，这些不是很重要，所以就没有加入。
其实VBP文件里包含了很多工程的细节，但是我们主要只要抓住重要的字段。

重建MOD
MOD文件比较简单，由于文件名可能和模块名不同，编译的时候舍弃了实际文件名，而用模块名来作为标识，所以我们生成的MOD文件的名字选用模块名，可能与原始的源文件组不同。
获得一个OBJECT之后，我们看Object_t.ObjectType通过查表我们能够确定它的性质
我们确定这个是MOD文件之后，我们通过Object_t.aObjectName（指向一个字符串）这个就是这个模块的名字，也是这个模块在编译后的文件中的标识。
我们用这个标识名来作为文件名，创建一个文件，然后我们通过Object_t.ProcCount知道这个MOD里存储着多少个FUNCTION和SUB，并且由于MOD是全编译，我们得不到具体的SUB和FUNCTION的名字，这些名字在编译的时候被丢弃的所以我们只能知道到底有多少个SUB和FUNCTION。
所以MOD的重建并不能得到什么东西，只能空创建一个文件然后最多写入：'There is totally 100 methods in this module.But we can't show you them.:)
还要记得一点就是我们要为这个BAS文件写上它的VB_ATTRIB,具体的格式就是Attribute VB_Name = "模块的名字(内部标识名)"
这部分的具体代码重建(一般来说不能完全重建)，我们只能有待更加强大的代码反编译来完成。

重建FRM
FRM和MOD不同的是我们可以得到FRM里面的所有控件的基本静态状态（属性），并且我们可以得到里面存储的SUB 和FUNCTION的名字（不同于MOD）
同样如何知道这个OBJECT是一个FRM文件还是要查表
知道它是一个FRM文件之后我们首先要了解一下FRM文件的结构,
FRM文件是类似配置文件的格式存储的，主要有外部的VB_ATTRIB定义以及FORM成员的定义
VERSION 5.00//文件的编译版本
Begin VB.Form 内部标识名（表示这个FORM的开始）
Begin VB.VB内部控件对象 内部标识名

End
End
Attribute VB_Name = "FORM的内部标识名"
多重的嵌套就完成了这个FORM里面控件的从属关系（例如一个PICTUREBOX从属与FORM，而一个TEXT从属于一个FRAME）
之后我们要得到这个FRM文件里面的控件信息和SUB以及FUNCTION的信息，来完成我们的FRM写入
每个OBJECT里有一个TYPE表示这个OBJECT的属性，我们已经知道凡是是一个FRM都是有OptionalObjectInfo
这个结构告诉我们很多元素的索引，获取的方式十分简单，只要先确定有OptionalObjectInfo然后它的地址就是这个Object的(char*)ObjectInfo+sizeof(ObjectInfo)其实就是紧紧跟在ObjectInfo之后的。
结构的描述已经在上面列出了，这个结构中的信息十分复杂，我们要注意这么几个项目
ControlCount  列出控件个数
aControlArray 指向控件表
控件表是一个紧紧挨着的一个指针数组，我们可以逐个读取获得信息
伪代码：
for (i = 0 ; i&lt;=this-&gt;ObjectTable-&gt;ObjectCount1 -1 ; ++i,++t)
{//循环获得所有的OBJECT的指针
s = this-&gt;Get_OptionalObjectInfo(t);  //获取一个OBJECT的OptionalObjectInfo
if (s==NULL)  // it is a module and haven't any optional object info
continue;
cout&lt;&lt;"One frm found!"&lt;&lt;endl;//打印出：获得一个FRM
ct = this-&gt;Get_Control_t(s);//获取第一个Control的指针
for (d = 0 ; d&lt;=s-&gt;ControlCount-1 ; d++)//循环获得每一个CONTROL的索引
cout&lt;&lt;"one control:"&lt;&lt;this-&gt;Get_Control_Name(ct++)&lt;&lt;endl;//获得了一个Control打印出它的名字
}

inline BYTE * VBEXE::Get_Control_Name(VBST::Control_t *x)
{
return (this-&gt;buffer + this-&gt;sf.VA2Offset(x-&gt;aName));
}
这里其实是我的DECOMPILER的一些片段，仅仅是用作调试的还未成型所以就不敢贴出来丢脸了：）。
每个Control_t的aName是一个VA指向一个字符串就是表示这个控件的名字，至于控件的属性我们以后再说：）
我们看看我的VBDECOMPILER输出的读取那个SEMI的DECOMPILER的东西
Load file sucessfully!
Get contols in object
One frm found!
one control:mnuFile
one control:mnuHelpAbout
one control:txtFinal
one control:mnuFileOpen
one control:mnuTools
one control:Label1
one control:Form
one control:Label2
one control:mnuFileRecent1
one control:mnuFileSep1
one control:mnuFileRecent2
one control:mnuFileRecent3
one control:mnuFileRecent4
one control:mnuFileSep2
one control:lblObjectName
one control:mnuOptions
one control:mnuFileDebugProcess
one control:mnuToolsPCodeProcedure
one control:txtCode
one control:tvProject
one control:StatusBar1
one control:sstViewFile
one control:fxgEXEInfo
one control:picPreview
one control:mnuFileAntiDecompiler
one control:mnuFileExportMemoryMap
one control:mnuFileGenerate
one control:lstMembers
one control:lstTypeInfos
one control:mnuFileSaveExe
one control:txtBuffer
one control:txtFunctions
one control:txtEditArray
one control:lblArrayEdit
one control:buffCodeAv
one control:buffCodeAp
one control:mnuFileExit
one control:txtResult
one control:cmdCancel
one control:txtStatus
one control:mnuHelp
one control:FrameStatus
One frm found!
one control:imgFlame
one control:lblTitle
one control:TmrLight
one control:tmrIcon
one control:Form
One frm found!
one control:chkShowOffsets
one control:chkSkipCOM
one control:chkDumpControls
one control:cmdClose
one control:chkPCODE
one control:chkShowColors
one control:Form
One frm found!
one control:Class
One frm found!
one control:Class
One frm found!
one control:lblTitle
one control:cmdClose
one control:Form
one control:Label1
one control:lstProcedures
one control:txtView

到这里，我们以及能够基本将原来的工程组还原出来了，但是仍然有缺点：
1）我们得不到（应该说实我水平不够）VBP文件里的Reference
2）我们即将讨论关于VERSION信息的东西（要从一个EXE的RESOURCE中看出端倪）
3）我们的CODE仍然还是一大问题
4）我们的STARTUP开始段，（马上会研究出来的）
5）我们还不知道哪个FRM有FRX文件哪个没有而且不能还原,而且FRM里面的控件还是不能获得属性
6）PCODE一点都没有讨论
7）还有一些细节的信息没能弄出来

下一篇会说到VERSION INFO和控件属性的东西

标 题: 答复
作 者: nbw
时 间: 2006-07-09 19:33
详细信息:

趁机也跟一个简单的。

VB6程序消息入口

by nbw

vb程序加载后是个：
00401160 &gt; $ 68 9C124000   push   0040129C
00401165   . E8 F0FFFFFF   call   &lt;jmp.&amp;MSVBVM60.#100&gt;
没法调试。

从回收站里把笔记拉了出来，再不贴上来恐怕下次又要重新分析了。

这里说一下处理方法：

打开VB6运行库，搜索下面代码：

733BAE50   8B7424 08   mov   esi, [esp+8]
733BAE54   57 push   edi
733BAE55   8B40 10 mov   eax, [eax+10]
733BAE58   8B0E   mov   ecx, [esi]
733BAE5A   8B7C81 0C   mov   edi, [ecx+eax*4+C]   //这里edi指向消息入口
733BAE5E   . 57   push   edi ; Project1.00401470

最后的edi是消息入口地址。

比如刚加载程序的时候，断在这里，edi就是程序的 Load 函数。

如果点一下程序上的按钮，也会断在这里，这时候edi就是这个按钮的click函数地址。

这些地址都是在一起的，

比如：

00401470   . 816C24 04 330&gt;sub   dword ptr [esp+4], 33  // 这个是app.load
00401478   . E9 93040000   jmp   00401910
sub   dword ptr [esp+4], xx   // 这个可能是timer
jmp   xxxxxxxx

需要注意的是sub命令后面的参数不能理解为win32里面的消息，比如这里的0x33代表程序加载，换个程序可能33代表click。