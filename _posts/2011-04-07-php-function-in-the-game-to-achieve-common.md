---
ID: 912
post_title: 游戏中常见功能的PHP实现
author: ChinaBUG
post_excerpt: |
  　　许多游戏和游戏系统都需要骰子。让我们先从简单的部分入手：掷一个六面骰子。实际上，滚动一个六面骰子就是从 1 到 6 之间选择一个随机数字。在 PHP 中，这十分简单：echo rand(1,6);。
  　　在许多情况下，这基本上很简单。但是在处理机率游戏时，我们需要一些更好的实现。PHP 提供了更好的随机数字生成器：mt_rand()。在不深入研究两者差别的情况下，可以认为 mt_rand 是一个更快、更好的随机数字生成器：echo mt_rand(1,6);。如果把该随机数字生成器放入函数中，则效果会更好。
layout: post
permalink: >
  http://blog.ipodmp.com/2011/04/php-function-in-the-game-to-achieve-common.html
published: true
post_date: 2011-04-07 15:38:29
---
<p style="text-align: center;">2011-02-09</p>
<strong>简单的掷骰器</strong>

　　许多游戏和游戏系统都需要骰子。让我们先从简单的部分入手：掷一个六面骰子。实际上，滚动一个六面骰子就是从 1 到 6 之间选择一个随机数字。在 PHP 中，这十分简单：echo rand(1,6);。

　　在许多情况下，这基本上很简单。但是在处理机率游戏时，我们需要一些更好的实现。PHP 提供了更好的随机数字生成器：mt_rand()。在不深入研究两者差别的情况下，可以认为 mt_rand 是一个更快、更好的随机数字生成器：echo mt_rand(1,6);。如果把该随机数字生成器放入函数中，则效果会更好。

1. 使用 mt_rand() 随机数字生成器函数：

function roll () { return mt_rand(1,6);  } 

echo roll();

然后可以把需要滚动的骰子类型作为参数传递给函数。

2. 将骰子类型作为参数传递：

function roll ($sides) { return mt_rand(1,$sides); }

echo roll(6);  // roll a six-sided die 

echo roll(10);  // roll a ten-sided die 

6 echo roll(20);  // roll a twenty-sided die

　　从这里开始，我们可以继续根据需要一次滚动多个骰子，返回结果数组；也可以一次性滚动多个不同类型的骰子。但是大多数任务都可以使用这个简单的脚本。

<strong>随机名称生成器</strong>

　　如果正在运行游戏、编写故事或者一次性创建大批字符，有时会疲于应付不断出现的新名字。让我们看一看可用于解决此问题的一个简单随机名称生成器。首先，让我们创建两个简单数组 — 一个用于名字，一个用于姓氏。

3. 名字和姓氏的两个简单数组：

$male = array( "William", "Henry", "Filbert", "John", "Pat", ); 

$last = array( "Smith", "Jones", "Winkler", "Cooper", "Cline", );

　　然后就可以从每个数组中选择一个随机元素：echo $male[array_rand($male)] . ' ' . $last[array_rand($last)];。要一次性提取多个名称，只需混合数组并根据需要提取。

4. 混合名称数组

shuffle($male); 

shuffle($last); 

for ($i = 0; $i &lt;= 3; $i++) { 

echo $male[$i] . ' ' . $last[$i]; 

}

　　基于此基本概念，我们可以创建保存名字和姓氏的文本文件。如果在文本文件的每一行中存放一个名字，则可以轻松地用换行符分隔文件内容以构建源代码数组。

5. 创建名称的文本文件

$male = explode('\n', file_get_contents('names.female.txt')); 

$last = explode('\n', file_get_contents('names.last.txt'));

　　构建或查找一些好的名字文件（代码归档 中附带了一些文件），此后我们绝不再需要为名字烦恼。

<strong>场景生成器</strong>

　　利用构建名字生成器使用的相同基本原理，我们可以构建场景生成器。此生成器不但在角色扮演游戏中十分有用，而且在需要用到伪随机环境集合（可用于角色扮演、即兴创作、写作等情况）的情况下也十分有用。我最喜欢的游戏之一，Paranoia 在其 GM Pack 中包括了 "任务混合器（mission blender）"。任务混合器可用于在快速滚动骰子时整合完整任务。让我们整合自己的场景生成器。

　　考虑以下场景：您醒来后发现自己迷失于丛林中。您知道自己必须赶去纽约，但是不知道原因。您可以听到附近的狗叫声及清晰的敌方搜寻者的声音。您浑身发冷、不住颤抖，而且没有武器。该场景中的每一句话都介绍场景的特定方面：

