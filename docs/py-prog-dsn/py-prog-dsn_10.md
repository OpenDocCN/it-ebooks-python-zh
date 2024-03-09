# 第九章 模拟与并发

# 第九章 模拟与并发

迄今为止，本书所讨论的计算具有两个特点：第一，计算是确定的，即只要输入相同， 程序执行后得到的结果总是一样的；第二，程序在任意时刻只做一件事，不能同时做多件事。 这是传统程序的典型特征。本章将介绍两种不属于这种典型形式的计算形式：一种是能够处 理随机现象的模拟方法，一种是能够同时做多件事的多线程并发。这两种计算形式的共同特 点是不确定性，即针对同样的输入，同一程序可能有不同的执行过程和结果。

# 9.1 模拟

## 9.1 模拟

现实中有很多问题，如果不利用计算机的话，就很难解决甚至不可能解决。例如天气预 报，古人只能通过肉眼看天来做预测，现代人则通过为大气过程建立数学模型并进行数值计 算来做预测，最新的理论更将确定性模型发展到不确定性模型，从而能对大气这个混沌系统 的行为做出更准确预报。这一切都有赖于计算机模拟（simulation）技术的应用，即利用计 算机为现实问题甚至假想问题建立模型，通过改变一些变量值或条件，来研究系统的行为， 获得原本无法获得的信息。模拟是利用计算机解决现实问题时的一个强大技术，除了天气预 报，还广泛用在风险分析、飞机设计、电影特效等很难直接解决真实问题的领域。

# 9.1.1 计算机建模

### 9.1.1 计算机建模

利用计算机解决现实中的问题，首先需要在计算机中将问题表示出来，这个过程称为建模（modeling），即建立描述现实问题的一个模型（model）。打个比方，用照相机拍摄自然 景物就是建模，即得到自然景物在照相机中的表示（数字图像）。不过照相机“建模”追求 的是模型必须反映自然景物的每一个细节，最好是一模一样。而用计算机为现实问题建模， 追求的是模型必须抽象出问题的关键特征，至于非关键的部分则可以忽略。如此得到的模型 比较简单，虽然不一定和现实很“像”，但足够支持解决问题。现实问题在计算机内的模型 通常都是数学模型，即利用数学公式或数学过程来描述现实问题。

下面我们写一个简单的程序，该程序的行为具有“混沌”现象的特征。所谓混沌现象， 是指在确定性系统中发生的看上去随机、不规则的运动，即用确定性理论描述的系统却表现 出不确定的行为。混沌现象的特征是不可预测性和对初始条件的极端敏感性。读者想必听说 过著名的“蝴蝶效应”：某处的一只蝴蝶扇动一下翅膀，这一扰动有可能导致很远的另一个 地方的天气出现非常大的变化。这个比喻想说的其实就是气象具有混沌行为。事实上，现实 生活和工程技术问题中，混沌现象是无处不在的。下面的程序虽然简单，却具有混沌现象不 可预测和对初始值敏感的两个特征。

【程序 9.1】chaos.py

```
def main():
    x = input("Enter a number between 0 and 1: ") 
    for i in range(10):
        x = 3.9 * x * (1 - x) 
        print x
main() 
```

运行这个程序，可得如下输出：

```
Enter a number between 0 and 1: 0.2
0.624
0.9150336
0.303213732397
0.823973143043
0.565661470088
0.958185428249
0.156257842027
0.514181182445
0.974215686851
0.0979659811419 
```

再次运行这个程序，但换一个输入数据 0.21，可得如下输出：

```
Enter a number between 0 and 1: 0.21
0.64701
0.89071343361
0.379637749907
0.918500422135
0.291943847024
0.806179285114
0.609391556931
0.928330600362
0.259478297495
0.749382311434 
```

从运行结果可以发现，尽管程序的代码是确定的，但输出的 10 个结果毫无规律，好像 完全是不可预测的。此外，比较两次运行的输出结果，可以发现初始输入数据的微小变化会 使输出结果很快变得显著不同。这两点正是混沌现象的特征。本程序之所以能够显现出混沌 特征，是因为程序中使用了计算公式 k*x*(1-x)，反复利用这个公式求值就会导致混沌。 换句话说，程序 chaos 利用数学公式 k*x*(1-x)为混沌现象建立了模型。

一旦用计算机程序为现实问题建立了模型，我们就可以通过运行程序并分析结果来探索 现实问题的性质。这时，模型的好坏是至关重要的，错误的程序自然会给出错误的结果，但 正确的程序也可能因为不准确的模型而产生错误的结果。

# 9.1.2 随机问题的建模与模拟

### 9.1.2 随机问题的建模与模拟

现实中有许多不确定的事件，称为随机事件。例如抛一枚硬币，结果是正面朝上还是反

面朝上，这是不确定的。研究随机事件的数学方法是统计，例如经过大量统计试验可以得出 结论：抛硬币时正面朝上和反面朝上的可能性是相等的，各占 50%。注意，说硬币正面朝 上和反面朝上的可能性各占 50%，并不意味着抛硬币试验将得到“正，反，正，反，…” 的结果，完全有可能出现一连串的正面或一连串的反面。

既然现实问题中存在随机事件，用计算机解决这类问题时就需要为随机事件建模，即程 序能够模拟随机事件的发生。例如，假设程序 P 能够模拟抛硬币并显示每次抛硬币的结果（“正”或“反”），则 P 应该具有这样的特性：每一次显示的结果是不可预测的，但多次运 行 P 之后“正”、“反”出现的次数应该是差不多的。可以设想 P 中有这样的语句：

```
if 模拟抛硬币的结果是正面: 
    print "正"
else:
    print "反" 
```

这里 if 后面的条件必须有时为真有时为假，但无法预测每一次运行时的真假；而多次运行 后，条件为真为假的次数应该基本相等。

我们知道，计算机是确定性的机器，它总是按照预定的指令行事。对于一个程序，只要 程序的初始输入是一样的，那么程序的运行结果就是确定的、可预测的。就拿程序 9.1 来说，尽管它产生了看上去不可预测的数值序列，但那并非真正的随机，因为按照程序 9.1 中的数 学式去计算，从相同的 x 开始必能得到完全一样的数值序列。所以，程序 9.1 所用的数学式 不能用于模拟随机事件，我们需要更好的能产生随机数的方法。

随机数生成函数

