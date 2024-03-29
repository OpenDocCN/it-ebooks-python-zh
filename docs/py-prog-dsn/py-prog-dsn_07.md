# 第六章 大量数据的表示和处理

第二章讨论了现实世界信息在计算机中的抽象表示问题，那里主要介绍的是简单数据， 而本章将继续介绍复杂数据的表示和处理。简单数据一般指单个数据，并且没有内部结构， 不可分割。复杂数据正相反，可在两方面呈现复杂性：一是数量多，即待处理的数据是由大 量相互关联的成员数据组成的；二是有内部结构，即数据在内部由若干分量组成，每个分量 本身可能又由更小的分量组成。对于大量数据，可以用集合体数据类型来表示；对于数据的 内部结构，可以利用面向对象中的类来刻划。本章介绍大量数据的表示和处理，第七章介绍 利用类来描述具有深层结构的数据。

# 6.1 概述

## 6.1 概述

实际应用中所处理的数据经常是“大量同类型数据的集合”，例如一次物理实验获得的 大量实验数据、一篇文章中的所有单词、一幅画布上的所有图形等等，这几个例子分别展示 了大量数值的集合、大量字符串的集合和大量对象的集合。为了表示和处理大量数据，编程 语言提供了集合体数据类型，如 Python 中的列表（list）、元组（tuple）、字典（dict）、集合（set）和文件（file）等。 一个问题中的大量数据通常是“相关的”，即数据之间存在特定的逻辑联系。表示相关的大量数据时，必须将它们之间的逻辑联系也表示出来。在程序设计中，为了方便、高效地 处理大量相关数据，常将各数据按照某种合适的存储结构组织在一起，以便反映各数据之间 的逻辑联系。数据结构（data structure）是计算机科学的一个分支，专门研究如何将大量相 关数据按特定的逻辑结构组织起来，以及如何高效地处理这些数据。

下面以字符串数据为例来说明上面这段话的含义。夸张一点说，字符串数据——如 "HELLO"——也是复杂数据，因为它是由一些更简单的数据项（5 个字符"H"、"E"、"L"、 "L"和"O"）组成的。为了在程序中能够方便、高效地处理字符串"HELLO"，我们将这 5 个字符视为以“连续存储的序列结构”组织在一起，因为这种存储结构最恰当地反映了 5 个字符之间的逻辑关系，从而最有利于数据处理的实现。反之，如果将 5 个字符东一个西一个地分散存储，比如用几个独立的变量来分别存储这 5 个字符：

```py
c1 = "H"
c2 = "E"
c3 = "L"
c4 = "L"
c5 = "O" 
```

那么就没有表示出这 5 个字符的逻辑关系（即按特定次序组成的一个单词）。尽管这种存储 方式也实现了数据的存储，但它很难支持方便、高效的数据处理。例如，为了输出"HELLO"， 程序必须分别找到 c1 到 c5 这几个存储位置，并取出其中字符组合成输出，显然非常麻烦。 而在“连续存储的序列结构”方式下，程序只需找到一个存储位置即可。

存储数据的目的是为了将来处理数据，故存储结构一定要适应将来的数据处理。对字符 串数据而言，将相关的一组字符存储为一个连续序列就是合适的，因为这种序列结构非常适 合取字符、取子串、合并等字符串操作。因此，编程语言通常都以“连续存储序列”的逻辑 结构来组织多个字符组成的数据，并将字符串作为基本类型提供给程序员使用。这样，程序 员就不必再去费心考虑该用什么逻辑结构来组织多字符数据了。当然，字符串实在算不得复 杂数据，说它是简单数据也无不可，但以上讨论完全适用于真正复杂的数据。

总之，我们得到的教训是：如果不将组成复杂数据的大量数据项按照合适的逻辑关系组 织起来，那么就无法对复杂数据进行方便、高效的处理。换句话说，合适的数据结构往往是设计有效算法的关键。初学编程者通常以为算法才是程序设计的关键，其实不然。计算机科 学家 N. Wirth①提出过一个公式：算法 + 数据结构 ＝ 程序 这足以说明数据结构的重要性。

由于现实世界中构成复杂数据的逻辑关系多种多样，编程语言不可能对每一种逻辑结构 都像对字符串那样提供现成的数据类型和处理方法。另外，即使是同一个复杂数据，随着处 理需求的不同，也会导致采用不同的逻辑结构。因此，编程语言一般不会提供现成的数据结 构给我们使用，我们需要根据待解决的实际问题，自行设计复杂数据的数据结构。

本章介绍用于表示和处理大量数据的集合体数据类型和几种常用的数据结构。

# 6.2 有序的数据集合体

## 6.2 有序的数据集合体

大量数据按次序排列而形成的集合体称为序列（sequence）。注意，这里所说的“次序”是指各成员数据之间有位置的前后，并非指成员数据按值的大小来排列。就像一群人站成一 排即成序列，并不一定要按高矮顺序排列。

Python 中的字符串、列表和元组数据类型都是序列，第二章中对它们有过初步介绍， 本节将继续介绍对序列的处理。从第五章我们初步了解了对象的概念，以及如何对图形对象 进行操作。这里要说明的是，Python 序列其实都是以面向对象方式实现的，因此对序列的 处理可以通过对序列对象的方法进行调用而实现。

表 6.1 列出了对序列的一些通用的操作（运算符和内建函数），利用这些操作可以实现 对序列的索引、联接、复制、检测成员等。

| 方法 | 含义 |
| --- | --- |
| s1 + s2 | 序列 s1 和 s2 联接成一个序列 |
| s * n 或 n * s | 序列 s 复制 n 次，即 n 个 s 联接 |
| s[i] | 序列 s 中索引为 i 的成员 |
| s[i:j] | 序列 s 中索引从 i 到 j 的子序列 |
| s[i:j:k] | 序列 s 中索引从 i 到 j 间隔为 k 的子序列 |
| len(s) | 序列 s 的长度 |
| min(s) | 序列 s 中的最小数据项 |
| max(s) | 序列 s 中的最大数据项 |
| x in s | 检测 x 是否在序列 s 中，返回 True 或 False |
| x not in s | 检测 x 是否不在序列 s 中，返回 True 或 False |

表 6.1 对序列的基本操作

此外，序列还支持比较运算。序列 s 和 t 的大小按字典序确定：首先通过比较 s[0]与 t[0] 来决定大小，相等时再比较 s[1]和 t[1]，依次类推。这就是说，两个序列相等当且仅当它们 的对应位置上的成员都相等，并且长度相同。

各种序列还有各自特殊的操作，下面分别讨论。

> ① PASCAL 语言的设计者。

# 6.2.1 字符串

### 6.2.1 字符串

关于字符串数据，第二章已经详细介绍过对字符串的基本操作，以及利用字符串库 string 提供的函数来实现更丰富的操作。这里我们再介绍另一种处理方式，即面向对象的方式。 Python 中，每个字符串实际上都是一个对象，因而可以通过向字符串对象发送方法请求的方式来实现对字符串的操作。表 6.2 列出了字符串对象的一些常用方法，并将对应的 string

库函数（参见表 2.5）列在一起以供比较。

| 字符串对象方法 | string 库函数 | 含义 |
| --- | --- | --- |
| s.capitalize() | capitalize(s) | s 首字母大写 |
| s.center(width) | center(s,width) | s 扩展到给定宽度且 s 居中 |
| s.count(sub) | count(s,sub) | sub 在 s 中出现的次数 |
| s.find(sub) | find(s,sub) | sub 在 s 中首次出现的位置 |
| s.ljust(width) | ljust(s,width) | s 扩展到给定宽度且 s 居左 |
| s.lower() | lower(s) | 将 s 的所有字母改成小写 |
| s.lstrip() | lstrip(s) | 将 s 的所有前导空格删去 |
| s.replace(old,new) | replace(s,old,new) | 将 s 中所有 old 替换成 new |
| s.rfind(sub) | rfind(s,sub) | sub 在 s 中最后一次出现的位置 |
| s.rjust(width) | rjust(s,width) | s 扩展到给定宽度且 s 居右 |
| s,rstrip() | rstrip(s) | 将 s 的所有尾部空格删去 |
| s.split() | split(s) | 将 s 拆分成子串的列表 |
| s.upper() | upper(s) | 将 s 的所有字母改成大写 |

表 6.2 字符串对象的方法

不要忘了，字符串数据是不可修改的，因此表 6.2 中没有修改字符串 s 的方法。 下面通过一些例子来演示字符串对象的方法的使用。

```py
>>> s = "I think, therefore I am."
>>> s.count('I')
2
>>> s.find('re')
12
>>> (s.lower()).replace('i','I')
'I thInk, therefore I am.'
>>> s.split()
['I', 'think,', 'therefore', 'I', 'am.']
>>> s.islower()
False 
```

# 6.2.2 列表

### 6.2.2 列表

我们先回顾第二章中介绍的关于列表的知识。 列表是由多个数据组成的序列，可以通过索引（位置序号）来访问列表中的数据。与很

多编程语言提供的数组（array）类型不同，Python 列表具有两个特点：第一，列表成员可 以由任意类型的数据构成，不要求各成员具有相同类型；第二，列表长度是不定的，随时可 以增加和删除成员。另外，与 Python 字符串类型不同，Python 列表是可以修改的，修改方 式包括向列表添加成员、从列表删除成员以及对列表的某个成员进行修改。