　　"您醒来后发现自己迷失于丛林中" — 这句话将建立设置。
　　"您知道自己必须赶去纽约" — 这句话将描述目标。
　　"您可以听到狗叫声" — 这句话将介绍敌人。
　　"您浑身发冷、不住颤抖，而且没有武器" — 这句话将添加复杂度。
　　就像创建名字和姓氏的文本文件一样，首先分别创建设置、目标、敌人和复杂度的文本文件。代码归档中附带了样例文件。在拥有这些文件后，生成场景的代码与生成名称的代码基本相同。

6. 生成场景

$settings = explode("\n", file_get_contents('scenario.settings.txt')); 

$objectives = explode("\n", file_get_contents('scenario.objectives.txt')); 

$antagonists = explode("\n", file_get_contents('scenario.antagonists.txt')); 

$complicati**** = explode("\n", file_get_contents('scenario.complicati****.txt')); 

shuffle($settings); 

shuffle($objectives); 

shuffle($antagonists); 

shuffle($complicati****); 

echo $settings[0] . ' ' . $objectives[0] . ' ' . $antagonists[0] . ' '. $complicati****[0] . "&lt;br /&gt;\n";

　　我们可以通过添加新文本文件向场景中添加元素，也可能希望添加多重复杂度。添加到基本文本文件中的内容越多，场景随时间的变化就越多。

<strong>牌组创建器（Deck builder）和装备（shuffler）</strong>

　　如果您要玩纸牌并且要处理与纸牌相关的脚本，我们需要用装备中的工具整合一副牌组构建器。首先，让我们构建一副标准纸牌。需要构建两个数组 — 一个用于保存同花色的组牌，而另一个用于保存牌面。如果稍后需要添加新组牌或牌类型，则这样做将获得很好的灵活性。

7. 构建一副标准扑克牌

$suits = array ( "Spades", "Hearts", "Clubs", "Diamonds"); 

$faces = array ( "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Jack", "Queen", "King", "Ace");

　　然后构建一副牌数组来保存所有纸牌值。只需使用一对 foreach 循环即可完成此操作。

8. 构建一副牌数组

$deck = array(); 

foreach ($suits as $suit) { 

foreach ($faces as $face) { 

$deck[] = array ("face"=&gt;$face, "suit"=&gt;$suit); 

} 

}

　　在构建了一副扑克牌数组后，我们可以轻松地洗牌并随机抽出一张牌。

9. 洗牌并随机抽出一张牌

shuffle($deck); 

$card = array_shift($deck); 

echo $card['face'] . ' of ' . $card['suit'];

　　现在，我们就获得了抽取多副牌或构建多层牌盒（multideck shoe）的捷径。

<strong>胜率计算器：发牌</strong>

　　由于构建扑克牌时会分别跟踪每张牌的牌面和花色，因此可以通过编程方式利用这副牌来计算得到特定牌的几率。首先每只手分别抽出五张牌。

10. 每只手抽出五张牌

$hands = array(1 =&gt; array(), 2=&gt;array()); 

for ($i = 0; $i &lt; 5; $i++) { 

     $hands[1][] = implode(" of ", array_shift($deck)); 

     $hands[2][] = implode(" of ", array_shift($deck)); 

}

　　然后可以查看这副牌，看看剩余多少张牌以及抽到特定牌的机率是多少。查看剩余的牌数十分简单。只需要计算 $deck 数组中包含的元素数。

　　要获得抽到特定牌的机率，我们需要一个函数来遍历整副牌并估算其余牌以查看是否匹配。

<strong>11. 计算抽到特定牌的几率</strong>

function calculate_odds($draw, $deck) { 

 $remaining = count($deck); 

 $odds = 0; 

 foreach ($deck as $card) { 

 if (  ($draw['face'] == $card['face'] &amp;&amp; $draw['suit'] ==  $card['suit'] ) ||  ($draw['face'] == '' &amp;&amp; $draw['suit'] == $card['suit'] ) ||  ($draw['face'] == $card['face'] &amp;&amp; $draw['suit'] == '' ) ) { 

 $odds++; 

} } 

 return $odds . ' in ' $remaining; 

 }

　　现在可以选出尝试抽出的牌。为了简单起见，传入看上去类似某张牌的数组。我们可以查找特定的一张牌。

12. 查找指定的一张牌

$draw = array('face' =&gt; 'Ace', 'suit' =&gt; 'Spades'); 

 echo implode(" of ", $draw) . ' : ' . calculate_odds($draw, $deck);

　　或者可以查找指定牌面或花色的牌。

<strong>13. 查找指定牌面或花色的牌</strong>

