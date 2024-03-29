## Python 深入 05 装饰器

[`www.cnblogs.com/vamei/archive/2013/02/16/2820212.html`](http://www.cnblogs.com/vamei/archive/2013/02/16/2820212.html)

作者：Vamei 出处：http://www.cnblogs.com/vamei 欢迎转载，也请保留这段声明。谢谢！

装饰器(decorator)是一种高级 Python 语法。装饰器可以对一个函数、方法或者类进行加工。在 Python 中，我们有多种方法对函数和类进行加工，比如在[Python 闭包](http://www.cnblogs.com/vamei/archive/2012/12/15/2772451.html)中，我们见到函数对象作为某一个函数的返回结果。相对于其它方式，装饰器语法简单，代码可读性高。因此，装饰器在 Python 项目中有广泛的应用。

装饰器最早在 Python 2.5 中出现，它最初被用于加工函数和方法这样的可调用对象(callable object，这样的对象定义有 __call__ 方法)。在 Python 2.6 以及之后的 Python 版本中，装饰器被进一步用于加工类。

### 装饰函数和方法

我们先定义两个简单的数学函数，一个用来计算平方和，一个用来计算平方差：

```py
# get square sum
def square_sum(a, b): return a**2 + b**2

# get square diff
def square_diff(a, b): return a**2 - b**2

```

```py
print(square_sum(3, 4)) print(square_diff(3, 4))

```

在拥有了基本的数学功能之后，我们可能想为函数增加其它的功能，比如打印输入。我们可以改写函数来实现这一点：

```py
# modify: print input

# get square sum
def square_sum(a, b): print("intput:", a, b) return a**2 + b**2

# get square diff
def square_diff(a, b): print("input", a, b) return a**2 - b**2

```

```py
print(square_sum(3, 4)) print(square_diff(3, 4))

```

我们修改了函数的定义，为函数增加了功能。

现在，我们使用装饰器来实现上述修改：

```py
def decorator(F): def new_F(a, b): print("input", a, b) return F(a, b) return new_F # get square sum
@decorator def square_sum(a, b): return a**2 + b**2

# get square diff
@decorator def square_diff(a, b): return a**2 - b**2

print(square_sum(3, 4)) print(square_diff(3, 4))

```

装饰器可以用 def 的形式定义，如上面代码中的 decorator。装饰器接收一个可调用对象作为输入参数，并返回一个新的可调用对象。装饰器新建了一个可调用对象，也就是上面的 new_F。new_F 中，我们增加了打印的功能，并通过调用 F(a, b)来实现原有函数的功能。

定义好装饰器后，我们就可以通过@语法使用了。在函数 square_sum 和 square_diff 定义之前调用@decorator，我们实际上将 square_sum 或 square_diff 传递给 decorator，并将 decorator 返回的新的可调用对象赋给原来的函数名(square_sum 或 square_diff)。 所以，当我们调用 square_sum(3, 4)的时候，就相当于：

```py
square_sum = decorator(square_sum)
square_sum(3, 4)

```

我们知道，Python 中的变量名和对象是分离的。变量名可以指向任意一个对象。从本质上，装饰器起到的就是这样一个重新指向变量名的作用(name binding)，让同一个变量名指向一个新返回的可调用对象，从而达到修改可调用对象的目的。

与加工函数类似，我们可以使用装饰器加工类的方法。

如果我们有其他的类似函数，我们可以继续调用 decorator 来修饰函数，而不用重复修改函数或者增加新的封装。这样，我们就提高了程序的可重复利用性，并增加了程序的可读性。

### 含参的装饰器

在上面的装饰器调用中，比如@decorator，该装饰器默认它后面的函数是唯一的参数。装饰器的语法允许我们调用 decorator 时，提供其它参数，比如@decorator(a)。这样，就为装饰器的编写和使用提供了更大的灵活性。

```py
# a new wrapper layer
def pre_str(pre=''): # old decorator
    def decorator(F): def new_F(a, b): print(pre + "input", a, b) return F(a, b) return new_F return decorator # get square sum
@pre_str('^_^') def square_sum(a, b): return a**2 + b**2

# get square diff
@pre_str('T_T') def square_diff(a, b): return a**2 - b**2

print(square_sum(3, 4)) print(square_diff(3, 4))

```

上面的 pre_str 是允许参数的装饰器。它实际上是对原有装饰器的一个函数封装，并返回一个装饰器。我们可以将它理解为一个含有环境参量的[闭包](http://www.cnblogs.com/vamei/archive/2012/12/15/2772451.html)。当我们使用@pre_str('^_^')调用的时候，Python 能够发现这一层的封装，并把参数传递到装饰器的环境中。该调用相当于:

```py
square_sum = pre_str('^_^') (square_sum)

```

### 装饰类

在上面的例子中，装饰器接收一个函数，并返回一个函数，从而起到加工函数的效果。在 Python 2.6 以后，装饰器被拓展到类。一个装饰器可以接收一个类，并返回一个类，从而起到加工类的效果。

```py
def decorator(aClass): class newClass: def __init__(self, age):
            self.total_display = 0
            self.wrapped = aClass(age) def display(self):
            self.total_display += 1
            print("total display", self.total_display)
            self.wrapped.display() return newClass

@decorator class Bird: def __init__(self, age):
        self.age = age def display(self): print("My age is",self.age)

eagleLord = Bird(5) for i in range(3):
    eagleLord.display()

```

在 decorator 中，我们返回了一个新类 newClass。在新类中，我们记录了原来类生成的对象（self.wrapped），并附加了新的属性 total_display，用于记录调用 display 的次数。我们也同时更改了 display 方法。

通过修改，我们的 Bird 类可以显示调用 display 的次数了。

### 总结

装饰器的核心作用是 name binding。这种语法是 Python 多编程范式的又一个体现。大部分 Python 用户都不怎么需要定义装饰器，但有可能会使用装饰器。鉴于装饰器在 Python 项目中的广泛使用，了解这一语法是非常有益的。

欢迎继续阅读“[Python 快速教程](http://www.cnblogs.com/vamei/archive/2012/09/13/2682778.html)”