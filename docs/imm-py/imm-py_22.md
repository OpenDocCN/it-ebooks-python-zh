# 目标 Python2+3

# 22\. 目标 Python2+3

很多时候你可能希望你开发的程序能够同时兼容 Python2+和 Python3+。

试想你有一个非常出名的 Python 模块被很多开发者使用着，但并不是所有人都只使用 Python2 或者 Python3。这时候你有两个办法。第一个办法是开发两个模块，针对 Python2 一个，针对 Python3 一个。还有一个办法就是调整你现在的代码使其同时兼容 Python2 和 Python3。

本节中，我将介绍一些技巧，让你的脚本同时兼容 Python2 和 Python3。

**Future 模块导入**

第一种也是最重要的方法，就是导入`__future__`模块。它可以帮你在 Python2 中导入 Python3 的功能。这有一组例子：

上下文管理器是 Python2.6+引入的新特性，如果你想在 Python2.5 中使用它可以这样做：

```py
from __future__ import with_statement 
```

在 Python3 中`print`已经变为一个函数。如果你想在 Python2 中使用它可以通过`__future__`导入：

```py
print
# Output:

from __future__ import print_function
print(print)
# Output: <built-in function print> 
```

**模块重命名**

首先，告诉我你是如何在你的脚本中导入模块的。大多时候我们会这样做：

```py
import foo 
# or
from foo import bar 
```

你知道么，其实你也可以这样做：

```py
import foo as foo 
```

这样做可以起到和上面代码同样的功能，但最重要的是它能让你的脚本同时兼容 Python2 和 Python3。现在我们来看下面的代码：

```py
try:
    import urllib.request as urllib_request  # for Python 3
except ImportError:
    import urllib2 as urllib_request  # for Python 2 
```

让我来稍微解释一下上面的代码。
我们将模块导入代码包装在`try/except`语句中。我们是这样做是因为在 Python 2 中并没有`urllib.request`模块。这将引起一个`ImportError`异常。而在 Python2 中`urllib.request`的功能则是由`urllib2`提供的。所以,当我们试图在 Python2 中导入`urllib.request`模块的时候，一旦我们捕获到`ImportError`我们将通过导入`urllib2`模块来代替它。

最后，你要了解`as`关键字的作用。它将导入的模块映射到`urllib.request`，所以我们通过`urllib_request`这个别名就可以使用`urllib2`中的所有类和方法了。

**过期的 Python2 内置功能**

另一个需要了解的事情就是 Python2 中有 12 个内置功能在 Python3 中已经被移除了。要确保在 Python2 代码中不要出现这些功能来保证对 Python3 的兼容。这有一个强制让你放弃 12 内置功能的方法：

```py
from future.builtins.disabled import * 
```

现在，只要你尝试在 Python3 中使用这些被遗弃的模块时，就会抛出一个`NameError`异常如下：

```py
from future.builtins.disabled import *

apply()
# Output: NameError: obsolete Python 2 builtin apply is disabled 
```

**标准库向下兼容的外部支持**

有一些包在非官方的支持下为 Python2 提供了 Python3 的功能。例如，我们有：

*   enum `pip install enum34`
*   singledispatch `pip install singledispatch`
*   pathlib `pip install pathlib`

想更多了解，在 Python 文档中有一个[全面的指南](https://docs.python.org/3/howto/pyporting.html)可以帮助你让你的代码同时兼容 Python2 和 Python3。