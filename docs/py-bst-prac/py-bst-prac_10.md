### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# 测试你的代码

测试你的代码非常重要。

常常将测试代码和运行代码一起写是一种非常好的习惯。聪明地使用这种方法将会帮助你更加精确地定义代码的含义，并且代码的耦合性更低。

测试的通用规则：

*   测试单元应该集中于小部分的功能，并且证明它是对的。
*   每个测试单元应该完全独立。每个都能够单独运行，除了调用的命令，都需在测试套件中。要想实现这个规则，测试单元应该加载最新的数据集，之后再做一些清理。这通常用方法 `setUp()` 和 `tearDown()` 处理。
*   尽量使测试单元快速运行。如果一个单独的测试单元需要较长的时间去运行，开发进度将会延迟，测试单元将不能如期常态性运行。有时候，因为测试单元需要复杂的数据结构，并且当它运行时每次都要加载，所以其运行时间较长。把运行吃力的测试单元放在单独的测试组件中，并且按照需要运行其它测试单元。
*   学习使用工具，学习如何运行一个单独的测试用例。然后，当在一个模块中开发了一个功能时，经常运行这个功能的测试用例，理想情况下，一切都将自动。
*   在编码会话前后，要常常运行完整的测试组件。只有这样，你才会坚信剩余的代码不会中断。
*   实现钩子（hook）是一个非常好的主意。因为一旦把代码放入分享仓库中，这个钩子可以运行所有的测试单元。
*   如果你在开发期间不得不打断自己的工作，写一个被打断的单元测试，它关于下一步要开发的东西。当回到工作时，你将更快地回到原先被打断的地方，并且步入正轨。
*   当你调试代码的时候，首先需要写一个精确定位 bug 的测试单元。尽管这样做很难，但是捕捉 bug 的单元测试在项目中很重要。
*   测试函数使用长且描述性的名字。这边的样式指导与运行代码有点不一样，运行代码更倾向于使用短的名字，而测试函数不会直接被调用。在运行代码中，square()或者甚至 sqr()这样的命名都是可以的，但是在测试代码中，你应该这样取名 test_square_of_number_2()，test_square_negative_number()。当测试单元失败时，函数名应该显示，而且尽可能具有描述性。
*   当发生了一些问题，或者不得不改变时，如果代码中有一套不错的测试单元，维护将很大一部分依靠测试组件解决问题，或者修改确定的行为。因此测试代码应该尽可能多读，甚至多于运行代码。目的不明确的测试单元在这种情况下没有多少用处。
*   测试代码的另外一个用处是作为新开发人员的入门。当工作基于代码，运行并且阅读相关的测试代码是一个非常好的做法。开发人员将会或者应该发现热点，而这将引起困难和其它情况，如果他们一定要加一些功能，第一步应该是要增加一个测试单元，通过这种方法，确保新功能不再是一个没有被嵌入到接口中的工作路径。

## 基本

### 单元测试