计算机科学家对随机数生成问题有很成熟的研究，他们设计了一些数学公式，通过计算

这些公式就能得到随机数。不过，既然是用确定的数学公式计算出来的数值，那就不可能是 数学意义上的随机，因此准确的名称实际上是“伪随机数”。伪随机数在实际应用中完全可 以当作真随机数来使用，因为它具有真随机数的统计分布特性，简单地说就是“看上去”是 随机的。

Python 语言提供了一个标准库模块 random，该模块中定义了若干种伪随机数生成函数。 这些伪随机数生成函数的计算与模块导入的时间（精度非常高）有关，因此每次运行函数所 产生的数值是不可预测的。random 模块中用得最多的随机数生成函数是 randrange 和 random。

randrange 函数生成一个给定范围内的整型伪随机数，范围由 randrange 的参数指定，具 体格式和 range 函数一样。例如，randrange(1,3)随机地产生 1 或 2，randrange(1,7)返回范围 [1,2,3,4,5,6]中的某个数值，而 randrange(2,100,2)返回小于 100 的某个偶数。例如：

```
>>> from random import randrange
>>> for i in range(20):
print randrange(1,3),
1 2 2 2 1 1 2 2 2 1 1 2 1 2 2 1 1 1 1 1
>>> for i in range(20):
print randrange(1,7),
1 5 1 5 2 3 2 2 3 2 4 2 6 3 1 2 1 1 1 5 
```

注意，由于试验次数较少（20 次），randrange 所生成的数值并未如我们期望的那样均匀分布， 但随着试验次数的增加，会发现 randrange 产生的值都具有差不多的出现次数。

random 函数可用来生成浮点型伪随机数，确切地说，该函数生成[0,1)区间中的浮点数。

random 不需要提供参数。例如：

```
>>> from random import random
>>> for i in range(5):
print random()
0.35382204835
0.997559174002
0.672268961152
0.889307826404
0.246100521527 
```

注意，random 模块名与 random 函数名恰巧相同，不要因此而误用。

模拟

有了随机数生成函数，我们就可以来模拟随机事件了。以抛硬币问题为例，前面我们给 出了如下形式的代码：

```
if 模拟抛硬币的结果是正面: 
    print "正"
else:
    print "反" 
```

并且指出 if 后面的条件的真假应该是不可预测、均匀分布的。

考虑 randrange(1,3)：该函数产生随机数 1 或 2，每一次调用到底生成什么值是不可预测 的，并且大量调用后两个数值出现的机会是一样的。据此，randrange(1,3) == 1 正是我们所 需要的条件，此条件每一次计算时的真假是随机的，但长远来看真假情形各占 50%。将这 个条件代入上面的条件语句，即得

```
if randrange(1,3) == 1: 
    print "正"
else:
    print "反" 
```

这样，我们就通过调用合适的随机数生成函数的方式模拟了随机事件，这种模拟方法称为蒙特卡洛方法。 类似地，掷骰子也是现实中常见的随机问题，如果希望在程序中模拟掷骰子，可以这样做：

```
value = randrange(1,7) print "你掷出的点数是:",value 
```

再看个例子，两个运动员打乒乓球，谁能赢呢？胜负自然取决于球员的技术水平，但又 并非水平高的人必然赢，毕竟体育比赛和天时地利人和等各种因素有关。既然比赛结果有随 机性，我们就可以利用蒙特卡洛方法来模拟比赛。假设 A、B 两个球员相互之间的胜率大致 是 55%对 45%，那么他们打一次比赛（比赛的单位可以是 1 分、1 局或 1 盘，在此并不重要） 的结果可以用如下代码模拟：

```
if random() &lt; 0.55: 
    print "A wins."
else:
    print "B wins." 
```

这里，由于 random()生成的随机数均匀地分布在[0,1)区间内，所以有 55%的值落在 0.55 的 左边，即 random() < 0.55 为真的可能性为 55%，为假（即 random() >= 0.55）的可能性为 45%， 这就恰当地模拟了 A 和 B 的胜率。注意，random() = 0.55 时应该算作 B 赢，因为 random() 生成的随机数包含 0 但不包含 1。

# 9.1.3 编程案例：乒乓球比赛模拟

### 9.1.3 编程案例：乒乓球比赛模拟

众所周知，中国乒乓球项目的技术水平世界第一，以至于所有比赛的冠军几乎都由中国球员包办。为了增强乒乓球运动的吸引力，提高其他国家的人对这项运动的兴趣，国际乒联 想了很多办法来削弱中国球员的绝对优势，例如扩大乒乓球的直径、禁用某些种类的球拍、 改变赛制等等。在本节中，我们将编写程序来模拟乒乓球比赛，以便研究一项针对中国球员 的规则改革是否真的有效。这项改革是：从 2001 年 9 月 1 日起，乒乓球比赛的每一局比分从 21 分改为 11 分。

球员技术水平的表示 乒乓球是两个球员之间的比赛，比赛开始后由一个球员发球，另一个球员将球接回来，然后两人交替击球，直至一方没能将球回到对方台上，这时另一方就得一分。一个球员有几 次发球机会，用完这些发球机会后将换发球。

比赛胜负由球员的技术水平决定，我们用两个球员对阵时各自的得分概率来表示他们的 技术水平。如果球员 A 与 B 水平相当，则 A 拿下 1 分的概率是 50%，B 拿下 1 分的概率也 是 50%；如果 A 水平较高，拿下 1 分的概率是 55%，则 B 拿下 1 分的概率就只有 45%了。 顺便指出，球员技术水平的表示方法并无一定之规，是由编程者自己主观确定的，关键 是表示方法要比较符合实际。我们这里采用的得分概率表示方法很简单，但没有考虑球员作 为发球方和接发球方的区别。读者可以考虑其他的表示方法，如：将球员的世界排名换算成 获胜概率，或者用球员作为发球方时的得分概率，或者用综合考虑发球得分概率和接发球得分概率的某个概率计算公式，等等。

模拟一回合比赛与得分

设 A、B 两球员比赛时，各自得分的概率为 prob 和 1-prob。利用蒙特卡洛方法，下面 的代码即模拟了得到 1 分的一回合比赛，这是整个模拟程序的核心功能。

```
if random() < prob: 
    pointA = pointA + 1
else:
    pointB = pointB + 1 
```

我们可以立刻来测试这个核心功能。假设 A 的得分概率是 0.55，让 A、B 进行 10000 分的较量，看看各自得分情况如何。测试代码如下：

