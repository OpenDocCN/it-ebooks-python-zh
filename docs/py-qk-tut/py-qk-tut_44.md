## Python 标准库 13 循环器 (itertools)

[`www.cnblogs.com/vamei/p/3174796.html`](http://www.cnblogs.com/vamei/p/3174796.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！

在[循环对象](http://www.cnblogs.com/vamei/archive/2012/07/09/2582499.html)和[函数对象](http://www.cnblogs.com/vamei/archive/2012/07/10/2582772.html)中，我们了解了循环器(iterator)的功能。循环器是对象的容器，包含有多个对象。通过调用循环器的 next()方法 (__next__()方法，在 Python 3.x 中)，循环器将依次返回一个对象。直到所有的对象遍历穷尽，循环器将举出 StopIteration 错误。

在 for i in iterator 结构中，循环器每次返回的对象将赋予给 i，直到循环结束。使用 iter()内置函数，我们可以将诸如表、字典等容器变为循环器。比如

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
for i in iter([2, 4, 5, 6]):
    print(i)

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

标准库中的 itertools 包提供了更加灵活的生成循环器的工具。这些工具的输入大都是已有的循环器。另一方面，这些工具完全可以自行使用 Python 实现，该包只是提供了一种比较标准、高效的实现方式。这也符合 Python“只有且最好只有解决方案”的理念。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
# import the tools
from itertools import *

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

### 无穷循环器

count(5, 2)     #从 5 开始的整数循环器，每次增加 2，即 5, 7, 9, 11, 13, 15 ...

cycle('abc')    #重复序列的元素，既 a, b, c, a, b, c ...

repeat(1.2)     #重复 1.2，构成无穷循环器，即 1.2, 1.2, 1.2, ...

repeat 也可以有一个次数限制:

repeat(10, 5)   #重复 10，共重复 5 次

### 函数式工具

[函数式编程](http://www.cnblogs.com/vamei/archive/2012/07/10/2582772.html)是将函数本身作为处理对象的编程范式。在 Python 中，函数也是对象，因此可以轻松的进行一些函数式的处理，比如 map(), filter(), reduce()函数。

itertools 包含类似的工具。这些函数接收函数作为参数，并将结果返回为一个循环器。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
from itertools import * rlt = imap(pow, [1, 2, 3], [1, 2, 3])

for num in rlt:
    print(num)

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

上面显示了 imap 函数。该函数与 map()函数功能相似，只不过返回的不是序列，而是一个循环器。包含元素 1, 4, 27，即 1**1, 2**2, 3**3 的结果。函数 pow(内置的乘方函数)作为第一个参数。pow()依次作用于后面两个列表的每个元素，并收集函数结果，组成返回的循环器。

此外，还可以用下面的函数:

starmap(pow, [(1, 1), (2, 2), (3, 3)])

pow 将依次作用于表的每个 tuple。

ifilter 函数与 filter()函数类似，只是返回的是一个循环器。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
ifilter(lambda x: x > 5, [2, 3, 5, 6, 7]

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

将 lambda 函数依次作用于每个元素，如果函数返回 True，则收集原来的元素。6, 7

此外，

ifilterfalse(lambda x: x > 5, [2, 3, 5, 6, 7]) 

与上面类似，但收集返回 False 的元素。2, 3, 5

takewhile(lambda x: x < 5, [1, 3, 6, 7, 1])

当函数返回 True 时，收集元素到循环器。一旦函数返回 False，则停止。1, 3

dropwhile(lambda x: x < 5, [1, 3, 6, 7, 1])

当函数返回 False 时，跳过元素。一旦函数返回 True，则开始收集剩下的所有元素到循环器。6, 7, 1 

### 组合工具 

我们可以通过组合原有循环器，来获得新的循环器。

chain([1, 2, 3], [4, 5, 7])      # 连接两个循环器成为一个。1, 2, 3, 4, 5, 7

product('abc', [1, 2])   # 多个循环器集合的笛卡尔积。相当于嵌套循环 

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
for m, n in product('abc', [1, 2]):
    print m, n

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

permutations('abc', 2)   # 从'abcd'中挑选两个元素，比如 ab, bc, ... 将所有结果排序，返回为新的循环器。

注意，上面的组合分顺序，即 ab, ba 都返回。

combinations('abc', 2)   # 从'abcd'中挑选两个元素，比如 ab, bc, ... 将所有结果排序，返回为新的循环器。 

注意，上面的组合不分顺序，即 ab, ba 的话，只返回一个 ab。

combinations_with_replacement('abc', 2) # 与上面类似，但允许两次选出的元素重复。即多了 aa, bb, cc

### groupby()

将 key 函数作用于原循环器的各个元素。根据 key 函数结果，将拥有相同函数结果的元素分到一个新的循环器。每个新的循环器以函数返回结果为标签。

这就好像一群人的身高作为循环器。我们可以使用这样一个 key 函数: 如果身高大于 180，返回"tall"；如果身高底于 160，返回"short";中间的返回"middle"。最终，所有身高将分为三个循环器，即"tall", "short", "middle"。 

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
def height_class(h):
    if h > 180:
        return "tall"
    elif h < 160:
        return "short"
    else:
        return "middle" friends = [191, 158, 159, 165, 170, 177, 181, 182, 190]

friends = sorted(friends, key = height_class)
for m, n in groupby(friends, key = height_class):
    print(m)
 print(list(n))

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

注意，groupby 的功能类似于 UNIX 中的 uniq 命令。分组之前需要使用 sorted()对原循环器的元素，根据 key 函数进行排序，让同组元素先在位置上靠拢。

### 其它工具

compress('ABCD', [1, 1, 1, 0])  # 根据[1, 1, 1, 0]的真假值情况，选择第一个参数'ABCD'中的元素。A, B, C

islice()                        # 类似于 slice()函数，只是返回的是一个循环器

izip()                          # 类似于[zip()函数](http://www.cnblogs.com/vamei/archive/2012/07/09/2582435.html)，只是返回的是一个循环器。 

### 总结

itertools 的工具都可以自行实现。itertools 只是提供了更加成形的解决方案。

欢迎继续阅读“[Python 快速教程](http://www.cnblogs.com/vamei/archive/2012/09/13/2682778.html)”