作为序列的一种，我们可以对列表施加序列的基本操作，如索引、合并和复制等（参见 表 6.1）。另外由于列表是可以修改的，Python 还为列表提供了修改操作，见表 6.3。

| 修改方式 | 含义 |
| --- | --- |
| a[i] = x | 将列表 a 中索引为 i 的成员改为 x |
| a[i:j] = b | 将列表 a 中索引从 i 到 j（不含）的片段改为列表 b |
| del a[i] | 将列表 a 中索引为 i 的成员删除 |
| del a[i:j] | 将列表 a 中索引从 i 到 j（不含）的片段删除 |

表 6.3 列表的修改

本节要引入的是面向对象方式的列表操作。和字符串一样，Python 列表实际上也是对 象，提供了很多有用的方法。例如，append()方法用于向列表尾部添加成员数据：

```py
>>> a = ['hi']
>>> a.append('there')
>>> a
['hi', 'there'] 
```

利用 append()方法，我们可以将用户输入的一批数据存储到一个列表中：

```py
data = []
x = raw_input('Enter a number: ') 
while x != "":
    data.append(eval(x))
    x = raw_input("Enter a number: ") 
```

这段代码实际上是累积算法，其中的列表 data 就是累积器：首先初始化为空列表，然后通 过循环来逐步累积（添加成员数据）。

表 6.4 列出了列表对象的常用方法。

| 方法 | 含义 |
| --- | --- |
| <列表>.append(x) | 将 x 添加到<列表>的尾部 |
| <列表>.sort() | 对<列表>排序（使用缺省比较函数 cmp） |
| <列表>.sort(mycmp) | 对<列表>排序（使用自定义比较函数 mycmp） |
| <列表>.reverse() | 将<列表>次序颠倒 |
| <列表>.index(x) | 返回 x 在<列表>中第一次出现处的索引 |
| <列表>.insert(i,x) | 在<列表>中索引 i 处插入成员 x |
| <列表>.count(x) | 返回<列表>中 x 的出现次数 |
| <列表>.remove(x) | 删除<列表>中 x 的第一次出现 |
| <列表>.pop() | 删除<列表>中最后一个成员并返回该成员 |
| <列表>.pop(i) | 删除<列表>中第 i 个成员并返回该成员 |

表 6.4 列表对象的方法

下面通过例子来说明对列表对象的处理：

```py
>>> a = ['Irrational',[3.14,2.718],'pi and e']
>>> a.sort()
>>> a
[[3.14, 2.718], 'Irrational', 'pi and e']
>>> a[0].reverse()
>>> a
[[2.718, 3.14], 'Irrational', 'pi and e']
>>> a.insert(2,'number')
>>> a
[[2.718, 3.14], 'Irrational', 'number', 'pi and e']
>>> print a.pop(0)
[2.718, 3.14]
>>> a
['Irrational', 'number', 'pi and e'] 
```

编程案例：一个统计程序 对大量数据进行统计、分析是实际应用中常见的问题，通过计算一些统计指标可以获得有关这批数据的多侧面的特征。常用的统计指标包括总和、算术平均值、中位数、众数、标 准差和方差等，这些指标的计算过程具有不同的特性。

“总和”是可以累积计算的，即可以先计算部分数据的和 sum，当有了新数据再加入 sum 并形成新的 sum。重复上述步骤直至所有数据都已加入 sum，这时所得即总和。利用我 们介绍过的累积算法模式，很容易实现求总和的代码：

```py
sum = 0
data = raw_input("输入新数据: ") 
while data != "":
    x = eval(data) 
    sum = sum + x 
```

从以上代码可以看到，虽然用户输入了很多数据，但程序中却始终只用一个简单变量 data 来存储输入的数据。为什么不怕后面输入的数据将前面输入的数据覆盖掉呢？巧妙之处 在于，累积算法每次接收一个输入数据就立即使用该数据（将新数据加到累加变量 sum 中）， 从而使变量 data 可以用于存储下一个输入数据。我们没有采用“先将所有输入数据存储起 来，然后再求和”的处理策略，因为这个策略需要大量存储空间，更麻烦的是我们预先并不 知道需要多少存储空间。类似地，输入数据的“个数”也可以利用累积算法来求得。

再看“算术平均值”指标，虽然它本身不能直接累积计算，但根据公式“平均值＝总和÷数据个数”可见，可以通过累积算法求得“总和”和“数据个数”，然后直接算出平均值。 推而广之，如果某个统计指标可以表示成某些累积类型指标的代数式，那么这个指标就可以 利用累积算法进行计算，无需保存所有输入数据。

再看一个统计指标——中位数（median）。中位数将全体数据划分为小于和大于它的两 部分，并且两部分的数据个数相等。如果全体数据从小到大有序排列，则处于中间位置的那 个数据就是中位数①。例如，数据集合{3,4,22,50,64}的中位数是 22。中位数的计算与总和、 算术平均值都不同，因为它不能通过累积来计算，如{3,4}的中位数与{3,4,22}的中位数直至{3,4,22,50,64}的中位数基本没什么关系。因此，为了对用户输入的一组数据求中位数，必须 将每个数据先保存起来，等全体数据都到位后才能计算。与中位数类似的、不具有累积计算 性质的统计指标还有众数、标准差等，可以称之为“整体型”指标，即它们都需要针对全体 数据进行计算。那么，如何存储所有输入数据呢？显然，定义许多独立变量来存储输入数据 是不合适的，因为我们不知道用户会输入多少个数据；即使知道用户将输入 n 个数据，定义 n 个独立变量来存储这些数据也是非常笨拙的做法。其实问题很容易解决，列表可以将所有 输入数据组合成单个数据，这样既保存了所有数据，又不需要定义许多独立变量。

下面我们来编写一个统计程序，其功能是获得用户输入的数值数据，并求出这批数据的 总和、算术平均值和中位数。如前所述，这三个指标分别代表三种类型的统计指标，因此我 们的统计程序虽然简单，但具有一般的意义。

按照模块化设计思想，我们分别为数据输入及每个指标的计算设计一个函数。 首先设计获得输入数据的函数。由于整体型指标中位数的计算需要用到全体输入数据，因此我们先将所有输入数据存储到一个列表中。获得用户输入的关键代码是一个哨兵循环， 数据列表是一个累积器，在循环中逐个接收数据。代码如下：

> ① 若数据个数为偶数，则取处于中间位置的两个数据的平均值。

```py
def getInput(): data = []
x = raw_input("Enter a number (&lt;Enter&gt; to quit): ") 
while x != "":
    data.append(eval(x))
    x = raw_input("Enter a number (&lt;Enter&gt; to quit): ") 
return data 
```

接着设计三个统计指标的函数。这些函数的参数都是列表 aList，调用时将存储输入数 据的 data 作为实参传递给 aList 即可。总和及算术平均值很容易计算，只要先对输入列表利 用累积求得总和，然后再除以列表长度即得平均值。列表长度可以用 len()直接求得，不需 要另外写一个累积循环。代码如下：

```py
def sum(aList): 
    s = 0.0
    for x in aList: 
        s = s + x
    return s
def mean(aList):
    return sum(aList) / len(aList) 
```

中位数的计算没有代数公式可用，我们先将全体数据从小到大排序，然后取中间位置的 数据值。当数据个数为奇数时，有唯一的中间位置，故中位数很容易找到；当数据个数为偶 数时，中位数是处于中间位置的两个数据的平均值。列表数据的排序可以利用现成的列表对 象方法 sort()实现，而奇偶性可以利用余数运算的结果来判断。代码如下：

```py
def median(aList): 
    aList.sort()
    size = len(aList) 
    mid = size / 2 
    if size % 2 == 1:
        m = aList[mid] 
    else:
        m = (aList[mid] + aList[mid-1]) / 2.0 
    return m 
```

利用以上四个模块，再加上主控模块 main()，就完成了我们的统计程序。完整代码见程 序 6.1。

【程序 6.1】statistics.py

```py
def getInputs(): d = []
    x = raw_input("Enter a number (&lt;Enter&gt; to quit): ") 
    while x != "":
        d.append(eval(x))
        x = raw_input("Enter a number (&lt;Enter&gt; to quit): ")
    return d
def sum(aList): s = 0.0
    for x in aList: 
        s = s + x
    return s
def mean(aList):
    return sum(aList) / len(aList)
def median(aList): aList.sort()
    size = len(aList) 
    mid = size / 2 
    if size % 2 == 1:
        m = aList[mid] 
    else:
        m = (aList[mid] + aList[mid-1]) / 2.0 
    return m
def main():
    print "This program computes sum, mean and median." 
    data = getInputs()
    sigma = sum(data) 
    xbar = mean(data) 
    med = median(data) 
    print "Sum:", sigma
    print "Average:", xbar 
    print "Median:", med
main() 
```

# 6.2.3 元组

### 6.2.3 元组

第二章中简单介绍了元组数据类型，我们知道元组是用一对圆括号括起、用逗号分隔的多个数据项的集合体。元组也是序列的一种，可以利用表 6.1 中的序列操作对元组进行处理。 元组和列表在很多方面都是相似的，但它们有一个重要的不同点：元组不可修改，即不能对元组施加表 6.3 中的操作。如果序列的内容一经创建就不再改变，那么建议使用元组来 表示这个序列，好处是效率较高，而且可以防止出现误修改操作。

元组的括号有时可以省略，例如用在赋值语句中。我们熟悉的为多个变量同时赋值其实 是元组赋值。下面是一些例子：