```
>>> from random import random
>>> pointA = pointB = 0
>>> for i in range(10000): if random() &lt; 0.55:
pointA = pointA + 1 else:
pointB = pointB + 1
>>> print pointA,pointB 5430 4570 
```

最后得分差不多就是 55%比 45%，可见模拟比赛的结果确实反映了 A、B 双方的实力。

模拟一局比赛

乒乓球比赛不是按比赛回合来判定胜负的，而是采用将若干回合组成一局的方式，以局 为单位来判定胜负。老规则采用每局 21 分制，新规则采用每局 11 分制。我们利用上述模拟一回合比赛及得分的代码，改成以局为单位进行比赛（假设采用 21 分制）。

```
>>> def oneGame():
pointA = pointB = 0
while pointA != 21 and pointB != 21: if random() &lt; 0.55:
pointA = pointA + 1 else:
pointB = pointB + 1 return pointA, pointB 
```

函数 oneGame 模拟了 21 分制的一局比赛：只要还没人达到 21 分，就继续进行回合较量；

否则退出循环，并返回本句中 A 和 B 各自的得分 pointA 和 pointB。调用 oneGame 的人可以 比较这两个返回值的大小，以判断是谁赢了这一局。

下面我们来测试 oneGame 函数，让两个球员进行 1000 局较量，看看胜负如何。

```
>>> gameA = gameB = 0
>>> for i in range(1000):
        pointA, pointB = oneGame() 
        if pointA > pointB:
            gameA = gameA + 1 
        else:
            gameB = gameB + 1
>>> print gameA,gameB 
751 249 
```

出人意料的是，虽然 A、B 在每一回合的获胜概率相差不大（55%比 45%），但如果按每局 21 分制进行比赛的话，A 的胜局数遥遥领先于 B（75%比 25%）！这里面的道理是显然的， 每回合的胜负偶然性对 21 分一局的胜负来说影响减小了，得分能力稍强的人更加可能赢得一局。可以想象，如果将一局的得分减少，就像国际乒联所做得那样改成每局 11 分，那么 每回合的胜负偶然性对一局的胜负就会有较大影响。稍后我们将在程序中验证这一点。

模拟一场比赛

一场乒乓球比赛也不是无限制地打很多局才能定胜负，一般都是采取 3 局 2 胜、5 局 3 胜或 7 局 4 胜的方式来完成比赛。下面我们采用 21 分制、3 局 2 胜的赛制，来编写模拟一 场比赛的程序 oneMatch，并通过模拟 100 场比赛来测试 oneMatch。

```
>>> def oneMatch():
        gameOver = [(3,0),(0,3),(3,1),(1,3),(3,2),(2,3)]
        gameA = gameB = 0
        while not (gameA,gameB) in gameOver: 
            pointA, pointB = oneGame()
            if pointA > pointB: 
                gameA = gameA + 1
            else:
                gameB = gameB + 1 
        return gameA, gameB
>>> matchA = matchB = 0
>>> for i in range(100):
        gameA, gameB = oneMatch() 
        if gameA > gameB:
            matchA = matchA + 1
        else:
            matchB = matchB + 1
>>> print matchA,matchB 
89 11 
```

可见按 3 局 2 胜来计算胜负，导致 A 和 B 的胜负更加悬殊了（89%比 11%），这是因为 3 局 2 胜的规则将每一局胜负偶然性的影响削弱了。

完整程序

通过以上设计过程，我们的模拟乒乓球比赛的程序越来越完善了。接下去我们可以进一 步改善程序的功能，例如将球员的技术水平改成由用户输入而不是固定的 0.55，允许采取不 同的比赛规则（21 分或 11 分），增加对比赛结果的分析，等等。这些新增特性就不详细解 释了，请读者自行阅读下面的完整程序代码。

【程序 9.2】pingpong.py

```
from random import random
def getInputs():
    p = input("Player A's winning prob: ")
    n = input("How many matches to simulate? ") 
    return p, n
def simNMatches(n,prob,rule): matchA = matchB = 0
    for i in range(n):
    gameA, gameB = oneMatch(prob,rule) 
    if gameA &gt; gameB:
        matchA = matchA + 1 
    else:
        matchB = matchB + 1 
    return matchA, matchB
def oneMatch(prob,rule):
    gameOver = [(3,0),(0,3),(3,1),(1,3),(3,2),(2,3)]
    gameA = gameB = 0
    while not (gameA,gameB) in gameOver: 
        pointA, pointB = oneGame(prob,rule) 
        if pointA > pointB:
            gameA = gameA + 1 
        else:
            gameB = gameB + 1 
        return gameA, gameB
def oneGame(prob,rule): 
    pointA = pointB = 0
    while not gameOver(pointA,pointB,rule): 
        if random() < prob:
            pointA = pointA + 1 
        else:
            pointB = pointB + 1 
    return pointA, pointB
def gameOver(a,b,rule):
    return (a&gt;=rule or b&gt;=rule) and abs(a-b)&gt;=2
def printSummary(a,b): n = float(a + b)
    print "Wins for A: %d (%0.1f%%)" % (a, a/n*100) 
    print "Wins for B: %d (%0.1f%%)" % (b, b/n*100)
def main():
    p, n = getInputs()
    matchA, matchB = simNMatches(n,p,21)
    print "\nRule: 21 points, best of 3 games." printSummary(matchA,matchB)
    matchA, matchB = simNMatches(n,p,11)
    print "\nRule: 11 points, best of 3 games." printSummary(matchA,matchB)
main() 
```

本程序的核心代码在前面介绍了，其他如 getInputs 和 printSummary 的功能都是显然的， 只有判断一局比赛结束的 gameOver 中有个条件 abs(a-b)>=2 需要说明一下。乒乓球比赛规 则规定，赢得一局比赛的球员至少要比对手多得 2 分，即 20:20（或 10:10）之后，一定要 连得 2 分才能赢得此局。

下面是本程序的一次运行结果：

```
Player A's winning prob: 0.52
How many matches to simulate? 100
Rule: 21 points, best of 3 games. Wins for A: 74 (74.0%)
Wins for B: 26 (26.0%)
Rule: 11 points, best of 3 games. Wins for A: 68 (68.0%)
Wins for B: 32 (32.0%) 
```

