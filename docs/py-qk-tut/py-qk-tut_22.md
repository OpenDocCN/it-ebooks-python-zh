## Python 深入 04 闭包

[`www.cnblogs.com/vamei/archive/2012/12/15/2772451.html`](http://www.cnblogs.com/vamei/archive/2012/12/15/2772451.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！

闭包(closure)是函数式编程的重要的语法结构。函数式编程是一种编程范式 (而面向过程编程和面向对象编程也都是编程范式)。在面向过程编程中，我们见到过函数(function)；在面向对象编程中，我们见过对象(object)。函数和对象的根本目的是以某种逻辑方式组织代码，并提高代码的可重复使用性(reusability)。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。

不同的语言实现闭包的方式不同。Python 以函数对象为基础，为闭包这一语法结构提供支持的 (我们在[特殊方法与多范式](http://www.cnblogs.com/vamei/archive/2012/11/19/2772441.html)中，已经多次看到 Python 使用对象来实现一些特殊的语法)。Python 一切皆对象，函数这一语法结构也是一个对象。在[函数对象](http://www.cnblogs.com/vamei/archive/2012/07/10/2582772.html)中，我们像使用一个普通对象一样使用函数对象，比如更改函数对象的名字，或者将函数对象作为参数进行传递。

### 函数对象的作用域

和其他对象一样，函数对象也有其存活的范围，也就是函数对象的作用域。函数对象是使用 def 语句定义的，函数对象的作用域与 def 所在的层级相同。比如下面代码，我们在 line_conf 函数的隶属范围内定义的函数 line，就只能在 line_conf 的隶属范围内调用。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
def line_conf():
    def line(x):
        return 2*x+1
    print(line(5))   # within the scope
 line_conf()
print(line(5))       # out of the scope

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

line 函数定义了一条直线(y = 2x + 1)。可以看到，在 line_conf()中可以调用 line 函数，而在作用域之外调用 line 将会有下面的错误：

> NameError: name 'line' is not defined

说明这时已经在作用域之外。

同样，如果使用 lambda 定义函数，那么函数对象的作用域与 lambda 所在的层级相同。

### 闭包

函数是一个对象，所以可以作为某个函数的返回结果。

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
def line_conf():
    def line(x):
        return 2*x+1
    return line       # return a function object

my_line = line_conf()
print(my_line(5)) 

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

上面的代码可以成功运行。line_conf 的返回结果被赋给 line 对象。上面的代码将打印 11。

如果 line()的定义中引用了外部的变量，会发生什么呢？

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
def line_conf():
    b = 15
    def line(x):
        return 2*x+b
    return line       # return a function object
 b = 5
my_line = line_conf()
print(my_line(5)) 

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

我们可以看到，line 定义的隶属程序块中引用了高层级的变量 b，但 b 信息存在于 line 的定义之外 (b 的定义并不在 line 的隶属程序块中)。我们称 b 为 line 的环境变量。事实上，line 作为 line_conf 的返回值时，line 中已经包括 b 的取值(尽管 b 并不隶属于 line)。

上面的代码将打印 25，也就是说，line 所参照的 b 值是函数对象定义时可供参考的 b 值，而不是使用时的 b 值。

一个函数和它的环境变量合在一起，就构成了一个闭包(closure)。在 Python 中，所谓的闭包是一个包含有环境变量取值的函数对象。环境变量取值被保存在函数对象的 __closure__ 属性中。比如下面的代码：

```py
def line_conf():
    b = 15
    def line(x):
        return 2*x+b
    return line       # return a function object
 b = 5 my_line = line_conf()
print(my_line.__closure__)
print(my_line.__closure__[0].cell_contents)

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

__closure__ 里包含了一个元组(tuple)。这个元组中的每个元素是 cell 类型的对象。我们看到第一个 cell 包含的就是整数 15，也就是我们创建闭包时的环境变量 b 的取值。

下面看一个闭包的实际例子：

![复制代码](img/rdb_epub_4593197691164204437.jpg)

```py
def line_conf(a, b):
    def line(x):
        return ax + b
    return line

line1 = line_conf(1, 1)
line2 = line_conf(4, 5)
print(line1(5), line2(5))

```

![复制代码](img/rdb_epub_4593197691164204437.jpg)

这个例子中，函数 line 与环境变量 a,b 构成闭包。在创建闭包的时候，我们通过 line_conf 的参数 a,b 说明了这两个环境变量的取值，这样，我们就确定了函数的最终形式(y = x + 1 和 y = 4x + 5)。我们只需要变换参数 a,b，就可以获得不同的直线表达函数。由此，我们可以看到，闭包也具有提高代码可复用性的作用。

如果没有闭包，我们需要每次创建直线函数的时候同时说明 a,b,x。这样，我们就需要更多的参数传递，也减少了代码的可移植性。利用闭包，我们实际上创建了泛函。line 函数定义一种广泛意义的函数。这个函数的一些方面已经确定(必须是直线)，但另一些方面(比如 a 和 b 参数待定)。随后，我们根据 line_conf 传递来的参数，通过闭包的形式，将最终函数确定下来。

### 闭包与并行运算

闭包有效的减少了函数所需定义的参数数目。这对于并行运算来说有重要的意义。在并行运算的环境下，我们可以让每台电脑负责一个函数，然后将一台电脑的输出和下一台电脑的输入串联起来。最终，我们像流水线一样工作，从串联的电脑集群一端输入数据，从另一端输出数据。这样的情境最适合只有一个参数输入的函数。闭包就可以实现这一目的。

并行运算正称为一个热点。这也是函数式编程又热起来的一个重要原因。函数式编程早在 1950 年代就已经存在，但应用并不广泛。然而，我们上面描述的流水线式的工作并行集群过程，正适合函数式编程。由于函数式编程这一天然优势，越来越多的语言也开始加入对函数式编程范式的支持。

欢迎继续阅读“[Python 快速教程](http://www.cnblogs.com/vamei/archive/2012/09/13/2682778.html)”

如果你喜欢这篇文章，欢迎**推荐**。

如果你认为这篇文章值得更多人阅读，欢迎使用右侧的**“分享”**功能。

#### 技术推动进步，分享促进社区。