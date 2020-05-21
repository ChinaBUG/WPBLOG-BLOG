---
ID: 3378
post_title: >
  2014，成为更好程序员的7个方法
author: ChinaBUG
post_excerpt: ""
layout: post
permalink: >
  http://blog.ipodmp.com/2014/04/2014-7-ways-to-become-a-better-programmer.html
published: true
post_date: 2014-04-02 09:37:42
---
<strong>1. 在怪罪其他东西之前先检查自己的代码</strong>

质疑一下你自己和他人的预设情况。来自不同供应商的工具，可能内置有不同的预设，也有可能相同的供应商提供不同的工具。

当有人想你报告一个你无法重复的问题之时，去看看他们做了些什么。他们可能会做一些你没有想到的事情，或者是按照不同的顺序来做那件事。

我的原则是，如果我遇到一个我无法避免的 bug 时，我会首先考虑是编译器的错误，然后我就会去检查堆栈是否被破坏了。这可以通过跟踪代码来实现，可以有效地移除问题。多线程问题是另一个绞尽脑汁也不容 易找到的错误来源，通常还伴随着机器的错误。当一个系统使用多线程的时候，所有的简单的代码的错误都会成倍地增长。不能依靠调试和单元测试去发现这样的兼 容性问题，所以简单的设计是最重要的。

总之，在你怪罪你的编译器之前，请记住福尔摩斯的忠告：“当你把所有的不可能都排除了，那么剩下的东西，无论他有多么的不可能，都必定是真相。”Dirk Gently 也说了类似的话。

——Allan Kelly

<strong>2. 持续学习</strong>

我们生活在一个有趣的时代。随着全球化的发展，你要知道有大量的人都能胜任你的工作。你需要不断地学习，以维持竞争力。否则，你会成为一个落伍的人，永远做着相同的工作，直到你不再被需要，或者这个工作被廉价外包给其他人的那一天。

因此，你打算做些什么呢？有些大方的老板会提供训练来拓宽你的技能。而其他的公司并不会给你空闲的时间和金钱去做任何的训练。所以为了工作的稳定，你需要为自己的教育负责。

这里是一些让你持续学习的方法清单。其中许多都能够随便在网上找到：
<ul>
	<li>阅读书籍、杂志、博客、Twitter 和其他网站。如果你想对一个目标进行更深入的研究，考虑添加一个邮件列表或新闻组</li>
	<li>如果你真想专注于某一种技术，那就动起手来——写一些代码</li>
	<li>成为行业的顶尖人物可能会妨碍你的学习，你得尽量与导师合作。虽然你可以从任何人那里学到东西，但是从那些比你更聪明或更有经验的人那里你能够学得更多。如果你不能找到一个导师，那就继续去找</li>
	<li>使用虚拟的导师。在网上找一些作者或者开发人员，他们写的东西你都会喜欢并且都会看的。订阅他们的博客</li>
	<li>了解你所使用的框架和库。知道了他们是如何工作的，你使用起来就更得心应手。如果他们是开源的，你就很幸运了。使用调试器来单步执行，去观察他们内部是如何运作的。你将会看到那些真正聪明的人所编写和审阅的代码</li>
	<li>当你做错了或者是在修复 bug，或者是碰到一个问题的时候，尝试去深入了解到底发生了什么。有可能其他人也遇到了同样的问题，并且把 2 他发布在了网上。Google 这时候就非常有用了</li>
	<li>学习一样东西的一个好方法就是去传授和谈论它。当人们想要听你讲解并且想问你问题的时候，你就会更加积极地去学习。在工作中使用 lunch-’ n’-learn 方法，可以是一个用户组或者是一个本地的协会</li>
	<li>加入或者创办一个研究小组（社区的模式）或本地用户组，可以研究你们感兴趣的语言，技术或者是法律</li>
	<li>多去参加会议。如果你不能去，很多的会议都会把内容免费发布到网上的</li>
	<li>想要长期通勤？听一下博客吧</li>
	<li>你是否曾经在一个代码库上运行一个静态分析工具或者在你的 IDE 里看到一些警告？弄明白他们报告的是什么，为什么要报告</li>
	<li>遵循高效程序员的建议，每年学习一门新的语言。至少学习一门新的技术或者是一个新的工具。弄一个分支出来添加上你的想法，以便你能够在你目前的知识库里使用</li>
	<li>并不是你应该学的每一样东西都必须跟技术有关。学习你工作领域的一些东西，能够让你更加了解需求，并且能够给帮助你解决一些商业问题。学习如何提高工作效率，学习怎样更高效低工作是一个不错的选择</li>
	<li>返回学校</li>