结果表明，假设球员 A 对球员 B 的得分概率为 52%，当采用每局 21 分、3 局 2 胜的规 则时，A 有 74%的机会赢得比赛；当采用每局 11 分、3 局 2 胜的规则时，A 的获胜概率降到了 68%。这说明，国际乒联对规则的修改确实能削弱强手的优势程度。不过即便如此， 强手获胜的概率还是相当高，何况中国球员的得分概率恐怕远不止 52%。或许要将每局分 数再减少点，或者用其他方法，才能增加外国选手获胜机会吧:-)。

读者可以试着修改程序 9.2，比如采取发球方得分概率作为技术水平的表示，并且将发 球、换发球等因素添加到模拟程序中。

# 9.2 原型法

## 9.2 原型法

我们在 4.3 中介绍了自顶向下逐步求精的程序设计方法。自顶向下设计是非常强大的程 序设计技术，但它也有不适用的场合。

自顶向下设计的第一步是顶层设计，这需要设计者对问题的全局有清晰的认识。万一要 解决的问题非常复杂，或者用户需求不是很完整、清晰，这时顶层设计就非常困难。另外， 设计者有时候会卡在自顶向下层次中的某一层，这就导致下层的精化无法继续，从而影响整 个程序的开发。即便前面这两个问题都不存在，自顶向下设计也存在开发周期过长、工作量 太大的缺点。

另一种程序设计方法是原型法（prototyping）。这种方法的思想是，先开发一个简单版 本，即功能少、界面简单的版本，然后再对这个简单版本逐步进行改善（添加或修改功能）， 直至完全满足用户需求。初始精简版程序称为原型（prototype）。应用原型法来进行软件开 发的步骤大致如下：

（1）确认基本需求；

（2）创建原型；

（3）向用户演示或交付用户试用，获得反馈意见；

（4）改善原型；回到（3），重复（3）、（4），直至用户最终认可。 可见，原型技术不是对整个问题按照“设计、实现、测试”的过程来开发，而是先按此

过程创建一个原型，然后根据用户反馈再重复此过程来改善原型。这样，通过许多“设计－ 实现－测试”小循环，原型逐步得到改善和扩展，直至成为最终产品。因此原型法也称为“螺 旋式开发”。原型法适合于大型程序开发中需求分析难以一次完成的场合，也常用在程序用 户界面设计领域。

原型法开发有很多优点，例如：用户可以通过实际使用的体验来评价软件产品是否合意， 而不是仅凭开发者口头的描述；开发者和用户可以在开发过程中发现以前没有考虑到的需求 或问题；开发者可以在产品开发的早期就获得用户反馈，避免在开发后期来修改设计，因为 越往后，修改的代价就越大。

原型法开发中的原型可以有两种处理方法。一种方法是，一开始构造的原型就是最终产 品的核心，后续工作都是对原型的改善，通过不断的积累和修改最后得到符合用户需求的产 品。开发过程中，原型尽管功能不完善，但一直是可用的，甚至可以作为在最终产品交付前 的替代产品。另一种方法是，初始创建的原型只是作为与用户进行交互的工具，通过不断展 示给用户看而获得反馈并改进。等到用户认可原型，该原型即被丢弃，开发者将基于用户已 经确认的需求开始正式开发产品。这种做法称为“快速原型法”，因为其主要目的是尽快构 造系统模型，强调的是开发速度。

我们在 9.1.3 中就采用了原型法来设计实现乒乓球比赛的模拟程序。 解决问题的关键是模拟乒乓球比赛，而比赛的最基本动作是打一个回合及记分，因此我们一开始就考虑如何模拟一个回合的比赛并给胜方得分。即：

```
if random() < prob: 
    pointA = pointA + 1
else:
    pointB = pointB + 1 
```

经过测试，可以确认这段代码确实能够模拟具有特定得分概率的球员之间的比赛。在此 基础上，根据实际乒乓球比赛的规则要求，我们将回合扩展到局，又扩展到一场 3 局 2 胜的 比赛。在设计的前期阶段，我们做了很多简化，比如直接假设球员的得分概率为 0.55，直接 规定采用 21 分一局的规则等等。随着螺旋式开发的进展，程序功能越来越完善，所有被简 化的东西最终都会以完善的形式实现。总结这个开发过程，我们的模拟程序大致经历了如下 阶段：

阶段 1：建立原型，能进行回合比赛并为胜方记分；

阶段 2：扩展为能进行一局比赛；

阶段 3：扩展为能进行 3 局 2 胜的比赛；

阶段 4：球员的得分概率、比赛次数改为由用户输入；

阶段 5：添加结果分析，输出分析结果。

通过原型法来编程序，对初学程序设计的人来说是很合适的，因为可以从原型获得最终 程序的感性认识，并进而理解自己到底要写什么样的程序。

要指出的是，螺旋式开发并不是用来取代自顶向下设计的，这两种设计方法是互为补充 的关系。例如，对于大型的复杂程序，如果用原型法，可能原型本身也比较复杂，这时就可 以采用自顶向下设计来创建原型。好的设计者应该根据情况选用多种设计方法，这一切都要 通过实践来学习掌握。

# 9.3 并行计算*

## 9.3 并行计算*

计算思维是建立在计算机的能力和限制之上的，计算机科学家的任务是尽量发扬计算机 的能力，避开计算机的限制。传统的计算概念是在计算机发明之初形成的，就是由一个处理 器按顺序执行一个程序的所有指令。并行计算则突破了这种限制，试图让多个处理器同时做 事情。并行计算的好处是显然的，想想一个人吃一锅饭与一百个人同时吃一锅饭的差别，就 能理解并行计算的威力。

可以在不同层次上讨论并行。最底层是机器指令级并行，但这不是我们要关心的层次， 本节主要讨论在程序层次上的并行。

# 9.3.1 串行、并发与并行

### 9.3.1 串行、并发与并行

计算机执行程序时，如果采用按顺序执行的方式，即仅当一个程序执行完毕，下一个程序才能开始执行，则称为串行（serial）执行。在串行执行方式下，CPU 每次由一个程序独 占使用，只要当前程序还没有结束，下一个程序就不能使用 CPU。这就像排队买东西，营 业员（即 CPU）每次只为一个顾客服务，等前面的顾客走了，后面的顾客才能获得服务。 串行执行方式有一个缺点，即 CPU 的利用率不高。例如，当一个程序正在等待用户输 入，这时 CPU 会在相当长的时间内无事可做；但因为程序还没结束，所以 CPU 又不能去执行别的程序。