$draw = array('face' =&gt; '', 'suit' =&gt; 'Spades'); 

$draw = array('face' =&gt; 'Ace', 'suit' =&gt; '');

<strong>简单的扑克发牌器</strong>

　　现在已经得到牌组构建器和一些工具，可以帮助计算出抽出特定卡的机率，我们可以整合一个真正简单的发牌器来进行发牌。出于本例的目的，我们将构建一个可以抽出五张牌的发牌器。发牌器将从整副牌中提供五张牌。使用数字指定需要放弃哪些牌，并且发牌器将用一副牌中的其他牌替换这些牌。我们无需指定发牌限制或特殊规则，但是您可能会发现这些是非常有益的个人经验。

　　如上一节所示，生成并洗牌，然后每只手五张牌。按数组索引显示这些牌，以便可以指定返回哪些牌。您可以使用表示要替换哪些牌的复选框来完成此操作。

<strong>14. 使用复选框表示要替换的牌</strong>

foreach ($hand as $index =&gt;$card) { 

 echo "&lt;input type='checkbox' name='card[" . $index . "]'&gt;  " . $card['face'] . ' of ' . $card['suit'] . "&lt;br /&gt;"; 

 }

　　然后，计算输入 array $_POST['card']，查看哪些牌已被选择用于替换。

<strong>15. 计算输入</strong>

 $i = 0; 

 while ($i &lt; 5) { 

 if (isset($_POST['card'][$i])) { 

 $hand[$i] = array_shift($deck); 

 }  }

　　使用此脚本，您可以尝试找到处理特定一组牌的最佳方法。

<strong>Hangman 游戏</strong>

　　Hangman 实质上是一款猜字游戏。给定单词的长度，我们使用有限的几次机会猜这个单词。如果猜出了出现在该单词中的一个字母，则填充该字母出现的所有位置。在猜错若干次（通常为六次）后，您就输了比赛。要构建一个简陋的 hangman 游戏，我们需要从单词列表开始。现在，让我们把单词列表制作成一个简单的数组。

<strong>16. 创建单词列表</strong>

$words = array ( "giants",  "triangle",  "particle",  "birdhouse",  "minimum",  "flood" );

　　使用前面介绍的技术，我们可以把这些单词移动到外部单词列表文本文件中，然后根据需要导入。

　　在得到单词列表后，需要随机选出一个单词，将每个字母显示为空，然后开始猜测。我们需要在每次进行猜测时跟踪正确和错误的猜测。只需序列化猜测数组并在每次猜测时传递它们，就可实现跟踪目的。如果需要阻止人们通过查看页面源代码侥幸猜对，则需要执行一些更安全的操作。

　　构建数组以保存字母和正确/错误的猜测。对于正确的猜测，我们将用字母作为键并用句点作为值填充数组。

17. 构建保存字母和猜测结果的数组

$letters = array('a','b','c','d','e','f','g','h','i','j','k','l','m','n','o',  'p','q','r','s','t','u','v','w','x','y','z'); 

$right = array_fill_keys($letters, '.'); 

$wrong = array();

　　现在需要一些代码来评估猜测并在完成猜字游戏的过程中显示该单词。

18. 评估猜测并显示进度

if (stristr($word, $guess)) { 

 $show = ''; 

 $right[$guess] = $guess; 

 $wordletters = str_split($word); 

 foreach ($wordletters as $letter) { 

 $show .= $right[$letter]; 

 }  } else { 

 $show = ''; 

 $wrong[$guess] = $guess; 

 if (count($wrong) == 6) { 

 $show = $word; 

 } else { 

 foreach ($wordletters as $letter) { 

 $show .= $right[$letter]; 

 }  }  }

　　在源代码归档 中，可以看到如何序列化猜测数组并将该数组从一次猜测传递到另一次猜测中。

<strong>纵横字谜助手</strong>

　　我知道这样做不合适，但是有时在玩纵横拼字谜时，您不得不费劲地找出以 C 开头并以 T 结尾、包含五个字母的单词。使用为 Hangman 游戏构建的相同单词列表，我们可以轻松地搜索符合某个模式的单词。首先，找到一种传输单词的方法。为了简单起见，用句点替换缺少的字母：$guess = "c...t";。由于正则表达式将把句点处理为单个字符，因此我们可以轻松地遍历单词列表以查找匹配。

<strong>19. 遍历单词列表</strong>

foreach ($words as $word) { 

 if (preg_match("/^" . $_POST['guess'] . "$/",$word)) { 

 echo $word . "&lt;br /&gt;\n"; 

 }  }