```py
>>> 1,2,3
(1, 2, 3)
>>> x = 1,2,3
>>> x
(1, 2, 3)
>>> x,y,z = 1,2,3
>>> x
1
>>> y,z
(2, 3) 
```

元组也可以嵌套，即元组的成员本身可以是元组，例如：

```py
>>> t = ("Lucy",("Math",90))
>>> t[1][1]
90 
```

Python 是以面向对象的方式实现元组类型的，元组对象支持的方法见表 6.5。

| 方法 | 含义 |
| --- | --- |
| <元组>.index(x) | 返回 x 在<元组>中首次出现处的索引 |
| <元组>.count(x) | 返回<元组>中 x 的出现次数 |

表 6.5 元组对象的方法

元组类型的名字 tuple 可以用作构造器，将一个字符串或列表转换成元组对象。例如：

```py
>>> tuple('hello')
('h', 'e', 'l', 'l', 'o')
>>> tuple([1,2,3])
(1, 2, 3)
>>> tuple(['hello','world'])
('hello', 'world') 
```

# 6.3 无序的数据集合体

## 6.3 无序的数据集合体

如 6.2 节所介绍的，Python 中的列表和元组都是有序集合体，成员之间存在某种次序， 因此可以通过各成员所处的位置（索引）来访问成员，就像一群人站成一排然后报数，每人 报出的数字就是他的位置序号。然而，我们在中学数学里所学的集合（set）是若干元素的 无序集合，集合中各元素之间不存在先后关系，就像一群人散乱地站在一起，无法令其报数。 现实中有很多信息可以用这种无序的数据集合体来表示，Python 提供了两种无序集合体类 型：集合和字典。

# 6.3.1 集合

### 6.3.1 集合

Python 提供了集合类型 set，用于表示大量数据的无序集合体。集合可以由各种数据组 成，数据之间没有次序，并且互不相同。可见，Python 集合基本上就是数学中所说的集合①。

集合类型的值有两种创建方式：一种是用一对花括号将多个用逗号分隔的数据括起来； 另一种是调用函数 set()，此函数可以将字符串、列表、元组等类型的数据转换成集合类型的 数据。不管用哪种方式创建集合值，在 Python 内部都是以 set([...])的形式表示的。注意，空 集只能用 set()来创建，而不能用字面值{}表示，因为 Python 将{}用于表示空字典（见 6.3.2 节）。

下面的会话过程演示了集合类型的值的创建。注意，集合中是不能有相同元素的，因此 Python 在创建集合值的时候会自动删除掉重复的数据。

> ① 当然 Python 集合并不完全等同于数学中的集合，例如数学中的集合可能是无穷集。

```py
>>> {1,2,3}
set([1, 2, 3])
>>> s = {1,1,2,2,2,3,3}
>>> s
set([1, 2, 3])
>>> set('set')
set(['s', 'e', 't'])
>>> set('sets')
set(['s', 'e', 't'])
>>> set([1,1,1,2,1])
set([1, 2])
>>> set((1,2,1,1,2,3,4))
set([1, 2, 3, 4])
>>> set()
set([])
>>> type(set())
<type 'set'>
>>> type({})
<type 'dict'> 
```

集合类型支持多种运算，学过中学数学的读者很容易理解这些运算的含义。我们将常用 的集合运算列在表 6.6 中。

| 运算 | 含义 |
| --- | --- |
| x in <集合> | 检测 x 是否属于<集合>，返回 True 或 False |
| s1 &#124; s2 | 并集 |
| s1 & s2 | 交集 |
| s1 – s2 | 差集 |
| s1 ^ s2 | 对称差 |
| s1 <= s2 | 检测 s1 是否 s2 的子集 |
| s1 < s2 | 检测 s1 是否 s2 的真子集 |
| s1 >= s2 | 检测 s1 是否 s2 的超集 |
| s1 > s2 | 检测 s1 是否 s2 的真超集 |
| s1 &#124;= s2 | 将 s2 的元素并入 s1 中 |
| len(s) | s 中的元素个数 |

图 6.6 集合运算

下面是集合运算的例子：

```py
>>> s1 = {1,2,3,4,5}
>>> s2 = {2,4,6,8}
>>> 6 in s1
False
>>> 6 in s2
True
>>> s1 | s2
set([1, 2, 3, 4, 5, 6, 8])
>>> s1 & s2
set([2, 4])
>>> s1 - s2
set([1, 3, 5])
>>> s1 |= s2
>>> s1
set([1, 2, 3, 4, 5, 6, 8])
>>> len(s2)
4 
```

和序列一样，集合与 for 循环语句结合使用，可实现对集合中每个元素的遍历。例如， 接着上面的例子继续执行语句：

```py
>>> for x in s2:
        print x,
8 2 4 6 
```

Python 集合是可修改的数据类型，例如上面例子中修改了集合 s1 的值。但是，Python 集合中的元素必须是不可修改的！因此，集合的元素不能是列表、字典等，只能是数值、字 符串、元组之类。同样，集合的元素不能是集合，因为集合是可修改的。然而，Python 另 外提供了 frozenset()函数，可用来创建不可修改的集合，这种集合可以作为另一个集合的元 素。下面的语句展示了 set 和 frozenset 的区别：

```py
>>> a = set(['hi','there'])
>>> b = set([a,3])
Traceback (most recent call last):
File "&lt;pyshell#74&gt;", line 1, in &lt;module&gt; b = set([a,3])
TypeError: unhashable type: 'set'
>>> a = frozenset(['hi','there'])
>>> b = set([a,3])
>>> b
set([3, frozenset(['there', 'hi'])]) 
```

Python 以面向对象方式实现集合类型，集合对象的方法如表 6.7 所示。

| 方法 | 含义 |
| --- | --- |
| s1.union(s2) | 即 s1 &#124; s2 |
| s1.intersection(s2) | 即 s1 & s2 |
| s1.difference(s2) | 即 s1 – s2 |
| s1.symmetric_difference(s2) | 即 s1 ^ s2 |
| s1.issubset(s2) | 即 s1 <= s2 |
| s1.issuperset(s2) | 即 s1 >= s2 |
| s1.update(s2) | s1 &#124;= s2 |
| s.add(x) | 向 s 中增加元素 x |
| s.remove(x) | 从 s 中删除元素 x（无 x 则出错） |
| s.discard(x) | 从 s 中删除元素 x（无 x 也不出错） |
| s.pop() | 从 s 中删除并返回任一元素 |
| s.clear() | 从 s 中删除所有元素 |
| s.copy() | 复制 s |

表 6.7 集合对象的方法

接着前面的例子，下面通过集合对象方法的调用来处理集合数据：

```py
>>> s2.union([1,2,3])
set([1, 2, 3, 4, 6, 8])
>>> s2.intersection((1,2,3,4))
set([2, 4])
>>> set([2,4]).issubset(s2)
True
>>> s2.issuperset(set([2,4]))
True
>>> s2.add(10)
>>> s2
set([8, 2, 4, 10, 6])
>>> print s2.pop()
8
>>> s2
set([2, 4, 10, 6]) 
```

# 6.3.2 字典

### 6.3.2 字典

在一个数据集合中查找信息有很多种方式，前面介绍的序列采用的是通过位置索引来查 找信息的方式。还有一种常用的查找方式是通过数据间的关联来查找信息，例如手机里的通 信录一般都是通过姓名查找对应的电话号码。Python 中的字典类型可用来实现这种通过数 据查找关联数据的功能。

相信读者都用过字典，知道字典是由大量“词条”组成的，每个词条由“单字（词）” 加“释义”组成。字典的用法正是“根据单字（词）查找释义”。Python 提供的字典类型（dict） 与现实中的字典是类似的：Python 字典是由大量的“键值对（key-value pair）”组成的集合， 每一个键值对形如“key:value”，其用法是通过“键”key 来访问相应的“值”value。

字典类型 dict 与集合类型 set 一样属于无序集合体，即字典中的键值对没有特定的排列 顺序，因此不能像序列那样通过位置索引来查找成员数据。

创建字典

字典的字面值是用一对花括号括起的、以逗号分隔的一些键值对，形如：

```py
{k1:v1,k2:v2,...,kn:vn} 
```

其中，键值对的“键”可以是任何不可修改类型的数据，如数值、字符串和元组等；而键值 对的“值”则可以是任何类型的数据。不含任何键值对的字典是空字典，表示为{}。例如：

```py
>>> d = {'Lucy':1234,'Tom':5678,'Mary':1357}
>>> print d
{'Mary': 1357, 'Lucy': 1234, 'Tom': 5678} 
```

注意，字典中键值对的显示次序与定义次序不同，这是因为字典是无序集合，字典的显示次序由字典在内部的存储结构决定。

除了字面值之外，还可以利用类型构造器 dict()来创建字典，创建时需要将字典的键值 对信息作为参数传递给 dict()。参数的形式有两种：一种是关键字参数形式（参见 4.2.4）， 一种是序列（列表或元组）形式，例如：

```py
>>> d1 = dict(name="Lucy",age=8,hobby=("bk","gm"))
>>> d1
{'hobby': ('bk', 'gm'), 'age': 8, 'name': 'Lucy'}
>>> d2 = dict([[(5,1),'Worker'],[(6,1),'Child'],[(7,1),'CPC']])
>>> d2
{(5, 1): 'Worker', (6, 1): 'Child', (7, 1): 'CPC'} 
```

对字典的操作