为了提高 CPU 的利用率，计算机可以采用这样的执行方式：当程序 P1 因为等待输入或 其他原因而暂时不用 CPU 时，CPU 就去执行另一个程序 P2；当 P1 结束输入时，CPU 再回 去继续执行 P1。

其实这种执行方式并不稀奇，现实中就有很多类似的做法。例如，在邮局的服务窗口前 有多人排队，如果第一个顾客要寄特快专递，需要填写很多信息，这时营业员就处于空闲状 态。假设排在第二的顾客只是想买张邮票，这时如果营业员严格按照排队原则讲究先来后到 的话，那么即使没事做也不能为后面的顾客服务，非得等到第一个顾客的事情处理完毕才接

待第二个顾客。这种方式显然是非常低效的，服务质量很差。相反，如果营业员能够趁第一 个顾客填写信息时抽空为后面的顾客办理业务，就能大大提高服务效率。又如，厨师做菜时， 如果一个菜正在锅里煮着，厨师肯定会去处理第二个菜，而不是非要等到第一个菜做好了才 去做第二个菜。

那么，计算机能不能实现上述执行方式呢？ 计算机程序的执行是由操作系统控制的，而现代操作系统都支持“多道程序”或“多任务”，即允许多个程序同时执行。相信读者都有同时运行多个程序的经验：一边打开浏览器 浏览网页，一边打开 MP3 播放器听音乐，一边还挂着 QQ 聊天程序。注意，这里所谓的“同 时”是在宏观意义上使用的。在微观上，一个 CPU 不可能真正“同时”执行程序，因为 CPU 在任一时刻只能执行一条指令。操作系统其实是在让多个程序分时使用 CPU，即 CPU 一会 儿执行 P1 程序的若干条指令，一会儿又转而执行 P2 程序的若干条指令，然后又回到 P1 继 续执行它的指令。总之，CPU 不停地在多个程序之间切换执行。由于 CPU 的运算速度非常 快，用户感觉不到这种切换过程，因此从宏观上看，就好像多个程序在同时执行一样。这种 多个相互独立的程序交叉执行的方式称为并发（concurrent）执行。并发执行的多个程序的 起止时间是交叉重叠的，而不是串行执行方式下的前后相继。

当然，如果计算机系统中有多个处理器（核心是 CPU），那么就可以做到真正的多个程 序“同时”执行，因为各 CPU 可以在同一时刻执行各自的指令。为了与单一 CPU 上的并发 相区别，我们称这种执行方式为并行（parallel）执行。事实上，当今计算机技术的一个发 展方向就是多处理器并行计算。例如，中国的“天河一号”计算机曾经在全球最快计算机排 名中名列第一，它拥有 6144 个通用处理器和 5120 个加速处理器，其二期产品“天河一号 A”更是拥有 14336 个六核处理器、7168 个加速处理器和 2048 个八核处理器。即使在个人计算机领域，如今也普遍采用多核处理器，例如市场上已经有了 8 核处理器。

# 9.3.2 进程与线程

### 9.3.2 进程与线程

操作系统控制处理器在多个程序之间切换执行的过程称为调度。传统的多任务操作系统是以进程为单位进行调度的。进程（process）是指程序的一次执行所形成的实体，每当程序 开始执行，就会创建一个进程。每个进程由程序代码以及一些状态信息（如进程数据的当前 值和当前执行点等）组成，状态信息也称为进程的上下文。

注意，程序与进程是不同的概念。首先，不同程序在计算机中执行，自然形成不同的进 程。其次，即使是同一个程序，当它在计算机中多次执行时，也会创建多个不同进程，这些 进程虽然具有相同的程序代码，但各有自己的上下文。例如，在 Windows 中我们可以同时 运行多个记事本程序（notepad.exe），以便编辑多个文本文件。这时，在系统中就创建了多 个 notepad 进程①。

通常，操作系统通过划分时间片来调度进程，各进程分时占有处理器来执行指令。从宏 观上看，多个进程是在同时运行。这就是操作系统的多任务机制。如前所述，多进程并发执 行可以提高系统资源的利用率。

但是，多任务、多进程也有缺点。第一，实现多进程并发需要花费不少系统开销。因为 每创建一个进程都要为它分配一些内存，以便存储它的上下文；而操作系统在不同进程间进 行调度时需要保存和恢复进程上下文。第二，进程间通信比较困难。进程和进程是隔离的， 各进程拥有自己的地址空间，一个进程不能访问另一个进程的地址空间，从而在进程之间很 难共享信息。因此，对于两个需要传递信息的进程，相互通信很麻烦。

> ① Windows 用户可以通过任务管理器来查看所有已创建的进程。

为了解决上述两个问题，产生了线程的概念。传统程序是从第一行指令一直执行到最后 一行指令，程序中只有一个控制流。这就像写小说时，沿着唯一的故事线索推进。而所谓线程（thread），是指程序中的一段代码，它构成程序中一个相对独立的执行单位。线程概念 使我们可以从一个程序中分出多个控制流来执行多个任务，所以线程实际上是程序内部的多 任务机制。就好比一部小说在叙述过程中，同时存在着多个故事线索，多头并进。图 9.1 给 出了单线程与多线程的示意图。

![](img/程序设计思想与方法 289700.png)

![](img/程序设计思想与方法 289701.png)

图 9.1 单线程程序与多线程程序

线程的字面意义就是程序（在执行时就是进程）的一个执行“线索”，一个程序中可以

有多个执行线索。《三国演义》里讲到庞统处理公务的故事，他“手中批判，口中发落，耳 内听词”，不到半天就把积攒了一百多天的事情都处理掉了。用本节术语来说，庞统就是采 用了多线程并发执行技术，所以能够高效率地解决问题。

线程与进程既相似，也有明显的区别。系统中的多个进程一般是由执行不同程序而创建 的，而多个线程是同一程序（进程）的多个执行流；多个进程的状态是各自独立的，而多线 程可以共享地址空间（代码和上下文）；调度进程时上下文切换比多线程的上下文切换开销 大；进程间通信比较麻烦，而线程之间通过共享内存很容易通信。

在单个处理器上，可通过分时抢占或者线程自身执行情况来实现多线程的并发执行。前 者由操作系统进行调度，后者由线程自己放弃控制（比如执行到一个休眠指令时）。不同线 程之间切换得非常快，因此在用户看来各线程是在同时执行的。可见多线程与多任务、多进 程机制的实现技术是类似的。