</ul>
如果你有《黑客帝国》里的尼奥那样的能力就好了，能够直接下载我们需要的东西到大脑里面去。但是我们并没有，所以必须花费一定的时间去学习。你不必每时每刻都在学习。一点点时间足以，比如一周一次，有总比没有好。我们总得有一些工作之外的生活。

科技发展如此迅速，我们不要被甩在后面了。

——Clint Shank

<strong>3. 不要害怕破坏某些东西</strong>

每一个具有行业经验的人无疑曾在一个充满不稳定性的项目中工作过。这个系统是很难<a title="重构:改善既有代码的设计" href="http://www.amazon.cn/gp/product/B003BY6PLK/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&amp;tag=vastwork-23&amp;linkCode=as2&amp;camp=536&amp;creative=3200&amp;creativeASIN=B003BY6PLK" target="_blank" rel="nofollow">重构</a>的， 通常改变一个地方就会触及到另一个不相关的地方。每当要添加一个模块的时候，程序员的目标都是尽量少改动，在每一个版本中都是小心翼翼的。这就和把建造摩 天大楼当做搭积木一样，容易造成灾难。修改对的时候是非常伤脑筋的，因为系统已经生病了。它需要一个医生，否则状况就会越来越差。虽然你已经知道了你系统 发生了什么错误，但是你还是害怕“打破鸡蛋去煮你的煎蛋卷”。一个熟练的医生知道，为了做手术就必须开刀，而且她也知道开刀只是暂时的，而且很快就会愈 合。对于最初的疼痛来说，做手术是非常有价值的，患者通常都会获得比做手术前更好的状态。

不要去担心你的代码。当你在做事的时候如果暂时被打断，谁会去担心呢？对改变的恐惧会让你的项目将进入这样的状态。花一些时间去重构项目会让你节约 很多的时间。还有一个额外的好处就是一个团队面对这个损坏的系统的处理经验会让你们明白该怎样才能让它正常工作。要学会运用这些知识，而不是抵触他们。每 个人都不应该把时间花在自己所讨厌的东西上。重新定义内部接口，重组模块，重构、复制、粘贴代码，并通过减少依赖来简化设计。你可以通过消除极端情况来减 少代码的复杂度，他们通常会产生不当的耦合性。慢慢地将旧架构过渡到新的架构，边改边测试。试图在一个可能产生很多问题的大项目上进行一次大的重构，这些 问题可能慧然你在中途就放弃之前所作的所有的努力。

作为一个医生，是不应该害怕切除患病的部位，以留出愈合的空间。态度是会传染的，并且会激励其他人去对那些一直拖延着的项目进行修改。去列出一个团 队都感觉良好的项目的清单。虽然这些任务可能不会产生明显的效果，但你得去说服管理层，他们就会减少开支，加速对新版本的开发。永远不要停止关心代码的总 体“健康度”。

——Mike Lewis

<strong>4. 做专业的程序员</strong>

一个专业的程序员最重要的特征就是个人责任感。专业的程序员会对自己的生涯、自己的估计、自己的日程安排、自己的错误以及自己的作品负责。一个专业的程序员是不会把这些责任推给其他人的。

如果你是一个专业人员，那么你就会对自己的工作负责。你有责任阅读和学习。你有责任追赶业界及技术的潮流。而很多程序员都认为这是他们上司的工作。 对不起，这是大错特错的。你认为医生也会那样做吗？你认为律师也是那样的吗？不是的，他们会利用自己的时间和金钱去学习。他们花费大量的下班时间去阅读期 刊和做出计划。他们会时刻更新自己，我们也必须这样做。你和雇主之间的关系只是为了履行合同。总之：你的雇主承诺给你工资，你就得承诺去把这份工作做好。

专业的程序员会对他们编写的代码负责。如果他们不清楚代码是否会正常的工作，就绝不会轻易放出代码。试想一下，如果打算放出一个不确定的代码，你还 有可能是一个专业的程序员吗？专业的程序员都不希望 QA 来发现他们的错误，因为他们如果不经严格测试是不会放出代码的。当然，QA 也许会找到一些问题，因为没有什么是完美的嘛。但是作为专业人士，重要的是我们的态度，我们决不能让 QA 找到什么问题。

专业人士都是团队合作。他们会对整个团队的未来负责，这并不是他们个人的工作。他们互相帮助，彼此教导，互相学习，甚至包括别人需要的任何时候。当一个队友倒下，其他人都会去关心，因为他们知道他们都有互相需要的时候。

