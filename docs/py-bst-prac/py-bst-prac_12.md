### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# 常见陷阱

大多数情况下，Python 的目标是成为一门简洁和一致的语言，同时避免意外情况。 然而，有些情况可能会使新人困惑。

其中一些情况是有意为之的，但可能有潜在的风险。而另一些可以说是语言的缺陷。 总的来说，下面是一些乍看起来很取巧的行为，不过只要你注意了强调的事项， 这些行为通常是可取的。

 ## 可变默认参数

看起来，*最* 让 Python 程序员感到惊奇的是 Python 对函数定义中可变默认参数的处理。

### 你所写的

```
def append_to(element, to=[]):
    to.append(element)
    return to 
```

### 你所期望的

```
my_list = append_to(12)
print my_list

my_other_list = append_to(42)
print my_other_list 
```

每次调用函数时，如果不提供第二个参数，就会创建一个新的列表，所以结果应是这样的：

> [12] [42]

### 而事实是

```
[12]
[12, 42] 
```

当函数被定义时，一个新的列表就被创建 *一次* ，而且同一个列表在每次成功的调用中都被使用。

当函数被定义时，Python 的默认参数就被创建 *一次*，而不是每次调用函数的时候创建。 这意味着，如果你使用一个可变默认参数并改变了它，你 *将会* 在未来所有对此函数的 调用中改变这个对象。

### 你应该做的

在每次函数调用中，通过使用指示没有提供参数的默认参数（[`None`](http://docs.python.org/library/constants.html#None "(在 Python v2.7)") [http://docs.python.org/library/constants.html#None] 通常是 个好选择），来创建一个新的对象。

```
def append_to(element, to=None):
    if to is None:
        to = []
    to.append(element)
    return to 
```

### 什么情况下陷阱不是陷阱

有时你可以专门“利用”（或者说特地使用）这种行为来维护函数调用间的状态。这通常用于 编写缓存函数。 

## 迟绑定闭包

另一个常见的困惑是 Python 在闭包(或在周围全局作用域（surrounding global scope）)中 绑定变量的方式。

### 你所写的

```
def create_multipliers():
    return [lambda x : i * x for i in range(5)] 
```

### 你所期望的

```
for multiplier in create_multipliers():
    print multiplier(2) 
```

一个包含五个函数的列表，每个函数有它们自己的封闭变量 `i` 乘以它们的参数，得到:

```
0
2
4
6
8 
```

### 而事实是

```
8
8
8
8
8 
```

五个函数被创建了，它们全都用 4 乘以 `x` 。

Python 的闭包是 *迟绑定* 。 这意味着闭包中用到的变量的值，是在内部函数被调用时查询得到的。

这里，不论 *任何* 返回的函数是如何被调用的， `i` 的值是调用时在周围作用域中查询到的。 接着，循环完成， `i` 的值最终变成了 4。

关于这个陷阱有一个普遍严重的误解，它被认为是和 Python 的 [lambdas](http://docs.python.org/reference/expressions.html#lambda "(在 Python v2.7)") [http://docs.python.org/reference/expressions.html#lambda] 有关。 由 `lambda` 表达式创建的函数并没什么特别， 而且事实上，同样的问题也出现在使用普通的 `定义` 上：

```
def create_multipliers():
    multipliers = []

    for i in range(5):
        def multiplier(x):
            return i * x
        multipliers.append(multiplier)

    return multipliers 
```

### 你应该做的

最一般的解决方案可以说是有点取巧（hack）。由于 Python 拥有在前文提到的为函数默认参数 赋值的行为（参见 可变默认参数 ）,你可以创建一个立即绑定参数的闭包,像下面这样：

```
def create_multipliers():
    return [lambda x, i=i : i * x for i in range(5)] 
```

或者，你可以使用 functools.partial 函数：

```
from functools import partial
from operator import mul

def create_multipliers():
    return [partial(mul, i) for i in range(5)] 
```

### 什么情况下陷阱不是陷阱

有时你就想要闭包有如此表现，迟绑定在很多情况下是不错的。不幸的是，循环创建 独特的函数是一种会使它们出差错的情况。

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.