在多处理器或多核处理器系统中，各处理器或内核都可以执行线程，从而多线程可以真 正地并行执行。由于多核处理器现已成为主流，因此了解并利用多线程技术对程序设计来说 变得越来越重要。

# 9.3.3 多线程编程的应用

### 9.3.3 多线程编程的应用

线程原本是操作系统中的概念，是操作系统用于实现系统功能的工具。现在线程已演变成为用户程序可使用的工具，广泛用于应用程序设计。 多线程技术主要用于需要并发执行的场合。例如在很多游戏程序中，都需要维持一个动画场景，而玩家可以通过鼠标或键盘来输入操作指令，控制游戏的进行。假如程序只有一个 控制流，则当程序执行到等待用户输入指令的时候，由于用户输入较慢（相对 CPU 速度来 说），程序无法向前继续执行语句，因而无法更新动画画面，效果上动画就停顿了。如果将 等待用户输入的任务和维持动画的任务用两个线程来独立实现，则可解决这个问题。

上述例子其实具有一般性。如果在一个长时间计算任务（维持动画）期间需要对输入输 出事件（鼠标或键盘指令）做出反应，单线程程序是不行的，因为程序会阻塞在长时间计算 任务上，没有机会来检查输入输出。而如果用一个线程来执行这个长时间计算任务，并让另 一个线程监控输入输出事件，两个线程的并发执行就可以使应用程序在执行计算任务的同时 能对用户输入作出反应。如图 9.2 所示。

![](img/程序设计思想与方法 290867.png)

图 9.2 多线程的应用

此外，在科学计算中，很多数值算法都可以并行化，如矩阵的并行计算、线性方程组的 并行求解、偏微分方程的并行求解和快速傅里叶变换的并行算法等等。这时可以为每个处理 器建立一个线程，从而高效地进行计算。

虽然多线程技术有很多用途，但掌握多线程编程有点难度，即使对职业程序员也是如此。 例如，多线程技术涉及所谓竞态条件（race condition），即因为未曾预料到的、对事件之间 相对时序的严重依赖而导致的异常行为。具体例子如：两个客户同时登录订票网站，都看到 某航班还剩一个座位，于是都下单预定该座位，最终必然会因为谁先来后到而引起纠纷。如 果两人是在售票点排队购票（串行执行）就没有这个问题。因此，多线程程序与串行程序是 不同的，需要分析并协调各线程间的复杂的执行时序，这导致编程和调试都很困难。

多线程程序中，由于各线程是相互独立的，它们的并发执行没有确定次序可言，因此线 程是一种非确定性的计算模型，同一个程序的多次执行未必导致同样的结果。所以，多线程 计算模型不具有串行计算模型的可理解性、可预测性和确定性性质，使用时要非常小心。

# 9.3.4 Python 多线程编程

### 9.3.4 Python 多线程编程

很多编程语言都支持多线程编程，Python 语言亦然。与其他编程语言相比，Python 的 多线程编程是非常简单的。

Python 提供了两个支持线程的模块，一个是较老的 thread 模块，另一个是较新的 threading 模块。其中 threading 采用了面向对象实现，功能更强，建议读者使用。

thread 模块的用法

任何程序一旦开始执行，就构成了一个主线程。在主线程中随时可以创建新的子线程去

执行特定任务，并且主线程和子线程是并发执行的。 每个线程的生命期包括创建、启动、执行和结束四个阶段。 当主线程结束退出时，它所创建的子线程是否继续生存（如果还没有结束的话）依赖于系统平台，有的平台直接结束各子线程，有的平台则允许子线程继续执行。

thread 模块提供了一个函数 start_new_thread 用于创建和启动新线程，其用法如下： start_new_thread(<函数>,<参数>) 本函数的具体功能是：创建一个新线程并立即返回，返回值是所创建的新线程的标识号（如 果需要可以赋值给一个变量）。新线程随之启动，所执行的任务就是<函数>的代码，它应该 在程序中定义。调用<函数>时传递的实参由<参数>指定，<参数>是一个元组，如果<函数> 没有形参，则<参数>为空元组。当<函数>执行完毕返回时，线程即告结束。

下面用一个例子来说明 thread 模块的用法。程序 9.3 采用孙悟空拔毫毛变出小猴子的故事来演示主线程与它所创建的子线程的关系。主线程是孙悟空，子线程是小猴子。

【程序 9.3】eg9_3.py

```
# -*- coding: cp936 -*- import thread
def task(tName,n): 
    for i in range(n):
        print "%s:%d\n" % (tName,i)
        print "%s:任务完成!回真身去喽...\n" % tName 
        thread.interrupt_main()
print "<老孙>:我是孙悟空!"
print "<老孙>:我拔根毫毛变个小猴儿~~~"
thread.start_new_thread(task,("<小猴>",3))
print "<老孙>:我睡会儿,小猴儿干完活再叫醒我~~~\n" 
while True:
    print "<老孙>:Zzzzzzz\n" 
```

程序执行后，孙悟空（主线程）先说了两句话，然后创建小猴子，最后进入一个无穷循 环。小猴子创建后就立即启动，执行函数 task，该函数的任务只是显示简单的信息。task 函 数的最后一行调用 thread 模块中定义的 interrupt_main 函数，该函数的功能是在主线程中引 发 KeyboardInterrupt 异常，从而中断主线程的执行。如果没有这条语句，主线程将一直处于 无穷循环之中而无法结束。下面是程序的执行效果：

```
<老孙>:我是孙悟空!
<老孙>:我拔根毫毛变个小猴儿~~~
<老孙>:我睡会儿,小猴儿干完活再叫醒我~~~
<小猴>:0
<老孙>:Zzzzzzz
<小猴>:1
<老孙>:Zzzzzzz
<小猴>:2
<老孙>:Zzzzzzz
<小猴>:任务完成!回真身去喽...
<老孙>:Zzzzzzz
Traceback (most recent call last): File "eg9_3.py", line 15, in <module>
print "<老孙>:Zzzzzzz\n"
KeyboardInterrupt 
```

从输出结果可见，主线程和子线程确实是在以交叉方式并行执行。 顺便说一下，由于程序 9.3 中使用了汉字，所以要在程序的第一行使用

