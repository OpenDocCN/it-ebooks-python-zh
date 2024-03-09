### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# Web 应用

Python 作为一门强大的脚本语言，能够适应快速原型和较大项目的制作，它被广泛用于 web 应用程序的开发中。

## Context

### WSGI

Web 服务网关接口（Web Server Gateway Interface，简称“WSGI”）是一种在 web 服务器和 Python web 应用程序或框架之间的标准接口。通过标准化 web 服务器和 Python web 应用程序或框架之间的行为和通信，WSGI 使得编写可移植的的 Python web 代码变为可能，使其能够部署在任何 WSGI 可用的 web 服务器 上。WSGI 的文档在 [**PEP 3333**](https://www.python.org/dev/peps/pep-3333) [https://www.python.org/dev/peps/pep-3333]。

## 框架

广义地说，web 框架包含一系列库和一个主要的 handler，这样你就能够构建自己的代码来实现 web 应用（比如说一个交互式的网站）。大多数 web 框架包含模式和工具，至少实现以下功能：

URL 路由（URL Routing）

将输入的 HTTP 请求匹配到特定的 Python 代码用来调用

请求和响应对象（Request and Response Objects）

封装来自或发送给用户浏览器的信息

模板引擎（Template Engine）

能够将实现应用的 Python 代码逻辑和其要产生输出的 HTML（或其他）分离开

web 服务器开发（Development Web Server）

在开发机上运行 HTTP 服务器，从而快速开发；当文件更新时自动更新服务端代码。

### Django

[Django](http://www.djangoproject.com) [http://www.djangoproject.com] 是一个功能齐备的 web 应用框架。通过提供众多工具和模式，Django 旨在使得快速构建复杂的、有数据库支持的 web 应用成为可能，同时鼓励使用它作为编写代码的最佳实践。

Django 拥有非常庞大和活跃的社区。此外，许多预构建的 [可重用模块](http://djangopackages.com/) [http://djangopackages.com/] 可以原样合并到新工程中，或者定制成符合需求的样子。

在 [美国](http://djangocon.us) [http://djangocon.us] 和 [欧洲](http://djangocon.eu) [http://djangocon.eu] 均有每年度的 Django 会议。

### Flask

[Flask](http://flask.pocoo.org/) [http://flask.pocoo.org/] 是一款针对 Python 的“微型框架”。Flask 不是提供一切你可能需要的内容，而是实现了 web 应用框架中最常用的核心组件，比如说 URL 路由、请求和响应对象和模板等。作为 Flask 的用户，由你来决定选择和集成其他你可能用到的组件，比如说数据库访问或者表单生成和验证等。对于众多流行模块， [Extensions](http://flask.pocoo.org/extensions/) [http://flask.pocoo.org/extensions/] 或许符合你的需求。

Flask 的 **支持** 能在邮件列表中找到。发邮件给 flask@librelist.com，然后会回复到特定邮箱。

### Web.py

[Web.py](http://webpy.org/) [http://webpy.org/] web.py 是一个针对 Python 的 web 框架，简单而又强大。Web.py 中没有向导或者模板网站，必须从头构建。它不提供管理工具。Web.py 是 Aaron Swartz 的作品，是他 Reddit.com 工作时开发出来的。

### Web2py

一个全栈 web 框架和平台注重易用性。它关注快速开发，倾向于约定而非配置方法，以及遵循模型-视图-控制器（model-view-controller,MVC）的架构模式。

### Werkzeug

[Werkzeug](http://werkzeug.pocoo.org/) [http://werkzeug.pocoo.org/] 其实并不是真正的框架，但确实是一个构建 web 应用的非常有用的工具集。它提供 URL 路由工具、请求和响应对象，以及基本的开发服务端。它常用于需要更多灵活度而非在其他 web 框架中常见到的 web 应用中。

在它的 [mailing list](http://werkzeug.pocoo.org/community/#mailinglist) [http://werkzeug.pocoo.org/community/#mailinglist].、 可以找到支持内容。

### Tornado

[Tornado](http://www.tornadoweb.org/) [http://www.tornadoweb.org/] 是一个可扩展的、非阻塞的 web 服务端和 web 应用框架，拥有简单相近的用法。Tornado 以高性能文明。它最早是为了 [friendfeed](http://friendfeed.com/) [http://friendfeed.com/] ，一个实时聊天和博客矽统，而开发的。

在 Jinja2 模板引擎例子中，它被用来渲染页面。

### Pyramid

[Pyramid](http://www.pylonsproject.org/) [http://www.pylonsproject.org/] 介于大型框架（诸如 Django）和小型框架之间：它拥有大量的库和功能，因此并不是轻量级的。另一方面，它并没有提供 Django 所提供的所有功能。相反，Pyramid 提供对最常见任务的基本支持，并有相当大的可扩展性。此外，Pyramid 有完善的 [文档](http://docs.pylonsproject.org/en/latest/docs/pyramid.html) [http://docs.pylonsproject.org/en/latest/docs/pyramid.html]。它额外带有 Werkzeug 调试器，允许你调试正在浏览器中运行的 web 应用。

在 [文档](http://docs.pylonsproject.org/en/latest/index.html#support-desc) [http://docs.pylonsproject.org/en/latest/index.html#support-desc] 中获取 **支持**。

## Web 服务端

 ### Nginx

[Nginx](http://nginx.org/) [http://nginx.org/] （发音为 “engine-x”）是一个 web 服务器，并是 HTTP、SMTP 和其他协议的反向代理。它由其高性能、相对简洁以及对众多应用服务器（比如 WSGI 服务器）兼容而著名。它也拥有便利的特性，比如负载均衡、基本的认证、流等。Nginx 被设计为承载高负载的网站，并逐渐变得广为流行。  ## WSGI 服务器

独立 WSGI 服务器相比传统 web 服务器，使用更少的资源，并提供最高的性能 [[3]](#id32)。

 ### Gunicorn

[Gunicorn](http://gunicorn.org/) [http://gunicorn.org/] （Green Unicorn，绿色独角兽）是一个 WSGI 服务器，用来支持 Python 应用。它是 Ruby 的 [Unicorn](http://unicorn.bogomips.org/) [http://unicorn.bogomips.org/] 服务器的 Python 实现。Unicorn 被设计成轻量级的、易于使用的、并使用许多 UNIX 特性（UNIX idioms）。Gunicorn 不是被设计成面向互联网的 – 它被设计成运行于 Nginx 之后，缓存慢请求，以及关注其他重要的内容。在 [Gunicorn help](http://gunicorn.org/index.html#deployment) [http://gunicorn.org/index.html#deployment] 能找到 Nginx + Gunicorn 的简单配置步骤。  ### uWSGI

[uWSGI](https://uwsgi-docs.readthedocs.org) [https://uwsgi-docs.readthedocs.org] 用来构建全栈式的主机服务。除了进程管理、进程监控和其他功能外，uWSGI 也能作为一个应用服务器，适用于多种编程语言和协议 - 包括 Python 和 WSIG。uWSGI 既能当作独立的 web 路由器来运行，也能运行在一个完整 web 服务器（比如 Nginx 或 Apache）之后。对于后者，web 服务器可以基于 [uwsgi 协议](https://uwsgi-docs.readthedocs.org/en/latest/Protocol.html) [https://uwsgi-docs.readthedocs.org/en/latest/Protocol.html] 配置 uWSGI 和应用的操作。uWSGI 的 web 服务器支持允许动态配置 Python、传递环境变量以及进一步优化。要看更多细节，请看 [uWSGI 魔法变量](https://uwsgi-docs.readthedocs.org/en/latest/Vars.html) [https://uwsgi-docs.readthedocs.org/en/latest/Vars.html]。  ## 服务端最佳实践

如今，自承载 Python 应用的主体托管与 WSGI 服务器（比如说 Gunicorn）或是直接或间接在轻量级 web 服务器（比如说 nginx）之后。

WSGI 服务器为 Python 应用服务，它能更好的处理诸如静态文件服务、请求路由、DDoS 保护和基本认证的任务。 

## Hosting

### 平台即服务（Platform-as-a-Service）

平台即服务（PaaS）是一种云计算基础设施类型，抽象和管理基础设施、路由和网络应用的扩展。使用 PaaS 时，应用开发者只需关注编写应用代码，而无须关心配置细节。

大多数 Paas 服务提供命令行接口，开发者可以设置和查询配置，以及部署新版本的应用。

PaaS 服务及其合作伙伴提供附加功能集成到平台,如数据库托管,电子邮件服务、日志记录、计划和后台任务,账单和付款等。

#### Heroku

[Heroku](http://www.heroku.com/) [http://www.heroku.com/] 的 [Cedar stack](http://devcenter.heroku.com/articles/cedar) [http://devcenter.heroku.com/articles/cedar] 为 Python 2.7 应用提供一流的支持。

Heroku 允许你运行所想要的尽可能多的 Python web 应用，并且是免费的。Heroku 最恰当的描述是一个水平扩展的平台。一旦你在同一时间“扩展”你的应用，使之运行在超过 1 个 Dyno（抽象服务）上，他们便开始向你收费。

Heroku 维护了使用 Python 和 Heroku 交互的 [文章](https://devcenter.heroku.com/categories/python) [https://devcenter.heroku.com/categories/python]，同时也有 [手把手指导](https://devcenter.heroku.com/articles/getting-started-with-python) [https://devcenter.heroku.com/articles/getting-started-with-python] 来告诉你如何建立第一个应用。

#### DotCloud

[DotCloud](http://www.dotcloud.com/) [http://www.dotcloud.com/] 原生支持 WSGI 应用和后台/工作者任务在其平台上的运行。Web 应用使用 Python 2.6、 nginx 和 uWSGI，并允许高级用户自定义后两者的配置。

DotCloud 使用自定义命令行 API 客户端，能与管理在 git 仓库货其他任何版本控制系统中的应用工作。

DotCloud 有一个使用有限数据库的免费计划，但没有额外的服务（如缓存……）。

查阅 [基于 Python 的 DotCloud 文档](http://docs.dotcloud.com/services/python/) [http://docs.dotcloud.com/services/python/] 来获取更多信息，帮助开始。

#### Gondor

[Gondor](https://gondor.io/) [https://gondor.io/] 是一个部署 Django 和 Pinax 应用的专业的 PaaS（平台即服务）。Gondor 推荐 Django 1.6 版本，并支持任何 Python 2.7 版本上的 WSGI 应用。如果使用 `local_settings.py` 配置特定站点，Gondor 能够自动配置 Djnago 站点。

Gondor 有一个部署 [Django 工程](https://gondor.io/support/django/setup/) [https://gondor.io/support/django/setup/] 的指南。

## 模板

多数 WSGI 应用响应 HTTP 请求，从而服务于 HTML 或其他标记语言中的内容。关注点分离的概念建议我们使用模板，而不是直接由 Python 生成文本内容。模板引擎管理一系列的模板文件，其系统的层次性和包容性避免了不必要的重复。模板引擎负责渲染（产生）实际内容，用由应用生成的动态内容填充静态内容。

由于模板文件有时是由设计师或者前端开发者编写，处理不断增长的复杂度会变得困难。

一些通用的良好实践应用到了部分应用中，情景包括传递动态内容到模板引擎和模板自身中。

*   模板文件只应传递需要渲染的动态内容。避免传递附加的“以防万一”的内容： 需要时添加遗漏的变量比移除可能不用的变量要来的容易。
*   许多模板引擎允许在模板中编写复杂语句或者赋值，也有许多允许一些 Python 代码 在模板中等价编写。这种便利会导致复杂度不可控地增加，也使得查找 bug 变得更加 困难。
*   我们常常需要混合 JavaScript 模板和 HTML 模板。一种聪明的做法是孤立出 HTML 模板传递部分变量内容到 JavaScript 代码中的部分。

### Jinja2

[Jinja2](http://jinja.pocoo.org/) [http://jinja.pocoo.org/] 是一个和 Django 模板系统类似的模板引擎，并有一些额外的特性。它是一种基于文本的模板语言，因此能够用于生成任何标记内容。它允许定制过滤（filter）、标签、测试和全局内容，不像在 Django 框架中实现的模板系统，它还允许调用函数。Jinja2 基于 BSD 许可发行。

这里有一些在 Jinja2 中重要的 html 标签：

```py
{# 这是注释 #}

{# 下一个标签是输出变量： #}
{{title}}

{# 区块标签，能通过继承其他 html 代码来替换区块内容 #}
{% block head %}
<h1>This is the head!</h1>
{% endblock %}

{# 数组迭代输出 #}
{% for item in list %}
<li>{{ item }}</li>
{% endfor %} 
```

下面列举的内容是一个使用 Tornado 的站点的例子。Tornado 用起来并没那么复杂。

```py
# 导入 Jinja2
from jinja2 import Environment, FileSystemLoader

# 导入 Tornado
import tornado.ioloop
import tornado.web

# 载入模板文件 templates/site.html
TEMPLATE_FILE = "site.html"
templateLoader = FileSystemLoader( searchpath="templates/" )
templateEnv = Environment( loader=templateLoader )
template = templateEnv.get_template(TEMPLATE_FILE)

# 包含著名电影的 list
movie_list = [[1,"The Hitchhiker's Guide to the Galaxy"],[2,"Back to future"],[3,"Matrix"]]

# template.render() 返回包含渲染后 html 的字符串
html_output = template.render(list=movie_list,
                        title="Here is my favorite movie list")

# 主页的 handler
class MainHandler(tornado.web.RequestHandler):
    def get(self):
        # Returns rendered template string to the browser request
        self.write(html_output)

# 将 handler 赋给服务器 root (127.0.0.1:PORT/)
application = tornado.web.Application([
    (r"/", MainHandler),
])
PORT=8884
if __name__ == "__main__":
    # Setup the server
    application.listen(PORT)
    tornado.ioloop.IOLoop.instance().start() 
```

`base.html` 文件能够作为所有站点页面的基础，下面是实现的例子。

```py
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN">
<html lang="en">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" href="style.css" />
    <title>{{title}} - My Webpage</title>
</head>
<body>
<div id="content">
    {# 下一行内容将会在 site.html 模板中被添加。 #}
    {% block content %}{% endblock %}
</div>
<div id="footer">
    {% block footer %}
    &copy; Copyright 2013 by <a href="http://domain.invalid/">you</a>.
    {% endblock %}
</div>
</body> 
```

接下来是我们的 site 页面（`site.html`），它由 Python app 载入，并扩展了 `base.html`。下面的内容区块会自动填充到 `base.html` 页面的相关区块中。

```py
<!{% extends "base.html" %}
{% block content %}
 <p class="important">
    <div id="content">
        <h2>{{title}}</h2>
        <p>{{ list_title }}</p>
        <ul>
             {% for item in list %}
             <li>{{ item[0]}} :  {{ item[1]}}</li>
             {% endfor %}
        </ul>
    </div>
    </p>
{% endblock %} 
```

### Chameleon

[Chameleon](https://chameleon.readthedocs.org/) [https://chameleon.readthedocs.org/] 页面模板是使用 [模板属性语言（Template Attribute Language, TAL）](http://en.wikipedia.org/wiki/Template_Attribute_Language) [http://en.wikipedia.org/wiki/Template_Attribute_Language]、 [TAL 表达语法（TAL Expression Syntax,TALES）](http://chameleon.readthedocs.org/en/latest/reference.html#expressions-tales) [http://chameleon.readthedocs.org/en/latest/reference.html#expressions-tales] 和 [宏扩展 TAL（Macro Expansion TAL, Metal）](http://chameleon.readthedocs.org/en/latest/reference.html#macros-metal) [http://chameleon.readthedocs.org/en/latest/reference.html#macros-metal] 语法的 HTML/XML 模板引擎实现。

Chameleon 在 Python2.5 及以上版本（包括 3.x 和 pypy）都是可用的，并常被 [Pyramid Framework](http://trypyramid.com) [http://trypyramid.com] 使用。

页面模板是在文档结构中添加特定元素属性和文本标记。使用一系列简单语言概念，你能够控制文档流程、元素重复、文本替换和翻译。由于使用了基于属性的语法，未渲染的页面模板是合法的 HTML，它可以在浏览器中查看，甚至能够在 WYSIWYG 编辑器中编辑。这使得设计者和原型构建者之间在浏览器是中静态文件上的往复合作变得更加简单。

从下面的例子中能很快学到基本的 TAL 语言的用法：

```py
<html>
  <body>
  <h1>Hello, <span tal:replace="context.name">World</span>!</h1>
    <table>
      <tr tal:repeat="row 'apple', 'banana', 'pineapple'">
        <td tal:repeat="col 'juice', 'muffin', 'pie'">
           <span tal:replace="row.capitalize()" /> <span tal:replace="col" />
        </td>
      </tr>
    </table>
  </body>
</html> 
```

用作文本插入的 <span tal:replace=”expression” /> 形式非常常见。如果在未渲染的模板中并不要求严格的合法性，你可以取而代之地使用更加简洁和可读的语法，它使用 ${expression} 的形式，就像下面这样：

```py
<html>
  <body>
    <h1>Hello, ${world}!</h1>
    <table>
      <tr tal:repeat="row 'apple', 'banana', 'pineapple'">
        <td tal:repeat="col 'juice', 'muffin', 'pie'">
           ${row.capitalize()} ${col}
        </td>
      </tr>
    </table>
  </body>
</html> 
```

但是请记住，全部的 <span tal:replace=”expression”>Default Text</span> 语法也允许在未渲染的模板中有默认内容。

### Mako

[Mako](http://www.makotemplates.org/) [http://www.makotemplates.org/] 是一种模板语言，为了最大的性能，它编译为了 Python。它的语法和 API 借鉴了其他模板语言，如 Django 和 Jinja2 中最好的部分。它是包括 [Pylons 和 Pyramid](http://www.pylonsproject.org/) [http://www.pylonsproject.org/] 在内的 web 框架所使用的默认模板语言。

Mako 的一个模板例子如下：

```py
<%inherit file="base.html"/>
<%
    rows = [[v for v in range(0,10)] for row in range(0,10)]
%>
<table>
    % for row in rows:
        ${makerow(row)}
    % endfor
</table>

<%def name="makerow(row)">
    <tr>
    % for name in row:
        <td>${name}</td>\
    % endfor
    </tr>
</%def> 
```

要渲染一个非常基本的模板，你可以像下面这么做：

```py
from mako.template import Template
print(Template("hello ${data}!").render(data="world")) 
```

References

| [1] | [The mod_python project is now officially dead](http://blog.dscpl.com.au/2010/06/modpython-project-is-now-officially.html) [http://blog.dscpl.com.au/2010/06/modpython-project-is-now-officially.html] |

| [2] | [mod_wsgi vs mod_python](http://www.modpython.org/pipermail/mod_python/2007-July/024080.html) [http://www.modpython.org/pipermail/mod_python/2007-July/024080.html] |

| [[3]](#id14) | [Benchmark of Python WSGI Servers](http://nichol.as/benchmark-of-python-web-servers) [http://nichol.as/benchmark-of-python-web-servers] |

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.