字典的主要用途是查找与特定键相关联的值，具体操作形式如下：

```py
<字典>[<键>] 
```

其返回值就是字典中与给定的键相关联的值。如果指定的键在字典中不存在，则报错（KeyError）。例如：

```py
>>> d1["name"]
'Lucy'
>>> d1["age"] 8
>>> d1["hobby"]
('bk', 'gm')
>>> d1["gender"]
Traceback (most recent call last):
File "&lt;pyshell#22&gt;", line 1, in &lt;module&gt; d1["gender"]
KeyError: 'gender' 
```

前面创建的字典 d2 是以元组为键的，访问时当然要提供一个元组，且元组括号可省略。 例如：

```py
>>> d2[(6,1)]
'Child'
>>> d2[7,1]
'CPC' 
```

字典类型的数据是可以修改的。与某个键相关联的值可以通过赋值语句来修改，形如：

```py
<字典>[<键>] = <新值> 
```

如果指定的键不存在，则相当于向字典中添加新的键值对。例如：

```py
>>> d1["age"] = 9
>>> d1
{'hobby': ('bk', 'gm'), 'age': 9, 'name': 'Lucy'}
>>> d1["gender"] = "F"
>>> d1
{'hobby': ('bk', 'gm'), 'age': 9, 'name': 'Lucy', 'gender': 'F'} 
```

事实上，创建字典的常用方式就是从空字典开始，利用循环语句以某种方式逐个获得键 值对数据，并利用赋值语句加入字典。

del 命令可以用来删除字典条目，形如：

```py
del <字典>[<键>] 
```

Python 将字典实现为对象，表 6.8 给出了字典对象的方法。

| 方法 | 含义 |
| --- | --- |
| <字典>.has_key(<键>) | 若<字典>包含<键>，返回 True；否则返回 False |
| <字典>.keys() | 返回所有键构成的列表 |
| <字典>.values() | 返回所有值构成的列表 |
| <字典>.items() | 返回所有(key,value)元组构成的列表 |
| <字典>.clear() | 删除<字典>的所有条目 |

图 6.8 字典对象的方法

下面的会话过程演示了对象方法的用法：

```py
>>> d1.keys()
['hobby', 'age', 'name', 'gender']
>>> d1.values()
[('bk', 'gm'), 9, 'Lucy', 'F']
>>> d1.items()[0:2]
[('hobby', ('bk', 'gm')), ('age', 9)]
>>> d1.has_key("gender")
True 
```

# 6.4 文件

## 6.4 文件

众所周知，CPU 只能读写内存，因此当程序运行时，程序所处理的数据必须存储在内 存中。当程序结束或关机、掉电时，内存中的数据就会消失。为了永久保存数据，必须将数 据存储在磁盘、光盘、闪存盘等不依赖于电源的外部存储器上。另外，与外部存储器相比， 内存的容量小而价格高，不适合海量数据存储。总之，计算机问题求解必须考虑如何处理外 部存储器上的大量数据的问题。前面几节介绍的列表、元组、字典等类型虽然可以用于表示 大量数据，但它们都属于内存数据类型，是对内存数据的组织方式。编程语言另外提供了文 件类型来支持大量数据的存储和处理。

# 6.4.1 文件的基本概念

### 6.4.1 文件的基本概念

外部存储器上的数据是以文件形式进行组织的。一组相关数据存储在一起便构成一个文件（file），每个文件被赋予一个文件名，程序通过文件名来访问文件。文件名通常由主名 和扩展名构成，后者用来描述文件内容，如常见的.txt、.jpg、.doc 等等。当外存上存储了大 量文件时，为便于管理，常将文件分组，构成一个个文件夹（或称目录）；如果每个文件夹 中的文件还是很多，则可以继续分组构成子文件夹（子目录），最终形成一个树形层次式目 录结构。

目录路径

为了指定唯一的文件，必须提供详细的路径。事实上，一个完整的文件标识由磁盘驱动 器、目录层次和文件名三部分构成。各部分之间用特定字符进行分隔，分隔字符在不同操作系统中可能是不同的，例如 Windows 使用“\”，而 Unix、Linux 使用“/”。在 Python 程序 中，路径分隔字符既可以使用“\”，也可以使用“/”。例如，Python 安装目录中有个文件 misc.py，其路径可以用字符串

```py
"C:\Python27\Lib\compiler\misc.py" 
```

来表示。

注意：我们在第二章讨论字符串数据时说过，反斜杠字符“\”在 Python 中可作为转义 符，用于表示特殊字符，如"\n"（换行字符）、"\t"（Tab 字符）和"\xc4"（编码为十六进制 c4 的字符）等。文件路径中如果在反斜杠后出现了 n、t、x 等字符，就可能被解释成特殊字符， 从而导致错误。例如，试图用语句

```py
>>> f = open("C:\Python27\Lib\compiler\transformer.py") 
```

打开文件 transformer.py 时，Python 会将字符串中的\t 解释为 Tab 字符，从而报错。避免这 种错误的简单方法是使用正斜杠字符“/”或者使用两个反斜杠“\”表示单个反斜杠，即形 如

```py
"C:/Python27/Lib/compiler/transformer.py" "C:\\Python27\\Lib\\compiler\\transformer.py" 
```

如果文件和程序在同一个文件夹中，则程序中可以省略文件路径，直接使用文件名来标识文件。

文件格式

文件中存储的数据可以有不同的格式。最简单的文件是文本文件，其中存储的数据是无 格式的字符串，因此对文本文件的处理可以逐字符（字节）地进行。另一种文件格式是二进 制文件，其中存储的数据是二进制串，这种二进制串当然不能按一个字节对应一个字符的方 式来解释，例如存储图像、音频信息的.jpg、.mp3 文件就是常见的二进制文件。至于.doc、.xls 和.ppt 等格式的文件各自具有独特的文件结构，也可以归入二进制文件类别，只能用专门的 程序来处理。

在信息管理应用中，大量信息的组织通常都采用“字段－记录－文件”的层次格式。字 段是最基本的不可分割的数据项，如学号、姓名、年龄等；记录是若干个相关字段结合在一 起形成的数据，例如将某个学生的基本信息组合起来就构成形如（5120309001，张三，18） 的记录；大量同类型的记录即构成了文件，例如全体学生记录存储在磁盘上即构成一个学生 数据文件。所有记录按顺序存储，则文件格式可用图 6.1 表示。

![](img/程序设计思想与方法 188825.png)

图 6.1 字段－记录－文件

本书只讨论文本文件的处理。文本文件中存储的字符主要是可打印字符，包括字母、数字、标点符号和空格等。但有一些控制字符也是常用的，例如“回车”、“换行”等字符，可 用来将文本内容组织成一行一行的形式。由于控制字符不是可打印字符，在程序中只好用转 义符来来间接地表示，例如回车符表示为"\r"，换行符表示为"\n"。

# 6.4.2 文件操作

### 6.4.2 文件操作

常用计算机的人都知道，许多应用软件（如 Word、媒体播放器等）都需要处理文件， 并且都需要经过打开文件、读写文件、关闭文件的步骤，这其实是程序设计中文件处理的一 般过程的反映。

打开文件

在读写文件之前首先需要“打开”文件，这个步骤可以简单地理解为对磁盘文件进行必 要的初始化，至于其底层细节则无需了解。

Python 提供了函数 open 用于文件打开，用法如下：

```py
f = open(<文件名>,<打开方式>) 
```

其含义是按指定的<打开方式>打开由<文件名>标识的磁盘文件，创建一个文件对象作为函 数的返回值，并使变量 f 引用这个文件对象。常用的打开方式包括"r"和"w"，它们分别表示 “读”方式和“写”方式。

顺便强调一下，Python 中的文件处理是面向对象风格的，即文件是一个对象，通过文 件对象的方法来实现文件操作。我们在第五章中初步介绍了对象概念，并且将在第七章详细 讨论面向对象。

为了读取一个文件的内容，需要以读方式打开文件。例如：

```py
f = open("oldfile.dat","r") 
```

成功执行后，就可以通过文件对象 f 来读取文件 oldfile.dat 的内容了。若指定的文件不存在， 则 Python 将报错（IOError）。

为了向一个文件中写入内容，需要以写方式打开文件。例如：

```py
f = open("newfile.txt","w") 
```

成功执行后，就可以通过文件对象 f 来向文件 oldfile.dat 中写入内容了。注意，以写方式打 开文件时，如果指定的文件不存在，则创建该文件；如果指定的文件已经存在，则会清除该 文件原来的内容，即相当于创建新文件。所以，以写方式打开文件时一定要小心，不要把现 有文件破坏了。

读文件

在介绍文件读写之前，先要理解文件“当前读写位置”的概念。读者应该了解老式的录 放机的录放过程吧：录放机有一个磁头，用于读取或录入磁带信息，随着磁带的转动，磁头 也就不断改变着录放位置。Python 中的文件采用类似的顺序读写过程：打开文件后，当前 读写位置就是文件开始处；随着读写命令的执行，当前读写位置不断改变，直至到达文件末 尾。

Python 中的文件对象提供了 read()、readline()和 readlines()方法用于读取文件内容。 read()的用法如下：

```py
<变量> = <文件对象>.read() 
```

含义是读取从当前位置直到文件末尾的内容，并作为字符串返回。如果是刚打开的文件对象， 则返回的字符串包含文件的所有内容。

read()方法也可以带有参数：

