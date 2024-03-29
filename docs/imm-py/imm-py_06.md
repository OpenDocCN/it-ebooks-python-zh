# 三元运算符

三元运算符通常在 Python 里被称为条件表达式，这些表达式基于真(true)/假(not)的条件判断，在 Python 2.4 以上才有了三元操作。

下面是一个伪代码和例子：

**伪代码:**

```py
#如果条件为真，返回真 否则返回假
condition_is_true if condition else condition_is_false 
```

**例子:**

```py
is_fat = True
state = "fat" if is_fat else "not fat" 
```

它允许用简单的一行快速判断，而不是使用复杂的多行`if`语句。 这在大多数时候非常有用，而且可以使代码简单可维护。

另一个晦涩一点的用法比较少见，它使用了元组，请继续看：

**伪代码:**

```py
#(返回假，返回真)[真或假]
(if_test_is_false, if_test_is_true)[test] 
```

**例子:**

```py
fat = True
fitness = ("skinny", "fat")[fat]
print("Ali is ", fitness)
#输出: Ali is fat 
```

这之所以能正常工作，是因为在 Python 中，True 等于 1，而 False 等于 0，这就相当于在元组中使用 0 和 1 来选取数据。

上面的例子没有被广泛使用，而且 Python 玩家一般不喜欢那样，因为没有 Python 味儿(Pythonic)。这样的用法很容易把真正的数据与 true/false 弄混。

另外一个不使用元组条件表达式的缘故是因为在元组中会把两个条件都执行，而 `if-else` 的条件表达式不会这样。

例如:

```py
condition = True
print(2 if condition else 1/0)
#输出: 2

print((1/0, 2)[condition])
#输出 ZeroDivisionError 异常 
```

这是因为在元组中是先建数据，然后用 True(1)/False(0)来索引到数据。 而`if-else`条件表达式遵循普通的`if-else`逻辑树， 因此，如果逻辑中的条件异常，或者是重计算型（计算较久）的情况下，最好尽量避免使用元组条件表达式。