　　根据单词列表的质量及猜测的准确度，我们应当能够得到合理的单词列表以用于可能的匹配。您必须自己决定 "表示 '不按规则玩' 的由五个字母组成的单词" 的谜底是 "chest" 还是 "cheat"。

<strong>米德里比斯</strong>

　　米德里比斯是一款文字游戏，玩家在游戏中得到一个简短的故事并用同一类型的不同单词替换主要类型的单词，从而创建同一个故事的更无聊的新版本。阅读以下文本："I was walking in the park when I found a lake. I jumped in and swallowed too much water. I had to go to the hospital." 开始用其他单词标记替换单词类型。开始和结束标记带有下划线用于阻止意外的字符串匹配。

<strong>20. 用单词标记替换单词类型</strong>

$text = "I was _VERB_ing in the _PLACE_ when I found a _NOUN_. I _VERB_ed in, and _VERB_ed too much _NOUN_.  I had to go to the _PLACE_.";

　　接下来，创建几个基本单词列表。对于本例，我们也不会做得太复杂。

21. 创建几个基本单词列表

$verbs = array('pump', 'jump', 'walk', 'swallow', 'crawl', 'wail', 'roll'); 

$places = array('park', 'hospital', 'arctic', 'ocean', 'grocery', 'basement',  'attic', 'sewer'); 

 $nouns = array('water', 'lake', 'spit', 'foot', 'worm',  'dirt', 'river', 'wankel rotary engine');

　　现在可以重复地评估文本来根据需要替换标记。

<strong>22. 评估文本</strong>

 while (preg_match("/(_VERB_)|(_PLACE_)|(_NOUN_)/", $text, $matches)) { 

 switch ($matches[0]) { 

 case '_VERB_' : 

 shuffle($verbs); 

 $text = preg_replace($matches[0], current($verbs), $text, 1); 

 break; 

 case '_PLACE_' : 

 shuffle($places); 

 $text = preg_replace($matches[0], current($places), $text, 1); 

 break; 

 case '_NOUN_' : 

 shuffle($nouns); 

 $text = preg_replace($matches[0], current($nouns), $text, 1); 

 break; 

 } 

 } 

 echo $text;

　　很明显，这是一个简单而粗糙的示例。单词列表越精确，并且花在基本文本上的时间越多，结果就越好。我们已经使用了文本文件创建名称列表及基本单词列表。使用相同原则，我们可以创建按类型划分的单词列表并使用这些单词列表创建更加变化多端的米德里比斯游戏。

<strong>乐透机</strong>

　　全部选中乐透的六个正确号码 —— 退一步说 —— 在统计学上是不可能的。不过，许多人仍然花钱去玩，而且如果您喜欢号码，则查看趋势图可能很有趣。让我们构建一个脚本，该脚本将允许跟踪赢奖号码并在列表中提供选择次数最少的 6 个号码。（免责声明：这不会帮助您中乐透奖，因此请不要花钱购买奖券。这只是为了娱乐）。

　　把赢奖的乐透选择保存到文本文件中。用逗号分隔各个号码并把每组号码放在单独一行中。使用换行符分隔文件内容并使用逗号分隔行后，可以得到类似清单 23 的内容。

<strong>23. 把选择的赢奖乐透保存到文本文件中</strong>

$picks = array( 

 array('6', '10', '18', '21', '34', '40'), 

 array('2', '8', '13', '22', '30', '39'), 

 array('3', '9', '14', '25', '31', '35'), 

 array('11', '12', '16', '24', '36', '37'), 

 array('4', '7', '17', '26', '32', '33') 

 );

　　很明显，这不足以成为绘制统计数据的基本文件。但是它是一个开端，并且足以演示基本原理。

　　设置一个基本数组以保存选择范围。例如，如果选择 1 到 40 之间（例如，$numbers = array_fill(1,40,0);）的号码，则遍历我们的选择，递增相应的匹配值。

<strong>24. 遍历选择</strong>

foreach ($picks as $pick) { 

 foreach ($pick as $number) { 

 $numbers[$number]++; 

 }  }

　　最后，根据值将号码排序。此操作应当会把最少选择的号码放在数组的前部。

<strong>25. 根据值将号码排序</strong>

asort($numbers); 

 $pick = array_slice($numbers,0,6,true); 

 echo implode(',', array_keys($pick));

　　通过有规律地向包含中奖号码列表的文本文件添加实际的乐透中奖号码，可以发现选号的长期趋势。查看某些号码的出现频率十分有趣。

注：如需转载本文，请注明出处（原文链接），谢谢。更多精彩内容，请进入简明现代魔法首页。