```py
<变量> = <文件对象>.read(n) 
```

含义是读取从当前位置开始的 n 个字符，并以此字符串作为返回值。如果指定的 n 大于文件中从当前位置到末尾的字符数，则仅返回这些字符。如果当前位置已到达文件末尾，则 read 返回空串。

假设有一个文件 rhyme.txt，其文本内容是：

```py
Good, better, best, Never let it rest, Till good is better, And better, best. 
```

下面的语句序列对此文件进行读取

```py
>>> f = open("rhyme.txt","r")
>>> s = f.read(8)
>>> s
'Good, be'
>>> f.read(20)
'tter, best,\nNever le'
>>> print f.read()
t it rest,
Till good is better, And better, best.
>>> f.close() 
```

readline()的用法如下：

```py
<变量> = <文件对象>.readline() 
```

含义是读取从当前位置到行末（即下一个换行字符）的所有字符，并以此字符串作为返回值， 赋值给变量。通常用此方法来读取文件的当前行。如果当前处于文件末尾，则 readline 返回 空串。例如：

```py
>>> f = open("rhyme.txt","r")
>>> s = f.readline()
>>> s
'Good, better, best,\n'
>>> f.readline()
'Never let it rest,\n'
>>> print f.readline()
Till good is better,
>>> f.close() 
```

readlines()的用法如下：

```py
<变量> = <文件对象>.readlines() 
```

其含义是读取从当前位置直到文件末尾的所有行，并将这些行构成一个字符串列表作为返回 值，列表中的每个元素都是文件的一行。如果当前处于文件末尾，则 readlines 返回空列表。 例如：

```py
>>> f = open("rhyme.txt","r")
>>> f.readline()
'Good, better, best,\n'
>>> f.readline()
'Never let it rest,\n'
>>> f.readlines()
['Till good is better,\n', 'And better, best.\n']
>>> f.readlines()
[] 
```

写文件

当文件以写方式打开时，可以向文件中写入文本内容。与读文件一样，写入位置也是由 “当前读写位置”决定的。Python 文件对象提供两种写文件的方法：

```py
<文件对象>.write(<字符串>)
<文件对象>.writelines(<字符串列表>) 
```

其中，write 的含义是在文件当前位置处写入字符串，writelines 的含义是在文件当前位置处依次写入列表中的所有字符串。 下面的语句序列创建了一个新文件，并向其中写入了李白的名诗：

```py
>>> f = open("d:/libai.txt","w")
>>> f.write("窗前明月光")
>>> f.write("疑是地上霜\n")
>>> f.write("举头望明月\n 低头思故乡")
>>> f.close() 
```

注意每一次 f.write()都是紧接着上次写入的内容继续的，并不会因为是另一条 f.write()就另 起一行。为了写多行文本，必须人工添加换行字符“\n”。那么，上述语句序列所创建的文 件 libai.txt 有几行文本呢？没错，只有 3 行，因为第一次调用 f.write 时并没有写入换行符， 这导致诗的前两句被写在同一行上了。如图 6.2 所示。

![](img/程序设计思想与方法 191514.png)

图 6.2 写入多行文本

再次强调，写方式打开文件会导致要么创建一个新文件，要么清除一个旧文件，总之文件的内容是全新的。那么有没有办法在现有文件内容基础上再写入一些新内容呢？答案是肯 定的。Python 还提供一种文件打开方式"a"，表示“追加”。以追加方式打开文件后，当前位 置被定位在文件末尾，可以继续写入文本而不改变原有的文件内容。例如：

```py
>>> f = open("d:/libai.txt","a")
>>> f.write("\n---- 李白《静夜思》")
>>> f.close() 
```

结果如图 6.3 所示。

![](img/程序设计思想与方法 191969.png)

图 6.3 向文件追加写入内容

关闭文件

文件处理结束后需要关闭文件，这个步骤大体上涉及释放分配给文件的系统资源，以便 分配给其他文件使用。通过调用文件对象的 close 方法来关闭文件：

```py
<文件对象>.close() 
```

注意，即使程序中没有关闭文件，Python 程序结束时也会自动关闭所有打开的文件。

然而好的做法是由程序自己关闭文件，否则有可能因程序意外终止而导致文件数据丢失。例 如，以写方式打开文件时，如果向文件中写入了文本但还没有关闭文件，那么所写内容是不 会存盘的。这时再以读方式打开同一文件，read()命令返回的是空串。下面的语句序列演示 了这种情况。

```py
>>> f = open("d:/test","w")
>>> f.write("some words")
>>> g = open("d:/test","r")
>>> g.read()
''
>>> f.close()
>>> g.seek(0)
>>> g.read()
'some words' 
```

所以，强烈建议读者在程序中一旦结束对文件的读写，就立即关闭文件。

文件处理程序的常见结构

许多应用程序的算法结构都属于直接了当的 IPO（输入－处理－输出）模式，当输入输 出都是文件时，程序的结构大体如下：

```py
infile = open("input.dat","r") 
outfile = open("output.dat","w") 
while True:
    text = infile.readline() 
    if text == "":
        break
    do something with text ... 
    outfile.write(data)
infile.close() 
outfile.close() 
```

此代码的核心是一个 while 循环，循环的每一步利用 readline()读取输入文件的一行，然后对该行进行处理，并将处理结果写入输出文件。当某次循环读到空行（视为文件尾），则利用 break 跳出循环体，从而结束对文件的处理。

除了“while 循环+readline()”的结构，还可以利用“for 循环+readlines()”的结构。readlines() 一次性读出所有行，形成一个列表，然后针对这个列表进行循环。

```py
for line in infile.readlines():
    do something with line 
    ... 
```

实际上，Python 语言甚至允许直接将打开的文件与 for 循环结合使用，达到和“for 循 环+readlines()”同样的效果。代码如下：

```py
infile = open("input.dat","r") 
for line in infile:
    do something with line 
    ... 
```

这种用法有个好处是无需考虑内存大小，而 readlines()要求内存足够大，以便容纳它返回的 列表。

向文件追加数据

前述读方式打开的文件只能读取不能写入，写方式打开的文件是新建文件（写打开现存文件的话将清除内容），只能写入不能读取。有没有办法保留现存文件的内容并加入新内容 呢？

一种做法是先将文件的现有数据利用 readlines()读出来存入一个列表，然后向该列表添 加数据，最后再把新列表写入文件。这种做法对小文件没有问题，但当文件大小为数百 MB 或若干 GB 时，为了保存所有行的列表需要消耗大量内存。

其实 Python 还提供了一种打开方式"a"，称为“追加”方式，可以用于在现存文件的尾 部追加新数据。当然，如果请求打开的文件不存在，"a"方式就和"w"方式一样，创建一个新 文件。下面的语句演示了追加方式的用法：

```py
>>> f = open("oldfile.txt","a")
>>> f.write("something new\n")
>>> f.close() 
```

# 6.4.3 编程案例：文本文件分析

### 6.4.3 编程案例：文本文件分析

本节讨论一个文件分析程序，其功能是输入一个文本文件，对文件内容进行分词（将字符流划分为单词），然后统计文件中的字符数、单词数、每个单词的出现次数以及行数，最 后输出统计结果。按出现频率前 n 名的单词。这种分析在很多应用中都会用到，例如自然语 言处理、文档相似性比较、搜索引擎等。

分析程序的算法设计是直接了当的，其核心是对多个指标进行累积计数。其中，对字符 数和行数的计数可以利用文件操作的结果直接得到：read()可将整个文件的内容作为一个字 符串返回，字符串长度就是字符总数；readlines()将文件的所有行构成一个列表返回，列表 长度就是行数。至于单词总数，需要先将文件内容（字符串）划分成单词，这可以利用 string 库中的 split 函数实现。既可以对 read()返回的整个字符串分词，也可以通过循环来对 readlines() 返回的每一行字符串分词，我们将采用更简单的前一种方法。下面是实现这一部分工作的示 意代码，其中 f 表示被分析的文件对象：

```py
numchars = len(f.read()) 
numlines = len(f.readlines())
numwords = len(string.split(f.read())) 
```

分析程序中最麻烦的是对每个单词出现次数的累积计数。按照过去介绍的累积算法模式，需要为每一个累积量定义一个累积变量，并在循环中不断更新该变量。然而，这种做法 并不适合现在的场合，因为为文件中可能出现的成千上万个单词各定义一个累积变量显然太 笨拙了，更何况文件中到底有哪些单词是不能预知的。编程解决问题的诀窍之一是使用合适 的数据类型，6.1.2 中介绍的字典正可以在这个场合派上用场。

我们将建立一个字典 worddict，其关键字是文件中出现的单词，值是该单词在文件中出 现的次数，即 worddict[w]等于 w 在文件中出现的次数。在读文件单词的过程中，每当遇到 单词 w，就用下面的语句递增 w 的计数值：

```py
worddict[w] = worddict[w] + 1 
```

不过这里还有一个小麻烦：当首次遇到单词 w 时，字典 worddict 中尚未建立相应的词条， 即 worddict[w]无定义，因此上述递增计数的语句将导致错误（KeyError）。为解决这个小麻 烦，最容易想到的是用条件语句来检测单词 w 是否已经存在于字典中，代码如下：

```py
if worddict.has_key(w): 
    worddict[w] = worddict[w] + 1
else:
    worddict[w] = 1 
```

另一种做法是利用例外处理，通过捕获关键字错误（KeyError）来决定是递增计数还是 首次建立词条。代码如下：