专业的人士是不会容忍一大串 bug 列表的。一个巨大的 bug 清单是非常粗心的。一个在问题跟踪数据库里有成百上千问题的系统是粗心酿成的悲剧。事实上，在大多数的项目中，如果非常依赖问题跟踪系统，那么他们肯定总 是粗心大意的。只有非常大的系统才可能会又这么长的 bug 清单，这个时候需要的是自动化的管理。

专业人士不会把事情弄得一团糟，他们会对自己的工作引以为豪。他们保持代码的整洁，结构的良好，而且便于阅读。他们跟随着默认的标准而且做出了很好 的实践。他们永远不会趋之若鹜。假设你能够在医生给你做开放式心脏手术的时候灵魂出窍。这个医生有一个最后期限（只是字面意义上的）。他必须在心肺循环功 能损失过量血细胞之前完成。你觉得他该怎么做？你是想要他们像典型的软件开发人员那样匆忙而且混乱吗？或者想要他们说“我待会儿再回来解决”？还是你要他 们小心地遵循着纪律，抓紧时间，相信他自己的做法是目前可以采取的最好的方法。你是想要一片混乱还是专业精神呢？

专业人员得有责任感。他们会对自己的事业负责。他们会对代码的正常运行负责。他们对自己工作的质量负责。即使最后期限迫在眉睫，他们也不会放弃自己的原则。事实上，当压力越来越大的时候，专业人员甚至会对这些原则要求得更紧，因为他们认为这是对的。

——Robert C. Martin (Uncle Bob)

<strong>5. 利用代码分析工具</strong>

测试的价值是在他们编程之旅的早期阶段就灌输给开发者的。今年来，单元测试，<a title="测试驱动开发" href="http://www.amazon.cn/dp/B0011AP332/?tag=vastwork-23" target="_blank" rel="nofollow">测试驱动开发</a>，以及敏捷方法的兴起都被大量地用于开发周期的每一个过程。然而，测试只是众多能够提高代码质量的工具之一。

回到早期阶段，当C语言还是一个新兴的技术的时候，CPU 的时间和存储的形式都是非常珍贵的。第一个C语言编译器注意到了这一点，所以通过一些语义分析减少了便利代码的次数。这意味着在编译阶段，只能检测到一小 部分的错误。为了弥补这个，Stephen Johnson 编写了一个叫做 lint 的工具，这个工具能够取出你的代码中的一些冗余，实现了在其相似的C编译器中已经去除的静态分析。然而，静态分析工具，会增加大量的无用警告或者是一些关 于文体问题的不必要的警告。

当前，语言、编译器和静态分析工具的情况是非常不同的。内存和 CPU 时间现在也变得非常的便宜，所以编译器能够承担更多的错误检测。几乎每一种语言都至少拥有一个工具来检查违规的格式和常见的问题，不过有时，那些隐含的错 误并不会被检测到，比如潜在的空指针引用。对于更复杂的工具，比如针对C的 SPlint，针对 Python 的 Pylint，都是可配置的，也就是说，你可以通过一个配置文件选择这个工具在命令行或者是 IDe 里要发出什么错误和警告。SPlint 甚至会让你在注释里注释你的代码，以给别人更多关于程序运行的提示。

如果一切都失败了，你发现你自己正在寻找一些你的编译器或 IDE 或 lint 工具没有捕获的简单的 bug 或者是一些违规行为，你就得收起你所有的静态分析工具。这并不像听起来那么困难。大多数编程语言，尤其是那些声称是动态的语言，都会把他们的抽象语法树和 编译工具作为其标准库的一部分。去了解你正在使用的这个语言的开发团队的标准库的细节是非常有意义的，因为这样你就能发现一些有价值的东西，这对于静态分 析和动态测试是非常有用的。比如：Python 标准库包含了一个反汇编程序，它会告诉你生成一些编译程序和目标程序的字节代码。这对编译器作者 python-dev 团队来说貌似是一个不起眼的工具，但是它实际上在日常工作中发挥着不同寻常的作用。这个库能够反汇编出来你最后一次堆栈跟踪的信息，这会给你恰当的反馈， 因为字节码指令会把最后一次未捕获的异常扔在那里。

所以，不要把测试放在质量保证工作的最后，利用好分析工具，不要害怕把自己的错误展示出来

——Sarah Mount

<strong>6. 和你的朋友一起使用 Ubuntu 哲学</strong>

