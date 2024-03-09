### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# 网络

## Twisted

[Twisted](http://twistedmatrix.com/trac/) [http://twistedmatrix.com/trac/] 是一款基于事件驱动的网络引擎框架。 支持创建基于不同网络协议的应用，包括 http 服务器与客户端，SMTP 应用，POP3，IMAP 或 SSH 协议，即时通讯 [等等](http://twistedmatrix.com/trac/wiki/Documentation) [http://twistedmatrix.com/trac/wiki/Documentation]。

## PyZMQ

[PyZMQ](http://zeromq.github.com/pyzmq/) [http://zeromq.github.com/pyzmq/] 是 [ZeroMQ](http://www.zeromq.org/) [http://www.zeromq.org/] 的 Python 捆绑库 (binding)，ZeroMQ 是一款高效率的异步消息库，它的一个显著优点就是能 被用做消息队列且不需要消息代理。基本模式包括：

*   请求-回应模式：连接多个客户端与多个服务端，是远程过程调用和任务分配的模式。
*   发布-订阅模式：连接多个发布者和多个订阅者，是数据分配模式。
*   推-挽模式 (或管道模式) ：连接多个步骤，包括循环的输入/输出端，是并行任务分配 和收集模式。

想要快速入门，阅读 [ZeroMQ 指南](http://zguide.zeromq.org/page:all) [http://zguide.zeromq.org/page:all].

## gevent

[gevent](http://www.gevent.org/) [http://www.gevent.org/] 是一款基于协程的 Python 网络库，它使用 greenlets 提供在 libev 事件循环之上的高层次异步 API。

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.