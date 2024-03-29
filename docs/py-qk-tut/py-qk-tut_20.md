## Python 深入 02 上下文管理器

[`www.cnblogs.com/vamei/archive/2012/11/23/2772445.html`](http://www.cnblogs.com/vamei/archive/2012/11/23/2772445.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！ 

上下文管理器(context manager)是 Python2.5 开始支持的一种语法，用于规定某个对象的使用范围。一旦进入或者离开该使用范围，会有特殊操作被调用 (比如为对象分配或者释放内存)。它的语法形式是 with...as...

### 关闭文件

我们会进行这样的操作：打开文件，读写，关闭文件。程序员经常会忘记关闭文件。上下文管理器可以在不需要文件的时候，自动关闭文件。

下面我们看一下两段程序：

```py
# without context manager
f = open("new.txt", "w")
print(f.closed)               # whether the file is open
f.write("Hello World!")
f.close() print(f.closed)

```

以及：

```py
# with context manager
with open("new.txt", "w") as f:
print(f.closed)
    f.write("Hello World!")
print(f.closed)

```

两段程序实际上执行的是相同的操作。我们的第二段程序就使用了上下文管理器 (with...as...)。上下文管理器有隶属于它的程序块。当隶属的程序块执行结束的时候(也就是不再缩进)，上下文管理器自动关闭了文件 (我们通过 f.closed 来查询文件是否关闭)。我们相当于使用缩进规定了文件对象 f 的使用范围。

上面的上下文管理器基于 f 对象的 __exit__()特殊方法(还记得我们如何利用特殊方法来实现各种语法？参看[特殊方法与多范式](http://www.cnblogs.com/vamei/archive/2012/11/19/2772441.html))。当我们使用上下文管理器的语法时，我们实际上要求 Python 在进入程序块之前调用对象的 __enter__()方法，在结束程序块的时候调用 __exit__()方法。对于文件对象 f 来说，它定义了 __enter__()和 __exit__()方法(可以通过 dir(f)看到)。在 f 的 __exit__()方法中，有 self.close()语句。所以在使用上下文管理器时，我们就不用明文关闭 f 文件了。

### 自定义

任何定义了 __enter__()和 __exit__()方法的对象都可以用于上下文管理器。文件对象 f 是内置对象，所以 f 自动带有这两个特殊方法，不需要自定义。

下面，我们自定义用于上下文管理器的对象，就是下面的 myvow：

```py
# customized object

class VOW(object): def __init__(self, text):
        self.text = text def __enter__(self):
        self.text = "I say: " + self.text    # add prefix
        return self                          # note: return an object
    def __exit__(self,exc_type,exc_value,traceback):
        self.text = self.text + "!"          # add suffix
 with VOW("I'm fine") as myvow: print(myvow.text) print(myvow.text)

```

我们的运行结果如下:

```py
I say: I'm fine
I say: I'm fine!

```

我们可以看到，在进入上下文和离开上下文时，对象的 text 属性发生了改变(最初的 text 属性是"I'm fine")。

__enter__()返回一个对象。上下文管理器会使用这一对象作为 as 所指的变量，也就是 myvow。在 __enter__()中，我们为 myvow.text 增加了前缀 ("I say: ")。在 __exit__()中，我们为 myvow.text 增加了后缀("!")。

注意: __exit__()中有四个参数。当程序块中出现异常(exception)，__exit__()的参数中 exc_type, exc_value, traceback 用于描述异常。我们可以根据这三个参数进行相应的处理。如果正常运行结束，这三个参数都是 None。在我们的程序中，我们并没有用到这一特性。

### 总结：

通过上下文管理器，我们控制对象在程序不同区间的特性。上下文管理器(with EXPR as VAR)大致相当于如下流程:

```py
# with EXPR as VAR:
 VAR = EXPR
VAR = VAR.__enter__() try:
    BLOCK finally:
    VAR.__exit__()

```

由于上下文管理器带来的便利，它是一个值得使用的工具。