[`unittest`](http://docs.python.org/library/unittest.html#module-unittest "(在 Python v2.7)") [http://docs.python.org/library/unittest.html#module-unittest] 包括 Python 标准库中的测试模型。任何一个使用过 Junit，nUnit,或 CppUnit 工具的人对它的 API 都会比较熟悉。

创建测试用例通过继承 [`unittest.TestCase`](http://docs.python.org/library/unittest.html#unittest.TestCase "(在 Python v2.7)") [http://docs.python.org/library/unittest.html#unittest.TestCase] 来实现.

```
import unittest

def fun(x):
    return x + 1

class MyTest(unittest.TestCase):
    def test(self):
        self.assertEqual(fun(3), 4) 
```

因为 Python 2.7 单元测试也包括自己的发现机制。

> [在标准库文档中单元测试](http://docs.python.org/library/unittest.html) [http://docs.python.org/library/unittest.html]

### 文档测试

> [`doctest`](http://docs.python.org/library/doctest.html#module-doctest "(在 Python v2.7)") [http://docs.python.org/library/doctest.html#module-doctest] 模块查找零碎文本，就像在 Python 中 docstrings 内的交互式会话，执行那些会话以证实工作正常。

doctest 模块的用例相比之前的单元测试有所不同：它们通常不是很详细，并且不会用特别的用例或者处理模糊的回归 bug。作为模块和其部件主要用例的表述性文档，doctest 模块非常有用。

在函数中一个简单的 doctest:

```
def square(x):
    """Squares x.

 >>> square(2)
 4
 >>> square(-2)
 4
 """

    return x * x

if __name__ == '__main__':
    import doctest
    doctest.testmod() 
```

从命令行中运行这个模块时，doctest 模块将会运行并且细述是否一切如 docstrings 中描述一样工作良好。

## 工具

### py.test

相比于 Python 标准的单元测试模块,py.test 是一个没有模板的选择。

```
$ pip install pytest 
```

尽管这个测试工具功能完备，并且可扩展，但是它语法很简单。创建一个测试组件和写一个带有诸多函数的模块一样容易：

```
# content of test_sample.py
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5 
```

运行命令 py.test

```
$ py.test
=========================== test session starts ============================
platform darwin -- Python 2.7.1 -- pytest-2.2.1
collecting ... collected 1 items

test_sample.py F

================================= FAILURES =================================
_______________________________ test_answer ________________________________

 def test_answer():
>       assert func(3) == 5
E       assert 4 == 5
E        +  where 4 = func(3)

test_sample.py:5: AssertionError
========================= 1 failed in 0.02 seconds ========================= 
```

要比单元测试模型中相同功能所要求的工作量少得多。

> [py.test](http://pytest.org/latest/) [http://pytest.org/latest/]

### Nose

nose 继承测试单元，能够使测试更加容易。

```
$ pip install nose 
```

nose 自动化测试发现并节省人工创建测试组件的麻烦。它也提供各种插件，例如 xUnit 兼容性测试输出，覆盖度报告和测试选择。

> [nose](http://readthedocs.org/docs/nose/en/latest/) [http://readthedocs.org/docs/nose/en/latest/]

### tox

tox 是自动化测试管理和针对多种解释器配置测试工具。

```
$ pip install tox 
```

tox 允许通过简单的初始化样式配置文件，配置复杂的多参数测试矩阵。

> [tox](http://testrun.org/tox/latest/) [http://testrun.org/tox/latest/]

### Unittest2

Unittest2 是 Python2.7 中 unittest 模型的补丁，它的 API 有所改善，并且对 Python 之前版本中已有的内容有了更好的说明。

如果使用 Python2.6 版本或者以下，需要使用 pip 安装 unittest2。

```
$ pip install unittest2 
```

将来你可能想要以 unittest 之名导入模块，目的是更容易地把代码移植到新的版本中。

```
import unittest2 as unittest

class MyTest(unittest.TestCase):
    ... 
```

如果切换到新的 Python 版本，并且不再需要 unittest2 模块，你只需要在测试模块中改变 import 内容，而不必改变其它代码。

> [unittest2](http://pypi.python.org/pypi/unittest2) [http://pypi.python.org/pypi/unittest2]

### mock

`unittest.mock` 是 Python 中用于测试的一个库。在 Python3.3 版本中，标准库中就有。 [标准库](https://docs.python.org/dev/library/unittest.mock) [https://docs.python.org/dev/library/unittest.mock].

对于 Python 相对早的版本，如下操作：

```
$ pip install mock 
```

在测试环境下，使用 mock 对象能够替换部分系统，并且对它们如何被使用做了声明。 例如，你可以对一个方法打猴子补丁：

例如，你可以对一个方法打猴子补丁：

```
from mock import MagicMock
thing = ProductionClass()
thing.method = MagicMock(return_value=3)
thing.method(3, 4, 5, key='value')

thing.method.assert_called_with(3, 4, 5, key='value') 
```

在测试环境下，对于模型中的 mock 类或对象，使用补丁修饰器。在下面这个例子中，一直返回相同结果的外部查询系统使用 mock 替换（但仅用在测试期间）。

```
def mock_search(self):
    class MockSearchQuerySet(SearchQuerySet):
        def __iter__(self):
            return iter(["foo", "bar", "baz"])
    return MockSearchQuerySet()

# SearchForm here refers to the imported class reference in myapp,
# not where the SearchForm class itself is imported from
@mock.patch('myapp.SearchForm.search', mock_search)
def test_new_watchlist_activities(self):
    # get_search_results runs a search and iterates over the result
    self.assertEqual(len(myapp.get_search_results(q="fish")), 3) 
```

mock 有许多其它方法，你可以配置它，并且控制它的动作。

> [mock](http://www.voidspace.org.uk/python/mock/) [http://www.voidspace.org.uk/python/mock/]

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.