```py
try:
    worddict[w] = worddict[w] + 1 
except KeyError:
    worddict[w] = 1 
```

这个做法在使用字典的程序中很常用，我们的分析程序也采用了这个做法。 除了核心代码，还需补充一些在分词之前对文件字符串进行预处理的代码。其一，将文件内容中的字母都转换成小写，以使单词"WORD"和"word"被识别为同一单词；其二，将文 件内容中的各种标点符号都替换成空格，以使单词"one,two"能被正确地划分为两个单词 "one"和"two"，以及"one, two"不被划分为"one,"和"two"①。做这两件事的代码如下：

```py
text = string.lower(text)
for ch in "`~!@#$%^&*()-_=+[{]}\\|;:'\",&lt;.&gt;/?": 
    text = string.replace(text,ch," ") 
```

接下来即可划分单词，并对所有单词进行循环，在循环过程中构造字典 worddict。代码如下：

```py
wordlist = string.split(text) 
worddict = {}
for w in wordlist: 
    try:
        worddict[w] = worddict[w] + 1 
    except KeyError:
        worddict[w] = 1 
```

最后输出分析结果。由于单词可能很多，我们的分析程序只示意性地输出了 5 个单词及 其出现次数。更好的做法是根据出现次数对单词排名，并输出最频繁的前 n 名单词，有兴趣 的读者可以试着完善这个功能。

将以上讨论综合起来，即得完整的文件分析程序。

> ① 这里的细微差别在于逗号后是否有空格。

【程序 6.2】textanalysis.py

```py
import string
def main():
    fname = raw_input("File to analyze: ") f = open(fname,"r")
    text = f.read() numchars = len(text) f.seek(0)
    numlines = len(f.readlines()) text = string.lower(text)
    for ch in "`~!@#$%^&*()-_=+[{]}\\|;:'\",&lt;.&gt;/?": 
        text = string.replace(text,ch," ")
    wordlist = string.split(text) numwords = len(wordlist) worddict = {}
    for w in wordlist: 
        try:
            worddict[w] = worddict[w] + 1 
        except KeyError:
            worddict[w] = 1
    print "Number of characters:",numchars print "Number of lines:",numlines print "Number of words:",numwords pairlist = worddict.items()
    for i in range(10): 
        print pairlist[i],
main() 
```

注意，由于需要两次读文件（read 和 readlines），所以在第二次读文件之前应将“读写头” 移动到文件开始处，这就是第 8 行的 f.seek(0)所做的事情。

假设有文件 yours.txt，其内容如下：

```py
The life that I have Is all that I have,
And the life that I have Is yours.
The love that I have Of the life that I have
Is yours, and yours, and yours.
A sleep I shall have, A rest I shall have,
Yet death will be but a pause. For the peace of my years
In the long green grass,
Will be yours, and yours, and yours. 
```

则运行程序 6.2 后，将得到如下结果：

```py
File to analyze: yours.txt Number of characters: 315
Number of lines: 14 Number of words: 70
('and', 5) ('all', 1) ('peace', 1) ('love', 1) ('is', 3) 
```

# 6.4.4 缓冲

### 6.4.4 缓冲

当一个人饿了，面对一大碗饭，他该怎么吃呢？任务的目标是将这一碗饭送到肚子里去， 解决饿的问题，而达成目标的最快方法是将一碗饭一口吞下，可惜没人有这么大的嘴。事实 上，人们采取的是每次吃一口的方式，一口一口地将饭吃到肚子里去。这个例子很好地说明 了计算机解决问题时的“缓冲”技术。

利用计算机解决问题时，经常需要将大量数据从一个地方传送到另一个地方，并且一次 性地传送所有数据会遇到种种限制。这时，可以在内存中建立一个缓冲区（buffer），用做传 送数据的临时过渡。通过缓冲区，就可以将大量数据以一小批一小批的方式传送到目的地。

例如，处理一个很大的磁盘文件时，由于内存容量有限，无法一次性将文件内容全部读 入内存，只好在内存中建立一个缓冲区，每次将一小批数据读入缓冲区以供 CPU 处理。上 面说的吃饭例子中，我们的嘴就是缓冲区。生活中类似的例子很多，例如学生用的书包其实 也是一个缓冲区——学生不可能随身带着自己的所有书籍，于是采用书包作为临时存储区， 每天只需带当天要用的课本。

又如，当计算机向打印机传送数据进行打印时，由于发送方（计算机）和接收方（打印 机）的数据处理速率存在很大差异，不可能将数据一下子传给打印机，这时也可以使用缓冲 区来协调计算机和打印机的步调。这种情形在生活中也很常见，去银行办理业务时，由于顾 客来的频繁，而银行职员处理业务较慢，不可能实现“随到随处理”，因此银行设置了等待 区，用于缓冲顾客流。

下面我们编写一个文件拷贝程序，功能是将用户指定的文件复制到文件夹 d:\backup 中。 假设内存容量有限或者 CPU 处理能力有限，导致每次只能处理 1024 个字符。为此，我们使 用 read(n)来读文件，其中参数 n 表示从文件读取 n 个字符。程序代码如下：

【程序 6.3】buffer.py

```py
def main():
    fname = raw_input("Enter file name: ") 
    f = open(fname,"r")
    fcopy = open("d:/backup/"+fname,"w") 
    while True:
        buffer = f.read(1024) 
        if buffer == "":
            break 
        fcopy.write(buffer)
    f.close() 
    fcopy.close() 