```
# -*- coding: cp936 -*- 
```

其作用是告诉 Python 解释器代码中使用了 cp936 编码的字符（即汉字）。 下面再看一个例子。程序 9.4 的主线程创建了两个子线程，两个子线程都执行同一个函数 task，但以不同的节奏来显示信息（每次循环中利用 sleep 分别休眠 2 秒和 4 秒）。注意， 与程序 9.3 不同，主线程创建完子线程后就结束了，留下两个子线程继续执行①。

【程序 9.4】eg9_4.py

```
# -*- coding: cp936 -*- import thread
import time
def task(tName,n,delay): 
    for i in range(n):
        time.sleep(delay)
        print "%s:%d\n" % (tName,i)
print "<老孙>:我是孙悟空!"
print "<老孙>:我拔根毫毛变个小猴儿<哼>"
thread.start_new_thread(task,("<哼>",5,2))
print "<老孙>:我再拔根毫毛变个小猴儿<哈>"
thread.start_new_thread(task,("<哈>",5,4))
print "<老孙>:不管你们喽,俺老孙去也~~~\n" 
```

下面是程序 9.4 的一次执行结果：

```
<老孙>:我是孙悟空!
<老孙>:我拔根毫毛变个小猴儿<哼>
<老孙>:我再拔根毫毛变个小猴儿<哈>
<老孙>:不管你们喽,俺老孙去也~~~
>>> <哼>:0
<哈>:0
<哼>:1
<哼>:2
<哈>:1
<哼>:3
<哼>:4
![](img/程序设计思想与方法 293617.png)① 在作者所用的计算机平台（Windows XP + Python 2.7）上，主线程结束并不会导致子线程结束。
<哈>:2
<哈>:3
<哈>:4
KeyboardInterrupt
>>> 
```

注意在“<哼>:0”之前的 Python 解释器提示符“>>>”，它表明主线程已经结束，控制 已返回给 Python 解释器。但后续的输出表明两个子线程还在继续执行。读者可以分析一下 输出结果所显示的<哼>、<哈>交叉执行的情况，并想想为什么是这样的结果。另外要注意， 由于主线程先于两个子线程结束，导致两个子线程执行结束后无法返回，这时可以用组合键 Ctrl-C 来强行中止子线程，屏幕上出现的 KeyboardInterrupt 就是因此而来。

threading 模块的用法

虽然 thread 模块用起来很方便，但它的功能有限，不如较新的 threading 模块。threading 模块采用面向对象方式来实现对线程的支持，其中定义了类 Thread，这个类封装了有关线 程创建、启动等功能。

Thread 类的使用有两种方式。第一种用法是：直接利用 Thread 类来创建线程对象，并 在创建时向 Thread 构造器传递线程将要执行的函数；创建后通过调用线程对象的 start()方法 来启动线程，以执行指定的任务。这是使用 threading 模块来创建线程的最简单方式。具体 语法如下：

```
t = Thread(target=&lt;函数&gt;,args=&lt;参数&gt;) 
t.start() 
```

可见，创建线程对象时，需向 Thread 类的构造器传递两个参数：线程所执行的<函数>以及 该函数所需的<参数>。注意这里采用了关键字参数的传递方式。

下面的程序 9.5 与程序 9.4 的几乎是一样的，只是采用了 Thread 类来创建线程对象。

【程序 9.5】eg9_5.py

```
# -*- coding: cp936 -*-
from threading import Thread from time import sleep
def task(tName,n,delay): 
    for i in range(n):
        sleep(delay)
        print "%s:%d\n" % (tName,i)
print "<老孙>:我是孙悟空!"
print "<老孙>:我拔根毫毛变个小猴儿<哼>"
t1 = Thread(target=task,args=("<哼>",5,2))
print "<老孙>:我再拔根毫毛变个小猴儿<哈>"
t2 = Thread(target=task,args=("<哈>",5,4)) 
t1.start()
t2.start()
print "<老孙>:不管你们喽,俺老孙去也~~~\n" 
```

另一种使用 Thread 类的方法用到了线程对象的这么一个特性：当用 start()方法启动线程 对象时，系统会自动调用 run()方法。因此，只要我们将线程需要执行的任务代码写到 run() 方法中，就能在创建并启动线程对象后自动执行该任务。而将自己的代码写到 run()方法中 可以通过定义子类并重定义 run()的方式来做到。

总之，我们可以通过下列步骤来创建并启动线程，执行指定的任务。

（1）定义 Thread 类的子类。这相当于定制我们自己的线程对象。

（2）重定义 init**()方法，即定制我们自己的构造器来初始化线程对象，例如可以添 加更多的参数。注意，定制构造器时首先应当执行基类的构造器 Thread.**init ()，因为我 们定制的线程是原始线程的特例，首先要符合原始线程的要求。

（3）重定义 run()方法，即指定我们定制的线程将执行的代码。

（4）利用自定义的线程类创建线程实例，并通过调用该实例的 start()方法（间接调用 run()方法）或直接调用 run()方法来启动新线程执行任务。

程序 9.6 是采用上述方法的一个例子。

```
# -*- coding: cp936 -*-
from threading import Thread 
from time import sleep
exitFlag = False
class myThread(Thread):
    def __init__ (self,tName,n,delay): 
        Thread. __init__ (self) 
        self.name = tName 
        self.loopnum = n
        self.delay = delay
    def run(self):
        print self.name + ": 上场...\n" task(self.name,self.loopnum,self.delay)
        print self.name + ": 退场...\n"
    def task(tName,n,delay): for i in range(n):
        if exitFlag:
        print "<哼>已退场,<哈>也提前结束吧~~~\n" return
        sleep(delay)
        print "%s:%d\n" % (tName,i)
print "<老孙>:我是孙悟空!"
print "<老孙>:我拔根毫毛变个小猴儿<哼>"
t1 = myThread("<哼>",5,2)
t1.start()
print "<老孙>:我再拔根毫毛变个小猴儿<哈>\n" 
t2 = myThread("<哈>",10,4)
t2.start()
while t2.isAlive():
    if not t1.isAlive(): 
        exitFlag = True
print "<老孙>:小猴儿们都回了,俺老孙去也~~~" 
```

