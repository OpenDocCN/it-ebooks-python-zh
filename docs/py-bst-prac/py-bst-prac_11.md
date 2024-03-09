### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# 日志（Logging）

[日志](https://docs.python.org/2/library/logging.html#module-logging) [https://docs.python.org/2/library/logging.html#module-logging] 模块自 2.3 版本开始便是 Python 标准库的一部分。它被简洁的描述在 [**PEP 282**](https://www.python.org/dev/peps/pep-0282) [https://www.python.org/dev/peps/pep-0282]。众所周知，除了 [基础日志指南](http://docs.python.org/howto/logging.html#logging-basic-tutorial) [http://docs.python.org/howto/logging.html#logging-basic-tutorial] 部分，该文档并不容易阅读。

日志的两个目的：

*   **诊断日志** 记录与应用程序操作相关的日志。例如，用户遇到的报错信息，可通过搜索诊断日志获得上下文信息。
*   **审计日志** 为商业分析而记录的日志。从审计日志中，可提取用户的交易信息，并结合其他用户资料构成用户报告或者用来优化商业目标。

## ... 或者用打印(Print)?

当需要在命令行应用中显示帮助文档时， `打印` 是一个相对于日志更好的选择。而在其他时候，日志总能优于 `打印` ，理由如下：

*   日志事件产生的 [日志记录](https://docs.python.org/library/logging.html#logrecord-attributes) [https://docs.python.org/library/logging.html#logrecord-attributes] ，包含清晰可用的诊断信息，如文件名称、路径、函数名和行数等。
*   包含日志模块的应用，默认可通过根记录器对应用的日志流进行访问，除非你将日志过滤了。
*   可通过 [logging.Logger.setLevel](https://docs.python.org/2/library/logging.html#logging.Logger.setLevel) [https://docs.python.org/2/library/logging.html#logging.Logger.setLevel] 方法进行有选择的日志记录，或可通过设置 `logging.Logger.disabled` 属性为 `True` 来屏蔽日志记录。

## 库中的日志

[日志指南](http://docs.python.org/howto/logging.html) [http://docs.python.org/howto/logging.html] 中含 [库日志配置](https://docs.python.org/howto/logging.html#configuring-logging-for-a-library) [https://docs.python.org/howto/logging.html#configuring-logging-for-a-library] 的说明。由于是 *用户* ，而非库来指明如何响应日志事件，因此这里有一个值得反复说明的忠告：

注解

强烈建议不要向你的库日志中加入除 NullHandler 外的其它处理程序。

在库中，声明日志的最佳方式是通过 `__name__` 全局变量：[日志](https://docs.python.org/2/library/logging.html#module-logging) [https://docs.python.org/2/library/logging.html#module-logging] 模块通过点(dot)运算符创建层级排列的日志，因此，用 `__name__` 可以避免名字冲突。

以下是来自 [资源请求](https://github.com/kennethreitz/requests) [https://github.com/kennethreitz/requests] 的一个例子–把它放置在你的 `__init__.py` 文件中

```
# 设置默认日志处理方式，避免“未找到处理方法”的警告。
import logging
try:  # Python 2.7+
    from logging import NullHandler
except ImportError:
    class NullHandler(logging.Handler):
        def emit(self, record):
            pass

logging.getLogger(__name__).addHandler(NullHandler()) 
```

## 应用程序中的日志

应用程序开发的权威指南， [应用的 12 要素](http://12factor.net) [http://12factor.net] ，也在其中一节描述了 [日志的作用](http://12factor.net/logs) [http://12factor.net/logs] 。它特别强调将日志视为事件流，并将其发送至由应用环境所处理的标准输出中。

配置日志至少有以下三种方式：

*   使用 INI 格式文件：

> *   **优点**: 使用 [`logging.config.listen()`](http://docs.python.org/library/logging.config.html#logging.config.listen "(在 Python v2.7)") [http://docs.python.org/library/logging.config.html#logging.config.listen] 函数监听 socket，可在运行过程中更新配置
> *   **缺点**: 通过源码控制日志配置较少（ *例如* 子类化定制的过滤器或记录器）。

*   使用字典或 JASON 格式文件：

> *   **优点**: 除了可在运行时动态更新，在 Python 2.6 之后，还可通过 [`json`](http://docs.python.org/library/json.html#module-json "(在 Python v2.7)") [http://docs.python.org/library/json.html#module-json] 模块从其它文件中导入配置。
> *   **缺点**: 很难通过源码控制日志配置。

*   使用源码：

> *   **优点**: 对配置绝对的控制。
> *   **缺点**: 对配置的更改需要对源码进行修改。

### 通过 INI 文件进行配置的例子

我们假设文件名为 `logging_config.ini` 。关于文件格式的更多细节，请参见 [日志指南](http://docs.python.org/howto/logging.html) [http://docs.python.org/howto/logging.html] 中的 [日志配置](https://docs.python.org/howto/logging.html#configuring-logging) [https://docs.python.org/howto/logging.html#configuring-logging] 部分。

```
[loggers]
keys=root

[handlers]
keys=stream_handler

[formatters]
keys=formatter

[logger_root]
level=DEBUG
handlers=stream_handler

[handler_stream_handler]
class=StreamHandler
level=DEBUG
formatter=formatter
args=(sys.stderr,)

[formatter_formatter]
format=%(asctime)s %(name)-12s %(levelname)-8s %(message)s 
```

然后在源码中调用 `logging.config.fileConfig()` 方法：

```
import logging
from logging.config import fileConfig

fileConfig('logging_config.ini')
logger = logging.getLogger()
logger.debug('often makes a very good meal of %s', 'visiting tourists') 
```

### 通过字典进行配置的例子

Python 2.7 中，你可以使用字典实现详细配置。[**PEP 391**](https://www.python.org/dev/peps/pep-0391) [https://www.python.org/dev/peps/pep-0391] 包含了一系列字典配置的强制和 非强制的元素。

```
import logging
from logging.config import dictConfig

logging_config = dict(
    version = 1,
    formatters = {
        'f': {'format':
              '%(asctime)s  %(name)-12s  %(levelname)-8s  %(message)s'}
        },
    handlers = {
        'h': {'class': 'logging.StreamHandler',
              'formatter': 'f',
              'level': logging.DEBUG}
        },
    loggers = {
        'root': {'handlers': ['h'],
                 'level': logging.DEBUG}
        }
)

dictConfig(logging_config)

logger = logging.getLogger()
logger.debug('often makes a very good meal of %s', 'visiting tourists') 
```

### 通过源码直接配置的例子

```
import logging

logger = logging.getLogger()
handler = logging.StreamHandler()
formatter = logging.Formatter(
        '%(asctime)s  %(name)-12s  %(levelname)-8s  %(message)s')
handler.setFormatter(formatter)
logger.addHandler(handler)
logger.setLevel(logging.DEBUG)

logger.debug('often makes a very good meal of %s', 'visiting tourists') 
```

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.