```

显然这里的字符串变量 buffer 相当于缓冲区，通过每次读入 1024 个字符，像蚂蚁搬家一样将整个文件复制到目的文件夹。

# 6.4.5 二进制文件与随机存取*

### 6.4.5 二进制文件与随机存取*

前面介绍的文件处理是针对文本文件的，并且主要是顺序存取文件。本节简单介绍二进 制文件的处理以及文件的随机存取。

二进制文件

任何文件在底层都是字节序列。文本文件的字节可解释成字符的编码：如果是 ASCII 编码，则每个字节表示一个字符；如果是 GBK 编码，则每两个字节表示一个汉字。对文本 文件的处理完全基于这种字符解释。而二进制文件的字节序列表示任意的二进制数据，不能 解释为字符序列。对二进制文件的处理也必须基于特定的解释来进行。

Python 语言支持对二进制文件的处理，处理过程仍然是“打开－读写－关闭”三部曲。 打开二进制文件时必须指明“以二进制方式打开”，具体就是用"rb"、"wb"和"ab"分别表

示读打开、写打开和追加打开。例如：

```py
>>> bf1 = open("c:/windows/notepad.exe","rb")
>>> bf1.read(10)
'MZ\x90\x00\x03\x00\x00\x00\x04\x00'
>>> bf2 = open("c:/windows/explorer.exe","rb")
>>> bf2.read(10)
'MZ\x90\x00\x03\x00\x00\x00\x04\x00' 
```

这里我们分别打开了两个常用的 Windows 应用程序文件：记事本和资源管理器，并且各读 了头 10 个字节的内容。从输出结果可见，这些字节一般不能解释成字符①。细心的读者还可 以发现，notepad.exe 和 explorer.exe 这两个文件的头 10 个字符是一样的。这一点都不奇怪， 因为它们都是 exe 文件，而 exe 文件是有规定的文件头格式的。作为练习，读者不妨以二进 制方式打开几个.jpg 文件，并读取文件头若干字节的数据，看看有什么发现。

当然我们还可以将二进制文件以"wb"和"ab"方式打开，从而可以修改二进制文件。不过 除非你知道自己在做什么，一般不要尝试修改二进制文件，因为可能破坏文件格式。

关闭二进制文件和关闭文本文件是一样的，调用文件对象的 close 方法即可。

文件的随机存取

文件一般都是顺序读写的，即从文件开始处按顺序读写文件内容直至文件尾。然而，有时候也需要对文件进行随机读写，即直接定位到文件的特定位置进行读写，不需要读写从文 件头到目标位置之间的内容。以读书作类比，顺序读写就像从第一页逐词逐行读到最后一页 一样，而随机读写则像跳跃式读书，略过中间所有内容直接翻到某一页。

我们说过，读写文件时可以想象有一个“读写头”，就像磁带录音机的磁头一样，当前 读写头所在位置决定了读写的内容是什么。刚打开文件时，读写头位于文件开始处；随着读 写语句的执行，读写头不断移动。顺序读写就像磁带录放机在进行正常的回放或录音，而随 机读写就像快进和快倒。

Python 文件对象提供的 seek()方法可用于文件的随机存取，其用法形如

```py
<文件对象>.seek(n)
<文件对象>.seek(n,m) 
```

其中，seek(n)的含义是将文件当前位置移到偏移为 n 的地方，这里的偏移是相对于文件开始位置的，即文件的第 1 个字节偏移为 0，第 2 个字节偏移为 1，依此类推。seek(n,m)的含义 是将文件当前位置移到偏移为 n 的地方，这里的偏移要依 m 值来定：m 为 0 时相对于文件 开始位置（即与 seek(n)相同），m 为 1 时相对于文件当前位置，m 为 2 时相对于文件末尾。 偏移为正数表示朝文件尾方向移动，偏移为负数表示向文件头方向移动。

> ① 二进制文件中也可以含有字符数据，例如 exe 文件的头两个字节是字母 MZ，这是 exe 文件的标志。

下面的语句序列首先创建一个汉字文本文件 ccfile.txt，其中每个汉字（包括标点符号） 占用 2 字节。其次，以读方式打开 ccfile.txt，然后文件当前位置移到偏移 12 处（即略过前 5 个汉字和 1 个逗号）并读取 4 个字节（即“处处”）；然后倒退 16 个字节并读取 2 个字节（即“春”）；最后向前移动 26 个字节并读 2 个字节（即“风”），最后显示三次读的内容所联接 而成的字符串“处处春风”。

```py
>>> f = open("ccfile.txt","w")
>>> f.write("春眠不觉晓，处处闻啼鸟。夜来风雨声，花落知多少。")
>>> f.close()
>>> f = open("ccfile.txt","r")
>>> f.seek(12)
>>> s = f.read(4)
>>> f.seek(-16,1)
>>> s = s + f.read(2)
>>> f.seek(26,1)
>>> s = s + f.read(2)
>>> print s
处处春风
>>> f.tell()
30L 
```

顺便说一下，文件对象还提供 tell()方法，用于确定当前读写位置。具体用法见上面演 示的最后两行，显然读完“风”后，读写头即停留在 30 号字节处。

# 6.5 几种高级数据结构*

## 6.5 几种高级数据结构*

以上介绍的各种数据集合体都是 Python 直接提供的数据类型，属于基本的数据结构。 本节介绍几种高级数据结构，编程语言不直接支持它们的表示和操作，需要程序员自己实现。

# 6.5.1 链表

### 6.5.1 链表

如前所述，列表是由许多数据按次序排列形成的一种数据结构，列表成员之间的逻辑关 系是由他们的排列次序表示的。例如，如果一群人按姓氏笔画坐在一排相邻的椅子上，那么 这些人的排列次序就表示了他们姓氏笔画的关系，排在 1 号座位的人肯定是笔画最少的，排 在 i 号座位上的人肯定比排在 i+1 号座位上的人笔画要少（参见图 6.4）。

![](img/程序设计思想与方法 199980.png)

图 6.4 用排列次序表示数据间逻辑关系

这种连续排列的数据结构的优点是：仅凭排列次序（或相邻关系）就知道成员数据之间的逻辑关系，而不需要另外存储表示成员间逻辑关系的信息；可以通过位置信息（索引）对 任何成员进行随机访问，而不需要从头开始一个一个查看。但连续存储结构有也有缺点：如 果需要增加新成员，必须移动大量数据以便为新成员腾出空间；如果要删除某个数据，删除 后必须移动大量数据以便填补空缺、保持连续性。仍以图 6.4 所示场景为例，如果新来了一 个姓“冯”的人要加入队列，按数据逻辑关系他应当坐在“王”“孙”之间，因此必须使“王” 以后的所有人向右移动一个座位；如果“郑”离开了，那么“周”和其后的所有人必须左移 一个座位。可见，插入、删除操作的代价很大。

再看另一种场景：仍然是一群人要按姓氏笔画顺序排列，但这些人是东一个西一个随便 站立着的，因此无法仅凭这些人所处的位置来判断谁笔画多谁笔画少。这种情形下还有没有 办法表示他们的姓氏笔画顺序信息呢？当然有，例如我们可以让每个人用手指着应该排在他 后面的那个人（图 6.5）。这样，虽然这群人站得杂乱无章，但是通过他们的手指，事实上形 成了一个有序的排列。注意，最后一个人没有可指的对象，我们不妨让他以手指地，表示这 是排列的末尾。

![](img/程序设计思想与方法 200697.png)

图 6.5 用链接表示数据间逻辑关系

图 6.5 形象地表示了一种以链接方式组织的列表，这种数据结构称为链表（linked list）。 在链表中，成员之间的逻辑关系不是通过存储位置的相邻来表示，而是通过专门的链接信息 来表示。我们将链表中的成员称为结点，每个结点都由两部分信息组成：结点的数据和结点 的链接。结点的数据是实际应用要处理的数据，而结点的链接是对另一个结点的引用（或称 指针），用于表示数据间的逻辑关系。链表中最后一个成员的链接必须设置为表示“无所指” 的某个特殊值。链表结构的第一个结点是整个链表的入口，通常用一个专门的变量来记录链 表入口。链表的形状如图 6.6 所示。

![](img/程序设计思想与方法 200997.png)

图 6.6 链表

链表可以很好地解决连续存储列表的缺点。例如，如果图 6.5 中新来了“冯”，那我们 只需让“王”的手指改为指向“冯”，并让“冯”指向“孙”；如果“郑”要离开，我们只需 让“吴”的手指改为指向“周”！

然而，与普通列表相比，链表在访问其成员数据时比较麻烦，因为无法通过位置信息来 随机访问链表成员。例如，我们无法直接读取“链表的第 5 个结点”，为了进行这个操作， 必须从链表的头开始，顺着链接向后逐个检查结点。

编程实例：链表的表示和处理

有的编程语言提供了指针类型（存储单元的物理地址），可以很方便地表示链表结点之间的链接。但链接实际上是逻辑层的概念，不必非得用物理层的指针来实现。下面通过前述 按姓氏笔画排序的例子来说明链表的表示及操作方法，其中链接是以结点在列表中的位置索 引实现的。

我们用包含两个成员的列表[(name,strokes),link]来表示结点，其中第一个成员本身是二 元组，分别存储姓氏 name 和笔画数 strokes，第二个成员是链接 link。所有结点存储在列表 people 中，这里 people 相当于动态分配的存储空间，结点在 people 中的位置索引就是结点 的“存储地址”，结点的 link 值就是另一个结点的位置索引。因此，虽然结点是按随机次序 存储的，但所有结点按其 link 值前后相连就形成了一个链表。图 6.7(a)展示了存储空间中各 结点的物理存储次序和由链接决定的逻辑次序，其中各个结点的值如图 6.7(b)所示。我们另 外用变量 head 指向链表头（此处即索引为 3 的“孙”结点）。

![](img/程序设计思想与方法 201462.png)

(a) (b)

图 6.7 链表的表示

读者应该注意到，people 中存储的第一个结点很特别，这是我们设计的代表链表尾的特 殊结点。链表尾结点包含一个笔画数高达 100 的假想姓氏 X，目的是使将来新结点总能在链 表尾之前找到插入位置，这样可以使程序代码更简明。

现在来看如何在链表中插入新结点。我们首先利用 people.append()方法在存储空间的尾 部建立新结点 N，然后再将 N 插入到链表中。具体插入过程是：从链表头 head 开始，沿着 链接 link 查看链表，将沿途各结点与 N 比较，直至找到第一个笔画数大于 N 的结点 M。然 后使 N 的 link 指向 M，而原先指向 M 的结点改为指向 N（参见图 6.8）。这样的 M 肯定能 找到，因为最坏情况下会找到链表尾，而那里有一个笔画数为 100 的结点①。

![](img/程序设计思想与方法 202069.png)

图 6.8 向链表中插入新结点

如图 6.8 所示，为了在结点 L 和结点 M 之间插入结点 N，需要调整 L 和 N 的 link 值， 为此需要在查找链表的过程中记下连续两个结点 L 和 M 的地址，这正是下列代码中变量 p 和 q 的任务。插入结点的主要代码如下：

```py
p = head 
q = -1
while True:
    if people[p][0][1] <= people[tail][0][1]: 
        q = p
        p = people[p][1] 
    else:
    people[tail][1] = p 
    if q &gt;= 0:
        people[q][1] = tail 
    else:
        head = tail 
    break 
```

![](img/程序设计思想与方法 202286.png)

> ① 据说笔画数最多的汉字是由四个“龍”组成的，共 64 画。

解决了结点插入链表的问题，则链表的创建问题就变得很平凡了。从空链表（实际上有 一个特殊的链表尾结点）开始，每次根据用户输入的姓氏和笔画数建立新结点，并调用结点 插入算法，重复这个过程即可创建整个链表。程序 6.4 实现了这个功能。

【程序 6.4】linkedlist.py

```py
from string import split
def insert(llist,head,tail): 
    p = head
    q = -1
    while True:
        if llist[p][0][1] <= llist[tail][0][1]: 
            q = p
            p = llist[p][1] 
        else:
        llist[tail][1] = p 
        if q >= 0:
            llist[q][1] = tail 
        else:
            head = tail 
        break
    return head
def main():
    people = [[('X',100),-1]]
    head = 0
    s = raw_input("Enter name and strokes: ") 
    while s != "":
        s2 = split(s,',')
        name,strokes = s2[0],eval(s2[1]) 
        people.append([(name,strokes),-1]) 
        tail = len(people)-1
        head = insert(people,head,tail)
        s = raw_input("Enter name and strokes: ")
    print "Physical order:",
    for i in range(1,len(people)): 
        print people[i][0][0],
    print
    print "Logical order:", 
    p = head
    while people[p][1] >= 0: 
        print people[p][0][0], 
        p = people[p][1]
