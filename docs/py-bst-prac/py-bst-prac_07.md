## 一般概念

### 明确的代码

在存在各种黑魔法的 Python 中，我们提倡最明确和直接的编码方式。

**糟糕**

```py
def make_complex(*args):
    x, y = args
    return dict(**locals()) 
```

**优雅**

```py
def make_complex(x, y):
    return {'x': x, 'y': y} 
```

在上述优雅的代码中，x 和 y 以明确的字典形式返回给调用者。开发者在使用 这个函数的时候通过阅读第一和最后一行，能够正确地知道该做什么。而在 糟糕的例子中则没有那么明确。

### 每行一个声明

复合语句（比如说列表推导）因其简洁和表达性受到推崇，但在同一行代码中写 两条独立的语句是糟糕的。

**糟糕**

```py
print 'one'; print 'two'

if x == 1: print 'one'

if <复杂的比较> and <其他复杂的比较>:
    # 做一些工作 
```

**优雅**

```py
print 'one'
print 'two'

if x == 1:
    print 'one'

cond1 = <复杂的比较>
cond2 = <其他复杂的比较>
if cond1 and cond2:
    # 做一些工作 
```

### 函数参数

将参数传递给函数有四种不同的方式：

1.  **位置参数** 是强制的，且没有默认值。 它们是最简单的参数形式，而且能被用在 一些这样的函数参数中：它们是函数意义的完整部分，其顺序是自然的。比如说：对 函数的使用者而言，记住 `send(message, recipient)` 或 `point(x, y)` 需要 两个参数以及它们的参数顺序并不困难。

在这两种情况下，当调用函数的时候可以使用参数名称，也可以改变参数的顺序，比如说 `send(recipient='World', message='Hello')` 和 `point(y=2, x=1)`。但和 `send( 'Hello', 'World')` 和 `point(1, 2)` 比起来，这降低了可读性，而且带来了 不必要的冗长。

2.  **关键字参数** 是非强制的，且有默认值。它们经常被用在传递给函数的可选参数中。 当一个函数有超过两个或三个位置参数时，函数签名会变得难以记忆，使用带有默认参数 的关键字参数将会带来帮助。比如，一个更完整的 `send` 函数可以被定义为 `send(message, to, cc=None, bcc=None)`。这里的 `cc` 和 `bcc` 是可选的， 当没有传递给它们其他值的时候，它们的值就是 None。

Python 中有多种方式调用带关键字参数的函数。比如说，我们可以按定义中的参数顺序而无需 明确的命名参数来调用函数，就像 `send('Hello', 'World', 'Cthulhu', 'God')` 是将密件 发送给上帝。我们也可以使用命名参数而无需遵循参数顺序来调用函数，就像 `send('Hello again', 'World', bcc='God', cc='Cthulhu')` 。如果没有任何强有力的理由 不去遵循最接近函数定义的语法：`send('Hello', 'World', cc='Cthulhu', bcc='God')` 那么 这两种方式都应该是要极力避免的。