所以很多时候，我们独立地编写代码，这些代码反映了我们个人对问题的理解，也反映了一个非常个性化的解决方案。我们可能会是团队的一部分，但是我们 仍然会是独立的，因为这是一个团队。我们很容易忘记这些独立编写的代码会被其他人所执行、使用、扩展和依赖。这是在开发软件中容易被忽略的社交的一面。创 造软件是一个混合了技术和社交的活动。我们只需要经常抬头，这样才会意识到我们并不是孤立地工作的，我们都有责任去提高个人成功的可能性，而不只是为了开 发团队。

你可以在孤立的环境下写出高质量的代码，但这样会失去自我。从一个角度来看，那是一个以自我为中心的方法（不是自大，而是自我）。这也是一个禅宗的 观点，他就是针对你编写代码那一过程的。我总是试着进入这个环节，因为它会让我离高质量更加接近，但那之后我就会陷入这个环节。我的团队现在处于什么环 节？我的环节和团队的是一样的吗？

在祖鲁语中，Ubuntu 的哲学被概括为“Umuntu ngumuntu ngabantu”，可以大致翻译为“A person is a person through (other) persons.”(人与人之间是互相联系的。我会变得更好因为是你，通过你的行为让我变得更好。在另一方面，当我做自己的事做得糟糕的时候你也会在你所 做的事情上变糟。对于开发者来说，我们可以这样理解“A developer is a developer throuth (other) developers.”(开发者与开发者之间是相互联系的)，也可以说“Code is code through (other) code.”（代码与代码之间是相互联系的）

我写的代码的质量会影响到你写的代码的质量。如果我的代码质量很差会怎样呢？即使你写了很整洁的代码，但由于你会使用我的代码，所以你的代码也会降 低到和我的代码质量差不多的地步。你可以使用很多模式和技术去降低损失，但是损失已经造成了。我建议你去做一些必须做的事之外的一些事情，这是因为当我在 做自己的事情的时候我并不会去考虑你。

我会认为我的代码是非常整洁，但我还是认为如果我使用 Ubuntu 哲学我可以做得更好。Ubuntu 哲学到底是什么？它看起来就像是一段良好的整洁的代码。它并不是简单的代码，而是一件艺术品。它是跟创造艺术有关的行为。和你的朋友一起使用 Ubuntu 哲学将会帮助你的团队守住你们的价值观，增强你们的原则。如果有其他人在任何情况下接触到你的代码，都会变成一个更加优秀的开发者。

禅宗是有关个人的。对于一群人来说，Ubuntu 也是一种禅宗。我们几乎不会看到只为自己而写的代码。

——Aslam Khan

<strong>7. 你必须关心你的代码</strong>

不用福尔摩斯我们就会知道好的程序员才能写出好的代码。糟糕的程序员嘛…就不会。他们会产生我们必须清理的垃圾。你想写出好的东西，是不是？那你其实就是想去做一个好的程序员。

优秀的代码并不会无中生有。它并不像行星对齐那样是靠运气才产生的。为了获得优秀的代码，你就得努力去争取。这有些辛苦。如果你真的关心优秀的代码你就会写出很好的代码。

优秀的程序并不单单来自技术能力。我曾见过一些有很高能力的程序员，他们能够写出给人很深印象的算法，他们把编程语言的标准烂熟于心，但是他们却写 出了最糟糕的代码。这些代码阅读起来非常痛苦，用起来也痛苦，修改起来也痛苦。我也曾见过更多谦卑的程序员，他们坚持写出更加简单的代码，他们写出来非常 优雅非常富有表现力的程序，和他们工作简直就是享受。

根据我在软件行业多年的经验，我得出了这样的结论，一般的程序员和伟大的程序员之间真正的区别是：态度。优秀的程序使用了专业的方法，并在现实世界的约束和软件产业压力之下尽量写出最好的软件。

代码的铺就都得有一个良好的计划。要想成为一个优秀的程序员，你就必须做出很好的计划，并且真正关心起代码——培养积极的观点，养成良好的态度。伟大的代码是由工匠大师精心打造的，绝不是由满湖的程序员或自称编码大师的程序员在不经意间就完成的。

——Pete Goodliffe

祝大家在 2014 年一切顺利！

原文链接： <a href="http://programming.oreilly.com/2014/01/7-ways-to-be-a-better-programmer-in-2014.html" target="_blank"><span style="color: #0099cc;">Amy Jollymore</span></a>   翻译： <a href="http://blog.jobbole.com/"><span style="color: #0099cc;">伯乐在线 </span></a>- <a href="http://blog.jobbole.com/author/haofly/"><span style="color: #0099cc;">haofly</span></a>

译文链接： <a href="http://blog.jobbole.com/62142/"><span style="color: #0099cc;">http://blog.jobbole.com/62142/</span></a>