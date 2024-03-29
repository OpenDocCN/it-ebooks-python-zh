# 持续集成

## 为什么?

Martin Fowler 和 Kent Beck 首次提出 [Continuous Integration](http://martinfowler.com/articles/continuousIntegration.html) [http://martinfowler.com/articles/continuousIntegration.html] （简称：CI），将之描述为：

> 持续集成是一种软件开发实践：许多团队频繁地集成他们的工作，每位成员通常进行 日常集成，进而每天会有多种集成。每个集成会由自动的构建（包括测试）来尽可能快地 检测错误。许多团队发现这种方法可以显著的减少集成问题并且可以使团队的开发更加 快捷。

## Jenkins

[Jenkins CI](http://jenkins-ci.org) [http://jenkins-ci.org] 可扩展的持续集成引擎。 使用它吧！

## Buildbot

[Buildbot](http://docs.buildbot.net/current/) [http://docs.buildbot.net/current/] 是一个检查代码变化的自动化编译/ 测试的 Python 系统。

## Mule

[Mule](http://www.mulesoft.org/documentation/display/current/Mule+Fundamentals) [http://www.mulesoft.org/documentation/display/current/Mule+Fundamentals] 是一个轻量级的集成平台，它允许你在任何地方连接任何事物。你可以使用 Mule 智能地管理 节点之间的消息路由、数据映射、编制、可靠性、安全性和可扩展性。将其他系统和应用添加 到 Mule 中，使它处理系统间的所有通信，从而允许你追踪和监控每个发生的事件。

## Tox

[Tox](http://tox.readthedocs.org/en/latest/) [http://tox.readthedocs.org/en/latest/] 是一款为 Python 软件提供打包、测试和 开发的自动化工具，基于命令行或 CI 服务器。它是一个通用的虚拟环境管理和测试的命令行 工具，提供如下特性：

*   检查包在不同的 Python 版本和解释器下安装正确
*   在每个环境中运行你的测试、配置测试工具的选择
*   作为前端持续集成服务器，减少样板文件，合并了 CI 和基于 shell 的测试。

## Travis-CI

[Travis-CI](https://travis-ci.org/) [https://travis-ci.org/] 是一个分布式 CI 服务器，免费为开源项目构建测试。 它提供多个 worker 运行 Python 测试，并能和 GitHub 无缝集成。你甚至可以用它对你的 Pull Requests 评论是否构建这个特定的变更集。Travis-ci 是一个很好的、简单的方式去了解持续集成。

作为开始，将 `.travis.yml` 文件加入到你的仓库中，内容如下:

```py
language: python
python:
  - "2.6"
  - "2.7"
  - "3.2"
  - "3.3"
# command to install dependencies
script: python tests/test_all_of_the_units.py
branches:
  only:
    - master 
```

这将会使你的工程在罗列的 Python 版本中，用给定的脚本进行测试，而且只会构建主干分支。 有许多可供开启的选项，包括通知、步骤前后等。 [Travis-ci docs](http://about.travis-ci.org/docs/) [http://about.travis-ci.org/docs/] 详尽地解释了所有这些操作。

为了激活你的工程的测试，去 [travis-ci 网站](https://travis-ci.org/) [https://travis-ci.org/] 登录你的 GitHub 账号。然后在你的 profile 设置中激活你的工程。现在，每一次 push 到 GitHub 上的提交将会运行 你的工程中的测试。

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.