作为附注，请遵循 [YAGNI](http://en.wikipedia.org/wiki/You_ain't_gonna_need_it) [http://en.wikipedia.org/wiki/You_ain’t_gonna_need_it] 原则。 通常，移除一个用作“以防万一”但却看起来从未使用的可选参数（以及它在函数中的逻辑），比 添加一个所需的新的可选参数和它的逻辑要来的困难。

3.  **任意参数列表** 是第三种给函数传参的方式。如果函数的目的通过带有数目可扩展的 位置参数的签名能够更好的表达，该函数可以被定义成 `*args` 的结构。在这个函数体中， `args` 是一个元组，它包含所有剩余的位置参数。举个例子， 我们可以用任何容器作为参数去 调用 `send(message, *args)` ，比如 `send('Hello', 'God', 'Mom', 'Cthulhu')`。 在此函数体中， `args` 相当于 `('God','Mom', 'Cthulhu')`。

尽管如此，这种结构有一些缺点，使用时应该予以注意。如果一个函数接受的参数列表具有 相同的性质，通常把它定义成一个参数，这个参数是一个列表或者其他任何序列会更清晰。 在这里，如果 `send` 参数有多个容器（recipients），将之定义成 `send(message, recipients)` 会更明确，调用它时就使用 `send('Hello', ['God', 'Mom', 'Cthulhu'])`。这样的话， 函数的使用者可以事先将容器列表维护成列表（list）形式，这为传递各种不能被转变成 其他序列的序列（包括迭代器）带来了可能。

4.  **任意关键字参数字典** 是最后一种给函数传参的方式。如果函数要求一系列待定的 命名参数，我们可以使用 `**kwargs` 的结构。在函数体中， `kwargs` 是一个 字典，它包含所有传递给函数但没有被其他关键字参数捕捉的命名参数。

和 *任意参数列表*中所需注意的一样，相似的原因是：这些强大的技术是用在被证明确实 需要用到它们的时候，它们不应该被用在能用更简单和更明确的结构，来足够表达函数意图 的情况中。

编写函数的时候采用何种参数形式，是用位置参数，还是可选关键字参数，是否使用形如任意参数 的高级技术，这些都由程序员自己决定。如果能明智地遵循上述建议，就可能且非常享受地写出 这样的 Python 函数：

*   易读（名字和参数无需解释）
*   易改（添加新的关键字参数不会破坏代码的其他部分）

### 避免魔法方法

Python 对骇客来说是一个强有力的工具，它拥有非常丰富的钩子（hook）和工具，允许 你施展几乎任何形式的技巧。比如说，它能够做以下每件事：

*   改变对象创建和实例化的方式
*   改变 Python 解释器导入模块的方式
*   甚至可能（如果需要的话也是被推荐的）在 Python 中嵌入 C 程序

尽管如此，所有的这些选择都有许多缺点。使用更加直接的方式来达成目标通常是更好的 方法。它们最主要的缺点是可读性不高。许多代码分析工具，比如说 pylint 或者 pyflakes，将无法解析这种“魔法”代码。

我们认为 Python 开发者应该知道这些近乎无限的可能性，因为它为我们灌输了没有不可能 完成的任务的信心。然而，知道如何，尤其是何时 **不能** 使用它们是非常重要的。

就像一位功夫大师，一个 Pythonista 知道如何用一个手指杀死对方，但从不会那么去做。

### 我们都是负责任的用户

如前所述，Python 允许很多技巧，其中一些具有潜在的危险。一个好的例子是：任何客户端 代码能够重写一个对象的属性和方法（Python 中没有 “private” 关键字）。这种哲学 是在说：“我们都是负责任的用户”，它和高度防御性的语言（如 Java，拥有很多机制来预防 错误的使用）有着非常大的不同。

这并不意味着，比如说，Python 中没有属性是私有的，也不意味着没有合适的封装方法。 与其依赖在开发者的代码之间树立起的一道道隔墙，Python 社区更愿意依靠一组约定，来 表明这些元素不应该被直接访问。

私有属性的主要约定和实现细节是在所有的“内部”变量前加一个下划线。如果客户端代码 打破了这条规则并访问了带有下划线的变量，那么因内部代码的改变而出现的任何不当的行为或问题，都是客户端代码的责任。

鼓励“慷慨地”使用此约定：任何不开放给客户端代码使用的方法或属性，应该有一个下划线 前缀。这将保证更好的职责划分以及更容易对已有代码进行修改。将一个私有属性公开化 总是可能的，但是把一个公共属性私有化可能是一个更难的选择。

### 返回值

当一个函数变得复杂，在函数体中使用多返回值的语句并不少见。然而，为了保持函数 的明确意图以及一个可持续的可读水平，更建议在函数体中避免使用返回多个有意义的值。

在函数中返回结果主要有两种情况：函数正常运行并返回它的结果，以及错误的情况，要么 因为一个错误的输入参数，要么因为其他导致函数无法完成计算或任务的原因。

如果你在面对第二种情况时不想抛出异常，返回一个值（比如说 None 或 False）来表明 函数无法正确运行，可能是需要的。在这种情况下，越早返回所发现的不正确上下文越好。 这将帮助扁平化函数的结构：在“因为错误而返回”的语句后的所有代码能够假定条件满足 接下来的函数主要结果的运算。有多个这样的返回结果通常是需要的。

尽管如此，当一个函数在其正常过程中有多个主要出口点时，它会变得难以调试和返回其 结果，所以保持单个出口点可能会更好。这也将有助于提取某些代码路径，而且多个出口点 很有可能意味着这里需要重构。

```py
def complex_function(a, b, c):
    if not a:
        return None  # 抛出一个异常可能会更好
    if not b:
        return None  # 抛出一个异常可能会更好

    # 一些复杂的代码试着用 a,b,c 来计算 x
    # 如果成功了，抵制住返回 x 的诱惑
    if not x:
        # 一些关于 x 的计算的 Plan-B
    return x  # 返回值 x 只有一个出口点有利于维护代码 
```

## 习语（Idiom）

编程习语，说得简单些，就是写代码的 *方式*。编程习语的概念在 [c2](http://c2.com/cgi/wiki?ProgrammingIdiom) [http://c2.com/cgi/wiki?ProgrammingIdiom] 和 [Stack Overflow](http://stackoverflow.com/questions/302459/what-is-a-programming-idiom) [http://stackoverflow.com/questions/302459/what-is-a-programming-idiom] 上有充足的讨论。

采用习语的 Python 代码通常被称为 *Pythonic*。

尽管通常有一种 — 而且最好只有一种 — 明显的方式去写得 Pythonic；对 Python 初学者来说，写出习语式的 Python 代码的 *方式* 并不明显。所以，好的习语必须 有意识地获取。

如下有一些常见的 Python 习语：

 ### 解包（Unpacking）

如果你知道一个列表或者元组的长度，你可以将其解包并为它的元素取名。比如， `enumerate()` 会对 list 中的每个项提供包含两个元素的元组：

```py
for index, item in enumerate(some_list):
    # 使用 index 和 item 做一些工作 
```

你也能通过这种方式交换变量：

```py
a, b = b, a 
```

嵌套解包也能工作：

```py
a, (b, c) = 1, (2, 3) 
```

在 Python 3 中，扩展解包的新方法在 [**PEP 3132**](https://www.python.org/dev/peps/pep-3132) [https://www.python.org/dev/peps/pep-3132] 有介绍：

```py
a, *rest = [1, 2, 3]
# a = 1, rest = [2, 3]
a, *middle, c = [1, 2, 3, 4]
# a = 1, middle = [2, 3], c = 4 
``` 

### 创建一个被忽略的变量

如果你需要赋值（比如，在 解包（Unpacking） ）但不需要这个变量，请使用 `__`:

```py
filename = 'foobar.txt'
basename, __, ext = filename.rpartition('.') 
```

### 创建一个含 N 个对象的列表

使用 Python 列表中的 `*` 操作符：

```py
four_nones = [None] * 4 
```

### 创建一个含 N 个列表的列表

因为列表是可变的，所以 `*` 操作符（如上）将会创建一个包含 N 个且指向 *同一个* 列表的列表，这可能不是你想用的。取而代之，请使用列表解析：

```py
four_lists = [[] for __ in xrange(4)] 
```

### 根据列表来创建字符串

创建字符串的一个常见习语是在空的字符串上使用 [`str.join()`](http://docs.python.org/library/stdtypes.html#str.join "(在 Python v2.7)") [http://docs.python.org/library/stdtypes.html#str.join] 。

```py
letters = ['s', 'p', 'a', 'm']
word = ''.join(letters) 
```

这会将 *word* 变量赋值为 ‘spam’。这个习语可以用在列表和元组中。

### 在集合体（collection）中查找一个项

有时我们需要在集合体中查找。让我们看看这两个选择：列表和集合（set）。

用如下代码举个例子：

```py
s = set(['s', 'p', 'a', 'm'])
l = ['s', 'p', 'a', 'm']

def lookup_set(s):
    return 's' in s

def lookup_list(l):
    return 's' in l 
```

即使两个函数看起来完全一样，但因为 *查找集合* 是利用了 Python 中的集合是可哈希的 特性，两者的查询性能是非常不同的。为了判断一个项是否在列表中，Python 将会查看 每个项直到它找到匹配的项。这是耗时的，尤其是对长列表而言。另一方面，在集合中， 想的哈希值将会告诉 Python 在集合的哪里去查找匹配的项。结果是，即使集合很大，查询 的速度也很快。在字典中查询也是同样的原理。想了解更多内容，请见 [StackOverflow](http://stackoverflow.com/questions/513882/python-list-vs-dict-for-look-up-table) [http://stackoverflow.com/questions/513882/python-list-vs-dict-for-look-up-table] 。想了解在每种数据结构上的多种常见操作的花费时间的详细内容， 请见 [此页面](https://wiki.python.org/moin/TimeComplexity?) [https://wiki.python.org/moin/TimeComplexity?]。

因为这些性能上的差异，在下列场合在使用集合或者字典而不是列表，通常会是个好主意：

*   集合体中包含大量的项
*   你将在集合体中重复地查找项
*   你没有重复的项

对于小的集合体，或者你不会频繁查找的集合体，建立哈希带来的额外时间和内存的 开销经常会大过改进搜索速度所节省的时间。

## Python 之禅

又名 [**PEP 20**](https://www.python.org/dev/peps/pep-0020) [https://www.python.org/dev/peps/pep-0020], Python 设计的指导原则。

```py
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!

Python 之禅 by Tim Peters

优美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
当存在多种可能，不要尝试去猜测
而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召） 
```

想要了解一些 Python 优雅风格的例子，请见 [这些来自于 Python 用户的幻灯片](http://artifex.org/~hblanks/talks/2011/pep20_by_example.pdf) [http://artifex.org/~hblanks/talks/2011/pep20_by_example.pdf].

## PEP 8

[**PEP 8**](https://www.python.org/dev/peps/pep-0008) [https://www.python.org/dev/peps/pep-0008] 是 Python 事实上的代码风格指南。

你的 Python 代码遵循 PEP 8 通常是个好主意，当和其他开发者一起维护项目时， 这帮助使代码更加具有可持续性。这个命令行程序，[pep8](https://github.com/jcrocholl/pep8) [https://github.com/jcrocholl/pep8], 能够检查你的代码的一致性。在你的终端中运行下列命令：

```py
$ pip install pep8 
```

然后，对一个文件或者一系列的文件运行它，来获得任何违规行为的报告。

```py
$ pep8 optparse.py
optparse.py:69:11: E401 multiple imports on one line
optparse.py:77:1: E302 expected 2 blank lines, found 1
optparse.py:88:5: E301 expected 1 blank line, found 0
optparse.py:222:34: W602 deprecated form of raising exception
optparse.py:347:31: E211 whitespace before '('
optparse.py:357:17: E201 whitespace after '{'
optparse.py:472:29: E221 multiple spaces before operator
optparse.py:544:21: W601 .has_key() is deprecated, use 'in' 
```

程序 [autopep8](https://pypi.python.org/pypi/autopep8/) [https://pypi.python.org/pypi/autopep8/] 能自动将代码格式化 成 PEP 8 风格。用以下指令安装此程序：

```py
$ pip install autopep8 
```

用以下指令格式化一个文件：

```py
$ autopep8 --in-place optparse.py 
```

不包含 `--in-place` 标志将会使得程序直接将更改的代码输出到控制台，以供审查。 `--aggressive` 标志则会执行更多实质性的变化，而且可以多次使用以达到更佳的效果。

## 约定

这里有一些你应该遵循的约定，以让你的代码更加易读。

### 检查变量是否等于常量

你不需要明确地比较一个值是 True，或者 None，或者 0 - 你可以仅仅把它放在 if 语句中。 参阅 [真值测试](http://docs.python.org/library/stdtypes.html#truth-value-testing) [http://docs.python.org/library/stdtypes.html#truth-value-testing] 来了解什么被认为是 false。

**糟糕**:

```py
if attr == True:
    print 'True!'

if attr == None:
    print 'attr is None!' 
```

**优雅**:

```py
# 检查值
if attr:
    print 'attr is truthy!'

# 或者做相反的检查
if not attr:
    print 'attr is falsey!'

# or, since None is considered false, explicitly check for it
if attr is None:
    print 'attr is None!' 
```

### 访问字典元素

Don’t use the [`dict.has_key()`](http://docs.python.org/library/stdtypes.html#dict.has_key "(在 Python v2.7)") [http://docs.python.org/library/stdtypes.html#dict.has_key] method. Instead, use `x in d` syntax, or pass a default argument to [`dict.get()`](http://docs.python.org/library/stdtypes.html#dict.get "(在 Python v2.7)") [http://docs.python.org/library/stdtypes.html#dict.get]. 不要使用 [`dict.has_key()`](http://docs.python.org/library/stdtypes.html#dict.has_key "(在 Python v2.7)") [http://docs.python.org/library/stdtypes.html#dict.has_key] 方法。取而代之，使用 `x in d` 语法，或者 将一个默认参数传递给 [`dict.get()`](http://docs.python.org/library/stdtypes.html#dict.get "(在 Python v2.7)") [http://docs.python.org/library/stdtypes.html#dict.get]。

**糟糕**:

```py
d = {'hello': 'world'}
if d.has_key('hello'):
    print d['hello']    # 打印 'world'
else:
    print 'default_value' 
```

**优雅**:

```py
d = {'hello': 'world'}

print d.get('hello', 'default_value') # 打印 'world'
print d.get('thingy', 'default_value') # 打印 'default_value'

# Or:
if 'hello' in d:
    print d['hello'] 
```

### 维护列表的捷径

[列表推导](http://docs.python.org/tutorial/datastructures.html#list-comprehensions) [http://docs.python.org/tutorial/datastructures.html#list-comprehensions] 提供了一个强大的而又简洁的方式来处理列表。而且， [`map()`](http://docs.python.org/library/functions.html#map "(在 Python v2.7)") [http://docs.python.org/library/functions.html#map] 和 [`filter()`](http://docs.python.org/library/functions.html#filter "(在 Python v2.7)") [http://docs.python.org/library/functions.html#filter] 函数用用一种不同且更简洁的语法处理列表。

**糟糕**:

```py
# 过滤大于 4 的元素
a = [3, 4, 5]
b = []
for i in a:
    if i > 4:
        b.append(i) 
```

**优雅**:

```py
a = [3, 4, 5]
b = [i for i in a if i > 4]
# Or:
b = filter(lambda x: x > 4, a) 
```

**糟糕**:

```py
# 所有的列表成员都加 3
a = [3, 4, 5]
for i in range(len(a)):
    a[i] += 3 
```

**优雅**:

```py
a = [3, 4, 5]
a = [i + 3 for i in a]
# Or:
a = map(lambda i: i + 3, a) 
```

使用 [`enumerate()`](http://docs.python.org/library/functions.html#enumerate "(在 Python v2.7)") [http://docs.python.org/library/functions.html#enumerate] 获得列表中的当前位置的计数。

```py
a = [3, 4, 5]
for i, item in enumerate(a):
    print i, item
# 打印
# 0 3
# 1 4
# 2 5 
```

使用 [`enumerate()`](http://docs.python.org/library/functions.html#enumerate "(在 Python v2.7)") [http://docs.python.org/library/functions.html#enumerate] 函数比手动维护计数有更好的可读性。而且，它对迭代器 进行了更好的优化。

### 读取文件

使用 `with open` 语法来读取文件。它将会为你自动关闭文件。

**糟糕**:

```py
f = open('file.txt')
a = f.read()
print a
f.close() 
```

**优雅**:

```py
with open('file.txt') as f:
    for line in f:
        print line 
```

`with` 语句会更好，因为它能确保你总是关闭文件，及时是在 `with` 的区块中 抛出一个异常。

### 行的延续

当一个代码逻辑行的长度超过可接受的限度时，你需要将之分为多个物理行。如果行的结尾 是一个反斜杠（），Python 解释器会把这些连续行拼接在一起。这在某些情况下很有帮助， 但我们总是应该避免使用，因为它的脆弱性：如果在行的结尾，在反斜杠后加了空格，这会 破坏代码，而且可能有意想不到的结果。

一个更好的解决方案是在元素周围使用括号。左边以一个未闭合的括号开头，Python 解释器会把行的结尾和下一行连接起来直到遇到闭合的括号。同样的行为适用中括号 和大括号。

**糟糕**:

```py
my_very_big_string = """For a long time I used to go to bed early. Sometimes, \
 when I had put out my candle, my eyes would close so quickly that I had not even \
 time to say “I’m going to sleep.”"""

from some.deep.module.inside.a.module import a_nice_function, another_nice_function, \
    yet_another_nice_function 
```

**优雅**:

```py
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes, "
    "when I had put out my candle, my eyes would close so quickly "
    "that I had not even time to say “I’m going to sleep.”"
)

from some.deep.module.inside.a.module import (
    a_nice_function, another_nice_function, yet_another_nice_function) 
```

尽管如此，通常情况下，必须去分割一个长逻辑行意味着你同时想做太多的事，这 可能影响可读性。 © 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.