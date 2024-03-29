## Python 深入 06 Python 的内存管理

[`www.cnblogs.com/vamei/p/3232088.html`](http://www.cnblogs.com/vamei/p/3232088.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！

语言的内存管理是语言设计的一个重要方面。它是决定语言性能的重要因素。无论是 C 语言的手工管理，还是 Java 的垃圾回收，都成为语言最重要的特征。这里以 Python 语言为例子，说明一门动态类型的、面向对象的语言的内存管理方式。

### 对象的内存使用

赋值语句是语言最常见的功能了。但即使是最简单的赋值语句，也可以很有内涵。Python 的赋值语句就很值得研究。

整数 1 为一个对象。而 a 是一个引用。利用赋值语句，引用 a 指向对象 1。Python 是动态类型的语言(参考[动态类型](http://www.cnblogs.com/vamei/archive/2012/07/10/2582795.html))，对象与引用分离。Python 像使用“筷子”那样，通过引用来接触和翻动真正的食物——对象。

![](img/rdb_epub_1068535101583415583.jpg)

 引用和对象

为了探索对象在内存的存储，我们可以求助于 Python 的内置函数 id()。它用于返回对象的身份(identity)。其实，这里所谓的身份，就是该对象的内存地址。

```py
a = 1

print(id(a))
print(hex(id(a)))

```

在我的计算机上，它们返回的是:

11246696
'0xab9c68'

分别为内存地址的十进制和十六进制表示。

在 Python 中，整数和短小的字符，Python 都会缓存这些对象，以便重复使用。当我们创建多个等于 1 的引用时，实际上是让所有这些引用指向同一个对象。

```py
a = 1 b = 1

print(id(a))
print(id(b))

```

上面程序返回

11246696

11246696

可见 a 和 b 实际上是指向同一个对象的两个引用。

为了检验两个引用指向同一个对象，我们可以用 is 关键字。is 用于判断两个引用所指的对象是否相同。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
# True
a = 1 b = 1
print(a is b)

# True
a = "good" b = "good"
print(a is b)

# False
a = "very good morning" b = "very good morning"
print(a is b)

# False
a = []
b = []
print(a is b)

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

上面的注释为相应的运行结果。可以看到，由于 Python 缓存了整数和短字符串，因此每个对象只存有一份。比如，所有整数 1 的引用都指向同一对象。即使使用赋值语句，也只是创造了新的引用，而不是对象本身。长的字符串和其它对象可以有多个相同的对象，可以使用赋值语句创建出新的对象。

在 Python 中，每个对象都有存有指向该对象的引用总数，即引用计数(reference count)。

我们可以使用 sys 包中的 getrefcount()，来查看某个对象的引用计数。需要注意的是，当使用某个引用作为参数，传递给 getrefcount()时，参数实际上创建了一个临时的引用。因此，getrefcount()所得到的结果，会比期望的多 1。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
from sys import getrefcount

a = [1, 2, 3] 

```

print(getrefcount(a))

b = a
print(getrefcount(b))

![复制代码](img/rdb_epub_4593197691164204437.jpg)

由于上述原因，两个 getrefcount 将返回 2 和 3，而不是期望的 1 和 2。

### 对象引用对象

Python 的一个容器对象(container)，比如表、词典等，可以包含多个对象。实际上，容器对象中包含的并不是元素对象本身，是指向各个元素对象的引用。

我们也可以自定义一个对象，并引用其它对象:

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
class from_obj(object):
    def __init__(self, to_obj):
        self.to_obj = to_obj

b = [1,2,3]
a = from_obj(b)
print(id(a.to_obj))
print(id(b))

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

可以看到，a 引用了对象 b。

对象引用对象，是 Python 最基本的构成方式。即使是 a = 1 这一赋值方式，实际上是让词典的一个键值"a"的元素引用整数对象 1。该词典对象用于记录所有的全局引用。该词典引用了整数对象 1。我们可以通过内置函数 globals()来查看该词典。

当一个对象 A 被另一个对象 B 引用时，A 的引用计数将增加 1。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
from sys import getrefcount()

a = [1, 2, 3]
print(getrefcount(a))

b = [a, a]
print(getrefcount(a))

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

由于对象 b 引用了两次 a，a 的引用计数增加了 2。

容器对象的引用可能构成很复杂的拓扑结构。我们可以用 objgraph 包来绘制其引用关系，比如

```py
x = [1, 2, 3]
y = [x, dict(key1=x)]
z = [y, (x, y)]

import objgraph
objgraph.show_refs([z], filename='ref_topo.png')

```

![](img/rdb_epub_7617004035706336196.jpg)

objgraph 是 Python 的一个第三方包。安装之前需要安装 xdot。

```py
sudo apt-get install xdot
sudo pip install objgraph

```

[objgraph 官网](http://mg.pov.lt/objgraph/)

两个对象可能相互引用，从而构成所谓的引用环(reference cycle)。

```py
a = []
b = [a]
a.append(b)

```

即使是一个对象，只需要自己引用自己，也能构成引用环。

```py
a = []
a.append(a)
print(getrefcount(a))

```

引用环会给垃圾回收机制带来很大的麻烦，我将在后面详细叙述这一点。

### 引用减少

某个对象的引用计数可能减少。比如，可以使用 del 关键字删除某个引用:

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
from sys import getrefcount

a = [1, 2, 3] b = a
print(getrefcount(b))

del a
print(getrefcount(b))

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

del 也可以用于删除容器元素中的元素，比如: 

```py
a = [1,2,3]
del a[0]
print(a)

```

如果某个引用指向对象 A，当这个引用被重新定向到某个其他对象 B 时，对象 A 的引用计数减少:

![复制代码](img/rdb_epub_4593197691164204437.jpg)from sys import getrefcount

a = [1, 2, 3] b = a print(getrefcount(b)) a = 1 print(getrefcount(b))

![复制代码](img/rdb_epub_4593197691164204437.jpg)

### 垃圾回收

吃太多，总会变胖，Python 也是这样。当 Python 中的对象越来越多，它们将占据越来越大的内存。不过你不用太担心 Python 的体形，它会乖巧的在适当的时候“减肥”，启动垃圾回收(garbage collection)，将没用的对象清除。在许多语言中都有垃圾回收机制，比如 Java 和 Ruby。尽管最终目的都是塑造苗条的提醒，但不同语言的减肥方案有很大的差异 (这一点可以对比本文和[Java 内存管理与垃圾回收](http://www.cnblogs.com/vamei/archive/2013/04/28/3048353.html)

![](img/rdb_epub_2657326679553699675.jpg)

从基本原理上，当 Python 的某个对象的引用计数降为 0 时，说明没有任何引用指向该对象，该对象就成为要被回收的垃圾了。比如某个新建对象，它被分配给某个引用，对象的引用计数变为 1。如果引用被删除，对象的引用计数为 2，那么该对象就可以被垃圾回收。比如下面的表:

del a 后，已经没有任何引用指向之前建立的[1, 2, 3]这个表。用户不可能通过任何方式接触或者动用这个对象。这个对象如果继续待在内存里，就成了不健康的脂肪。当垃圾回收启动时，Python 扫描到这个引用计数为 0 的对象，就将它所占据的内存清空。

然而，减肥是个昂贵而费力的事情。垃圾回收时，Python 不能进行其它的任务。频繁的垃圾回收将大大降低 Python 的工作效率。如果内存中的对象不多，就没有必要总启动垃圾回收。所以，Python 只会在特定条件下，自动启动垃圾回收。当 Python 运行时，会记录其中分配对象(object allocation)和取消分配对象(object deallocation)的次数。当两者的差值高于某个阈值时，垃圾回收才会启动。

我们可以通过 gc 模块的 get_threshold()方法，查看该阈值:

```py
import gc
print(gc.get_threshold())

```

返回(700, 10, 10)，后面的两个 10 是与分代回收相关的阈值，后面可以看到。700 即是垃圾回收启动的阈值。可以通过 gc 中的 set_threshold()方法重新设置。

我们也可以手动启动垃圾回收，即使用 gc.collect()。

### 分代回收

Python 同时采用了分代(generation)回收的策略。这一策略的基本假设是，存活时间越久的对象，越不可能在后面的程序中变成垃圾。我们的程序往往会产生大量的对象，许多对象很快产生和消失，但也有一些对象长期被使用。出于信任和效率，对于这样一些“长寿”对象，我们相信它们的用处，所以减少在垃圾回收中扫描它们的频率。

![](img/rdb_epub_1159271873609087755.jpg)

小家伙要多检查

Python 将所有的对象分为 0，1，2 三代。所有的新建对象都是 0 代对象。当某一代对象经历过垃圾回收，依然存活，那么它就被归入下一代对象。垃圾回收启动时，一定会扫描所有的 0 代对象。如果 0 代经过一定次数垃圾回收，那么就启动对 0 代和 1 代的扫描清理。当 1 代也经历了一定次数的垃圾回收后，那么会启动对 0，1，2，即对所有对象进行扫描。

这两个次数即上面 get_threshold()返回的(700, 10, 10)返回的两个 10。也就是说，每 10 次 0 代垃圾回收，会配合 1 次 1 代的垃圾回收；而每 10 次 1 代的垃圾回收，才会有 1 次的 2 代垃圾回收。

同样可以用 get_threshold()来调整，比如对 2 代对象进行更频繁的扫描。

```py
import gc
gc.set_threshold(700, 10, 5)

```

### 孤立的引用环

引用环的存在会给上面的垃圾回收机制带来很大的困难。这些引用环可能构成无法使用，但引用计数不为 0 的一些对象。

```py
a = []
b = [a]
a.append(b)

del a
del b

```

上面我们先创建了两个表对象，并引用对方，构成一个引用环。删除了 a，b 引用之后，这两个对象不可能再从程序中调用，就没有什么用处了。但是由于引用环的存在，这两个对象的引用计数都没有降到 0，不会被垃圾回收。

![](img/rdb_epub_2762801077765553886.jpg)

孤立的引用环

为了回收这样的引用环，Python 复制每个对象的引用计数，可以记为 gc_ref。假设，每个对象 i，该计数为 gc_ref_i。Python 会遍历所有的对象 i。对于每个对象 i 引用的对象 j，将相应的 gc_ref_j 减 1。

![](img/rdb_epub_2605297196352599856.jpg)

遍历后的结果

在结束遍历后，gc_ref 不为 0 的对象，和这些对象引用的对象，以及继续更下游引用的对象，需要被保留。而其它的对象则被垃圾回收。

### 总结

Python 作为一种动态类型的语言，其对象和引用分离。这与曾经的面向过程语言有很大的区别。为了有效的释放内存，Python 内置了垃圾回收的支持。Python 采取了一种相对简单的垃圾回收机制，即引用计数，并因此需要解决孤立引用环的问题。Python 与其它语言既有共通性，又有特别的地方。对该内存管理机制的理解，是提高 Python 性能的重要一步。

欢迎继续阅读“[Python 快速教程](http://www.cnblogs.com/vamei/archive/2012/09/13/2682778.html)”