main() 
```

主程序首先创建空链表（实际上包含特殊的链表尾结点），然后由用户按“姓氏，笔画”格 式输入数据，程序在 people 末尾建立对应的新结点（相对于为新结点分配存储空间），接着 调用 insert 函数将新结点插入到链表中。重复输入数据、存储新结点、插入新结点的过程直 至输入为空，最后分别按 people 中的结点次序（物理存储次序）和链接的次序（逻辑次序） 显示所有结点的姓氏。下面是程序的一次执行过程和结果：

```py
Enter name and strokes: 赵,9 
Enter name and strokes: 钱,10 
Enter name and strokes: 孙,6 
Enter name and strokes: 李,7 
Enter name and strokes: 周,8 
Enter name and strokes: 吴,7 
Enter name and strokes: 郑,8 
Enter name and strokes: 王,4
Enter name and strokes:
Physical order: 赵 钱 孙 李 周 吴 郑 王
Logical order: 王 孙 李 吴 周 郑 赵 钱 
```

图 6.9 是最终结果的示意图。

![](img/程序设计思想与方法 203813.png)

图 6.9 输入 8 个姓氏之后的结果

程序 6.4 只实现了链表的插入功能，作为练习，读者可以尝试为程序增加查找、删除等 功能。

以上介绍的是最简单的单链表。为了更有效地处理链表，还可以设计双链表、循环链表 等结构。事实上，利用链接，还可以设计各种各样的非线性数据结构，如树和图等等。有关 内容可阅读数据结构教材。

# 6.5.2 堆栈

### 6.5.2 堆栈

堆栈（stack）也是一种数据集合体，其中的数据构成一种具有“后进先出（LIFO）”性 质的数据结构，即最后加入堆栈的数据总是首先取出。现实中堆栈的例子俯拾皆是，例如碗橱里的一摞碗、纸箱里的一摞书、弹夹中的子弹等等（图 6.10），他们共同的特点是先放进 去的东西垫底，最后放进去的东西在顶上，而取东西的顺序正好相反。

![](img/程序设计思想与方法 204066.png)

图 6.10 现实中的堆栈例子

如果忽略各种具体堆栈中无关紧要的成分，如所堆放的东西（碗、书、子弹）、容器（纸箱、碗橱、弹夹）和放入/取出的具体实现（人工、机械），那么我们可以抽象地定义堆栈。 所谓堆栈，是以如下两个操作进行处理的数据结构：

*   push(x)：在堆栈顶部推入一个新数据 x，x 即成为新的栈顶元素；

*   pop()：从堆栈中取出栈顶元素，显然被取出的元素只能是最后加入堆栈的元素。 为了完善这两个操作，还需提供一些辅助操作，如：

*   isFull()：检查堆栈是否已满。如果堆栈具有固定大小，那么满了之后是无法执行 push()的；

*   isEmpty()：检查堆栈是否为空。如果堆栈是空的，那么 pop()操作将出错。

此前介绍的数据类型大多是具体的，即它们的实现方式是给定的，例如 int 类型是以 4 个字节来表示，字符串类型是特定编码的字节串等等。而现在我们所讨论的堆栈则是抽象数 据类型，因为我们只规定了堆栈的操作方式，并没有规定操作的具体实现方式。

在具体应用中，可以采用多种不同的方式来实现堆栈这个抽象数据类型。例如，可以采 用列表来实现堆栈。令列表 stack 是存放数据的堆栈，按照堆栈的要求，对 stack 只能执行 push 和 pop 操作，不能像列表那样可以随机存取任何一个元素。假设以列表头为栈底，以 列表尾为栈顶，那么向堆栈中放入元素就只能在尾部添加，Python 列表对象提供的 append 方法正好提供堆栈所需的功能，因此可以用 append 来实现 push()，形如：

```py
def push(stack,x): 
    stack.append(x) 
```

另外，Python 列表对象的 pop()方法的功能是取出列表的最后一个元素，恰好符合堆栈的 pop()方法的要求，因此可以这样实现堆栈 pop 操作：

```py
def pop(stack): 
    return stack.pop() 
```

为了防止从空堆栈中取数据的错误，我们定义一个检测堆栈是否为空的函数：

```py
def isEmpty(stack): 
    return (stack == []) 
```

利用上述以列表实现堆栈的技术，程序 6.5 首先通过用户输入数据创建一个堆栈，然后 再逐个取出堆栈成员。输出恰好是输入的逆序，这是由堆栈的 LIFO 性质决定的。可见，利 用堆栈来逆序显示列表数据是非常容易的。

【程序 6.5】stack.py

```py
def push(stack,x): 
    stack.append(x)
def pop(stack): 
    return stack.pop()
def isEmpty(stack): 
    return (stack == [])
def main():
    stack = []
    print "Pushing..."
    x = raw_input("Enter a string: ") 
    while x != "":
        push(stack,x)
        x = raw_input("Enter a string: ") 
    print "Popping..."
    while not isEmpty(stack): 
        print pop(stack),
main() 
```

下面是程序 6.5 的一次执行情况：

```py
Pushing...
Enter a string: 1st 
Enter a string: 2nd 
Enter a string: 3rd 
Enter a string: 4th 
Enter a string: Popping...
4th 3rd 2nd 1st 
```

堆栈在计算机科学中非常有用，一个常见的用例是实现表达式的计算。 读者都熟悉算术表达式的中缀形式，但在用计算机处理表达式时常将表达式写成后缀形式，例如“1 + 2”可写成“1 2 +”、“3 + 4 *5”可写成“3 4 5* +”。后缀形式的表达式可以 利用堆栈来非常方便地求值，算法如下：

1\. 扫描后缀形式的表达式，每次读一个符号（运算数或者运算符）；

2\. 如果读到的是运算数，则 push 到堆栈中；如果读到的是运算符，则从堆栈 pop 两个运算 数，并执行该运算，然后将运算结果 push 入堆栈；

3\. 重复 1、2，直至到达表达式尾。这时堆栈中应该只剩一个运算数，就是表达式的结果值。

图 6.11 显示的是“3 4 5 * +”的计算过程。

![](img/程序设计思想与方法 205989.png)

图 6.11 利用堆栈对后缀表达式求值

以上求值部分非常容易实现，但要想对用户输入的中缀形式的算术表达式进行求值，还 需要先对输入进行语法分析，拆分出运算符和运算数，然后改成后缀形式。这部分编程有点 复杂，所以在此我们就不实现这个程序了。有兴趣的读者可以尝试解决这个问题。

# 6.5.3 队列

### 6.5.3 队列

队列（queue）也是数据集合体，其中的数据成员有序排列。与堆栈的“后进先出”相 反，队列具有“先进先出（FIFO）”的性质，即最先加入队列的数据将最先移出队列。现实 生活中，当很多人等待某项服务时，通常需要排队，这就是队列，排在最前面的人最先获得 服务。参见图 6.12。

![](img/程序设计思想与方法 206135.png)

图 6.12 队列

队列也是一种抽象数据类型，完全由操作定义其特性。与堆栈的 push/pop 类似，队列的两个主要操作是：

*   enqueue：入队，即在队列尾部添加数据；

*   dequeue：出队，即将队列头部的数据移出队列作为返回值。

队列的具体实现有多种方式，例如可以用顺序列表、链表来实现队列。由于队列和堆栈 具有相似性，所以这里不展开介绍了。作为练习，读者可以模仿上一节的例子，用列表来实 现队列。

# 6.6 练习

## 6.6 练习

1\. 分别举例说明现实中的什么信息适合用列表、元组、集合、字典来表示和处理。

2\. 以统计指标的计算为例，说明为什么同样是处理大量数据，有的程序不需要使用数据集 合体来存储大量数据，而有的程序则需要。

3\. 给定两个列表 `s1 = [2005,7,2,8]`和 `s2 = [’L’,’u’,’c’,'y']`，计算以下表达式：

（1）`s1 + s2`

（2）`2 * s1`

（3）`s1[1]`

（4）`s2[1:3]`

（5）`s1[1] + s2[-1]`

4\. 对第 3 题中的列表进行以下操作后，s1 和 s2 的值是什么。各小题之间是独立的。

（1）`s1.remove(2)`

（2）`s1.sort().reverse()`

（3）`s1.append([s2.index(’c’)])`

（4）`s2.pop(s1.pop(2))`

（5）`s2.insert(s1[2],’I’)`

5\. 修改程序 6.1，使程序能计算更多统计指标（如标准差）。

6\. 程序设计：自己编程实现 Python 列表对象的方法。

（1）编写函数 `count(aList, x)`，功能同 `aList.count(x)`；

（2）编写函数 `isin(aList, x)`，功能同 `x in aList`；

（3）编写函数 `index(aList, x)`，功能同 `aList.index(x)`；

（4）编写函数 `reverse(aList)`，功能同 `aList.reverse()`。

7\. 编写函数 `shuffle(aList)`，其功能是像洗牌一样将列表打乱。

8\. 利用筛法找出小于等于 n 的所有质数。基本思想是：首先创建从 2 到 n 的数值列表；然 后将列表的第一个数显示输出（是质数），并从列表中删除该数的所有倍数。重复以上过程 直至列表为空。例如，如果 n 为 10，则初始列表为[2, 3, 4, 5, 6, 7, 8, 9, 10]。输出 2，并删除 2、4、6、8、10。现在列表为[3, 5, 7, 9]。输出 3，删除 3、9。现在列表为[5, 7]。输出 5， 并删除 5。最后对[7]输出 7，删除 7。至此列表为空，程序结束。

9\. 设计一个学生信息管理系统。每个学生包括学号、姓名、年龄等信息，大量学生数据存 储在文件中。程序开始后向用户显示一个操作菜单，包括向数据文件增加数据项、删除数据 项、查找数据项和退出等功能。用户选择菜单项后执行相应功能，执行完毕回到菜单界面。

11\. 利用链表来实现堆栈数据结构。

12\. 分别利用普通列表和链表来实现队列数据结构。