当线程启动后，就处于“活着（alive）”的状态，直到线程的 run()方法执行结束（不管 是正常结束还是因为发生异常而终止），该线程才结束“活着”状态。线程对象的 is_alive() 方法可用来检查线程是否活着。程序 9.6 中，主线程在创建并启动两个子线程 t1 和 t2 之后， 就一直检测 t2 是否还活着，如果 t2 活着就接着检测 t1 是否活着。当 t2 活着而 t1 已经结束， 则将一个用作退出标志的全局变量 exitFlag 设置为 True（初始值为 False）。而 t2 执行任务循 环时会检测 exitFlag 变量，一旦发现它变成了 True，就知道另一个线程已经结束，于是不再 执行后面的任务，直接结束返回。读者应该注意到这件事的意义，它意味着多个线程之间是 可以进行协作、同步的，而不是各自只管闷着头做自己的事情。

以下是程序 9.6 的一次执行结果：

```
<老孙>:我是孙悟空!
<老孙>:我拔根毫毛变个小猴儿<哼>
<哼>: 上场...
<老孙>:我再拔根毫毛变个小猴儿<哈>
<哈>: 上场...
<哼>:0
<哼>:1
<哈>:0
<哼>:2
<哈>:1
<哼>:3
<哼>:4
<哼>: 退场...
<哈>:2
<哼>已退场,<哈>也提前结束吧~~~
<哈>: 退场...
<老孙>:小猴儿都回了,俺老孙去也~~~
>>> 
```

线程有名字，可通过 getName()和 setName()方法读出或设置线程名。 总是有一个主线程对象，它对应于程序的初始控制流。

并发计算中的同步问题

多个线程之间如果只是彼此独立地执行各自的任务，事情就简单了。但是实际应用中常常需要多个线程之间进行合作，合作的线程往往要存取一些公共数据。由于多个线程的执行 顺序是不可预测的，这就有可能导致公共数据处于不一致的状态。因此，如果允许多个线程 并发读写公共数据，就必须对多线程的交互进行控制，以保护公共数据的正确性。

程序 9.6 演示了两个线程通过一个全局变量进行协同的例子。

又如，Thread 对象具有一个 join()方法，一个线程对象 t1 可以调用另一个线程对象 t2 的 join()方法，这导致 t1 暂停执行，直至 t2 执行结束（或者执行一个指定的时间）。可见， join 方法能实现让一个线程等待另一个线程以便进行某种同步的目的。

threading 模块还实现了更一般的同步机制，在此就不介绍了。

# 9.3.5 小结

### 9.3.5 小结

多线程编程属于比较复杂的程序设计任务，即使对专家也不是容易的事情。这是因为多 线程在执行上具有不确定性，线程一旦启动，他们之间的相互依赖和相互作用的结果就是不 可预测的。《西游记》中的这段描写或许能帮助读者想象多线程并发执行的复杂性：

悟空见他凶猛，即使身外身法，拔一把毫毛，丢在口中嚼碎，望空喷去，叫一声“变”！ 即变做三二百个小猴……把个魔王围绕，抱的抱，扯的扯，钻裆的钻裆，扳脚的扳脚， 踢打挦毛，抠眼睛，捻鼻子，抬鼓弄，直打做一个攒盘。

不过，有很多看似需要并发计算的问题实际上可以用事件驱动（参见第八章）而不是用 线程来编程。事件驱动在多数情况下更容易实现，因为事件驱动只有一个执行流，没有并发 及同步问题。涉及 GUI 的问题就可以用事件驱动来解决，而科学计算的并行化则适合用多 线程来解决。总之，应当只在真的需要并发计算时才使用线程。

# 9.4 练习

## 9.4 练习

1\. 利用 random 或 randrange 函数来实现下列计算任务。

（1）表示打靶所得环数（0～10 环）的整型随机数；

（2）区间[-0.5,0.5]内的浮点型随机数；

（3）表示投掷一个骰子所得结果的随机数；

（4）表示投掷两个骰子所得结果的随机数。

2\. 修改程序 9.2，以 A、B 两球员各自的发球得分概率表示技术水平，并将发球、换发球添 加到程序中。

3\. 设计并实现模拟排球比赛的程序，以研究只有发球方才能得分的老规则与每回合得分的 新规则对胜负是否有影响。如果有，新规则有利于强队还是弱队。

4\. 编程模拟骰子游戏：玩家首先投掷两个骰子，如果掷出的是 2、3 或 12 点，则玩家输；

如果掷出的是 7 或 11 点，则玩家赢；如果是其他点数（设为 p 点），则继续投掷，一直到掷 出 7 点或者再次掷出 p 点为止，掷出 7 点则玩家输，再次掷出 p 点则玩家赢。模拟多局，并 估算玩家的获胜概率。

5\. 蒙特卡洛技术可用于估计 p 的值。假设有一块圆形飞镖板，正好嵌在一个正方形橱柜里。 现在来随机投掷飞镖，命中飞镖板的次数与命中橱柜（即飞镖板没有覆盖的四个角落）的次 数之比，是由飞镖板与橱柜面积决定的。设 n 是总的投掷次数（落在橱柜范围内），h 是命 中飞镖板的次数，则 p≈4h/n。编程模拟飞镖游戏，输入 n，输出 p 的估计值。提示：如果正 方形以原点为中心且大小为 2x2，则可以用 2*random() - 1 来生成落在正方形中的随机点的 x 和 y 坐标。如果 x2+y2≤1，则该点落在飞镖板内部。

6\. 编程模拟一手掷 5 个骰子，估计 5 个骰子点数全部相同的概率。

7\. 模拟一维随机漫步（random walk）：在一条很长的笔直马路上漫步，步行方向由掷硬币决 定。即如果掷出正面，则向前走一步；否则向后走一步。记录走 n 步后离出发点的距离 d。 重复多次试验，看看 d 的平均值等于多少。

8\. 模拟二维随机漫步：在一个平面上，每次随机向前、向后、向左或向右走一步。如果走 n 步，离出发点距离 d 是多少？需要重复多次求得 d 的平均值。

9\. 修改第 8 题，改成每一步允许向任何方向迈步。提示：利用 angle = random() * 2p 随机生 成一个方向角（前进方向与 x 轴的夹角），走到由 x = x + cos(angle)和 y = y + sin(angle)决定 的位置。作图显示漫游轨迹。

10\. 编写一个程序，其中创建 2 个线程，一个线程用来计算 2～10000000 之间的素数个数， 另一个线程用来计算 10000000～20000000 之间的素数个数。哪个范围内的素数多？