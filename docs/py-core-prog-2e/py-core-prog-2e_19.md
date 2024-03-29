# 第十七章　网络客户端编程

本章主题

♦　引言

♦　文件传输

♦　文件传输协议（FTP）

♦　网络新闻、Usenet 和新闻组

♦　网络新闻传输协议（NNTP）

♦　电子邮件

♦　简单邮件传输协议（SMTP）

♦　邮局协议 3（POP3）

♦　相关模块

在之前的章节中，我们已经大致了解了那些使用套接字的低级别的网络通讯协议。这种网络互连是当今互联网中大部分客户端/服务器协议的核心。这些网络协议包括文件传输（FTP,SCP 等）、阅读 Usenet 新闻组（NNTP）、电子邮件发送（SMTP）、从服务器上下载电子邮件（POP3,IMAP）等。这些协议的工作方式与之前在套接字编程中介绍的客户端/服务器的例子很像。唯一的不同在于，我们已经使用过 TCP/IP 等低级别的协议，并基于此创建了新的，更具体的协议来实现我们刚刚描述的服务。

## 17.1　什么是因特网客户端

在着手研究这些协议之前，我们要先问一个问题：“因特网客户端到底是什么？”要回答这个问题，我们把因特网简化成一个数据交换中心，数据交换的参与者是一个服务提供者和一个服务的使用者。有的人把它称为“生产者—消费者”（虽然这个词一般只用在讲解操作系统相关信息时）。服务器就是生产者，它提供服务，一般只有一个服务器（进程或主机等）和多个消费者，就像我们之前看的客户端/服务器模型那样。虽然现在我们不再使用低级别的套接字来创建因特网客户端，但模型是完全相同的。

这里，我们将详细了解三个网际协议——FTP、NNTP 和 POP3，并写出它们的客户端程序。通过这些程序，你将会发现这些协议的 API 是多么的相似——由于保持接口的一致性有很大的好处，所以，这些相似性在设计之初就考虑到了——更重要的是，你还能学会如何写出这些协议与其他协议实用的客户端程序来。虽然我们只着重说了这三个协议在看完这些协议后，你就能有足够的信心和能力写出任何网际协议的客户端程序了。

## 17.2　文件传输

### 17.2.1　文件传输网际协议

因特网中最流行的事情就是文件的交换。文件交换无处不在。有很多协议可以供因特网上传输文件使用。最流行的有文件传输协议（File Transfer Protocol, FTP）、Unix-to-Unix 复制协议（Unix-to-Unix CopyProtocol, UUCP）和网页的超文本传输协议（Hypertext Transfer Protocol, HTTP）。另外，还有（Unix 下的）远程文件复制指令 rcp（以及更安全、更灵活的 scp 和 rsync）。

迄今为止，HTTP、FTP 和 scp/rsync 还是非常流行的。HTTP 主要用于网页文件的下载和访问 Web 服务上。它一般不要求用户输入登录的用户名密码就可以访问服务器上的文件和服务。HTTP 文件传输请求主要是用于获取网页（文件下载）。

相对的，scp 和 rsync 要求用户登录到服务器，否则不能上传或下载文件。至于 FTP，跟 scp/rsync 一样，可以上传或下载文件，还采用了 Unix 的多用户的概念，用户一定要输入有效的用户名和密码才能使用。不过，FTP 也允许匿名登录。接下来，我们先仔细看看 FTP。

### 17.2.2　文件传输协议（FTP）

文件传输协议由已故的 Jon Postel（作者这里使用的 Jon 是 Jonathon 的简写，下文中会使用全名）和 Joyce Reynolds 开发，记录在 RFC （Request for Comment）959 号文档中，于 1985 年 10 月发布，主要用于匿名下载公共文件。也可以用于在两台电脑之间传输文件，尤其是在使用 Unix 系统作为文件存储系统，使用其他机器来工作的情况。早在网络流行之前，FTP 就是在因特网上文件传输、软件和源代码下载的主要手段之一。

FTP 要求输入用户名和密码才能访问远程的 FTP 服务器，但它也允许没有账号的用户以匿名用户登录。不过，管理员要先设置 FTP 服务器允许匿名用户登录。这时，匿名用户的用户名是“匿名”（anonymous），密码一般是用户的电子邮件地址。与特定的用户拥有特定的账户不同，这有点像是把 FTP 公开出来让大家访问。匿名用户通过 FTP 协议可以使用的命令与一般的用户相比来说，限制更多。

图 17-1 展示了这个协议，其工作流程如下：

1.客户端连接远程的 FTP 服务器；

2.客户端输入用户名和密码（或“匿名”和电子邮件地址）；

3.客户端做各种文件传输和信息查询操作；

4.客户端登出远程 FTP 服务器，结束通讯。

![](img/00028.jpg)

> > 图 17-1　因特网上的 FTP 客户端和服务器。客户端和服务器使用指令和控制端口发送 FTP 协议，而数据通过数据端口传输。

当然，这只是一个大致流程。有时，由于网络两边电脑的崩溃或是网络的问题，会导致整个事务在完成之前被中断。一般在客户端超过 15 分钟（900 秒）不活动之后，连接就会被关闭。

在底层上，FTP 只使用 TCP（见前面网络编程相关章节）——它不使用 UDP。而且，FTP 是客户端/服务器编程中很“与众不同”的例子。客户端和服务器都使用两个套接字来通讯：一个是控制和命令端口（21 号端口），另一个是数据端口（有时是 20 号端口）。

我们说“有时”是因为 FTP 有两种模式：主动和被动。只有在主动模式服务器才使用数据端口。在服务器把 20 号端口设置为数据端口后，它“主动”连接客户端的数据端口。而被动模式中，服务器只是告诉客户端它的随机端口的号码，客户端必须主动建立数据连接。在这种模式下，你会看到，FTP 服务器在建立数据连接时是“被动”的。最后，现在已经有了一种扩展被动模式来支持第 6 版本的网际协议（IPv6）地址——见 RFC 2428。

Python 已经支持了包括 FTP 在内的大多数据网际协议。支持各个协议的客户端模块可以在 http://docs.python.org/lib/internet.html 找到。现在看看用 Python 创建一个因特网客户端程序有多简单。

### 17.2.3　Python 和 FTP

那么，我们怎么用 Python 写 FTP 客户端程序呢？其实，我们之前已经提到过一些了。现在还要再加上相应的 Python 模块导入和调用的操作。现在再来回顾一下流程：

1.连接到服务器；

2.登录；

3.发出服务请求（有可能有返回信息）；

4.退出。

在使用 Python 的 FTP 支持时，你所需要做的就是导入 ftplib 模块，并实例化一个 ftplib.FTP 类对象，所有的 FTP 操作（如登录，传输文件和登出等）都要使用这个对象来完成。下面是一段 Python 的伪代码：

![](img/00030.jpg)

在看真实的例子之前，我们要先熟悉一下 ftplib.FTP 类的方法，这些方法将在代码中用到。

### 17.2.4　ftplib.FTP 类方法

在表 17.1 中列出了最常用的方法，这个表并不全面——想查看所有的方法，请参阅模块源代码——但这里列出的方法组成了我们在 Python 中 FTP 客户端编程的“API”。

![](img/00032.jpg)

也就是说，你不一定要使用其他的方法，因为它们或者是辅助函数，或者是管理函数，或者是被 API 调用的。

在一般的 FTP 通讯中，要使用到的指令有 login()、cwd()、dir()、pwd()、stor*()、retr*()和 quit()。有一些没有列出的 FTP 对象方法也是很有用的。请参阅 Python 的文档以得到更多关于 FTP 对象的信息：

![](img/00034.jpg)

### 17.2.5　交互式 FTP 示例

在 Python 中使用 FTP 非常的简单，你甚至可以不用写脚本，直接在交互式解释器中实时地看到交互与输出。下面这个例子是在几年前，python.org 还支持 ftp 服务的时候做的。

![](img/00036.jpg)

![](img/00038.jpg)

### 17.2.6　客户端 FTP 程序举例

之前我们说过，你可以不写脚本，在交互环境中使用 FTP。不过，下面我们还是要写一段脚本，假设你要从 Mozilla 的网站下载最新的 Bugzilla 的代码。例 17.1 就是用来完成这个工作的。我们在试着写一个应用程序，不过，你也可以交互式地运行这段代码。我们的程序使用 FTP 库来下载文件，也做了一些错误检测。

不过，程序并不完全自动。你要自己决定什么时候要去下载。如果你在使用类 Unix 系统，你可以设定一个“cron”任务来自动下载。另一个问题是，如果文件的文件名或目录名改了的话，程序就不能正常工作了。

这个程序用于下载网站中最新版本的文件。你可以修改这个程序让它下载你喜欢的程序。

![](img/00040.jpg)

![](img/00042.jpg)

如果运行脚本时没有出错，则会得到如下输出：

![](img/00044.jpg)

逐行解释

1 ~ 9 行

代码前几行导入要用的模块和设置一些常量。

11 ~ 44 行

main()函数分为以下几步：创建一个 FTP 对象，尝试连接到 FTP 服务器（12〜17 行）然后返回。在有任何错误发生的时候退出。我们尝试用“匿名”登录，如果不行就结束（19〜25 行）。下一步就是转到发布目录（27〜33 行），最后，下载文件（35~44 行）。

在 35〜36 行，我们传了一个回调函数给 retrbinary()，它在每接收到一块二进制数据的时候都会被调用。这个函数就是我们创建的本地文件对应文件对象的 write 方法。在传输结束的时候，Python 解释器会自动关闭这个文件对象，而不会丢失数据。虽然这样方便，但最好还是不要这样做，作为一个程序员，要尽量做到在资源不再被使用的时候就直接释放，而不是依赖其他代码来做释放操作。在这里，我们应该把文件对象保存到一个变量中，如变量 loc，然后把 loc.write 传给 ftp.retrbinary()方法。

在代码中，如果由于某些原因我们无法保存这个文件，那要把存在的空文件给删掉，以防搞乱文件系统（40 行）。最后，我们使用了 try-except-else 语句（35〜42 行），而不是写两遍关闭 FTP 连接然后返回的代码。

46 ~ 47 行

这是运行独立脚本的惯用方法。

### 17.2.7　FTP 的其他方面

Python 同时支持主动和被动模式。注意，在 Python2.0 及以前版本中，被动模式支持默认是关闭的，在 Python2.1 及以后版本中，默认是打开的。

以下是一些典型的 FTP 客户端类型：

•　命令行客户端程序：你可以使用一些 FTP 文件传输工具如/bin/ftp 或 NcFTP，它们允许用户在命令行交互式的参与到 FTP 通讯中来。

•　GUI 客户端程序：与命令行客户端程序相似，只是它是一个 GUI 程序。如 WsFTP 和 Fetch 等。

•　网页浏览器：在使用 HTTP 之外，大多数网页浏览器（也是一个客户端）可以进行 FTP 通讯。URL/URI 的第一部分就用来表示所使用的协议，如“http://blahblah.”这就告诉浏览器要使用 HTTP 作为与给定网站进行通讯的协议。修改协议部分，就可以发使用 FTP 的请求，如“ftp://blahblah.”，这跟使用 HTTP 的网页的 URL 很像（当然，“ftp://”后面的“blahblah”可以展开为“host/path？attributes”）。如果要登录，用户可以把登录信息（以明文方式）放在 URL 里，如：

“ftp://user:passwd@host/path?attrl=vall&attr2=val2…”。

•　定制程序：你自己写的用于 FTP 文件传输的程序。由于程序用于特殊目的，一般这种程序都不允许用户与服务器接触。

这 4 种客户端类型都可以用 Python 来写。上面，我们用 ftplib 来创建了一个自己的定制程序，你也可以自己做一个命令行的应用程序。在命令行的基础上，你可以使用一些界面工具包，如 Tk、wxWidgets、GTK+、Qt、MFC，甚至 Swing（要导入相应的 Python[或 Jython]的接口模块）来创建一个完整的 GUI 程序。最后，你可以使用 Python 的 urllib 模块来解析 FTP 的 URL 并进行 FTP 传输。在 urllib 的内部也导入并使用了 ftplib,urllib 也是 ftplib 的客户端。

FTP 不仅可以用在下载应用程序上，还可以用在系统之间文件的转移上。比如，如果你是一个工程师或是系统管理员，你需要传输文件。在跨网络的时候，很明显可以使用 scp 或 rsync 命令，或者把文件放到一个外部能访问的服务器上。不过，在一个安全网络的内部机器之间移动大量的日志或数据库文件，这种方法的开销就太大了，要注意安全性、加密、压缩、解压缩等。如果你想要做的只是写一个 FTP 程序来帮助你在下班后自动移动文件，那用 Python 是一个非常好的主意。

从 FTP 协议定义/规范（RFC959）中，你可以得到更多关于 FTP 的信息：ftp://ftp.isi.edu/in-notes/rfc959.txt 以及网页 http://www.networksorcery.com/enp/protocol/ftp.htm。其他相关的 RFC 有 2228、2389、2428、2577、2640 和 4217。想了解更多 Python 对 FTP 的支持，可以访问网址 http://python.org/docs/current/lib/module-ftplib.html。

## 17.3　网络新闻

### 17.3.1　Usenet 与新闻组

Usenet 新闻系统是一个全球存档的“电子公告板”。各种主题的新闻组一应俱全，从诗歌到政治，从自然语言学到计算机语言，从软件到硬件，从种植到烹饪以及招工、应聘、音乐、魔术、分手、求爱等。新闻组可以是面向全球泛泛而谈，也可以是只面向某个地理区域。

整个系统是一个由大量计算机组成的一个庞大的全球网络，计算机之间共享 Usenet 上的帖子。如果某一个用户发了一个帖子到本地的 Usenet 计算机上，这个帖子会被传播到其他相连的计算机上，并再由这些计算机传到与它们相连的计算机上，直到这个帖子传播到了全世界，每个人都收到这个帖子为止。

每个系统都有一个它已经“订阅”的新闻组的列表，它只接收它感兴趣的新闻组里的帖子——而不是服务器上所有新闻组的帖子。Usenet 新闻组服务内容取决于服务提供者，很多都是可供公众访问的，也有一些只允许特定的用户使用，例如付费用户、特定大学的学生等。如果 Usenet 系统管理员设置了的话，有可能会要求输入用户名和密码。管理员也可以设置是否只允许上传或只允许下载。

### 17.3.2　网络新闻传输协议（NNTP）

供用户在新闻组中下载或发表帖子的方法叫网络新闻传输协议（NNTP）。BrainKantor（加利福尼亚大学圣地亚哥分校）和 Phil Lapsley（加利福尼亚大学伯克利分校）创建并记录在 RFC 977 中，于 1986 年 2 月公布。其后的更新记录在 RFC 2980，于 2000 年 10 月公布。

作为客户端/服务器架构的另一个例子，NNTP 与 FTP 的操作方式很像，而且简单得多。FTP 需要不同的端口来做登录、数据传输和控制，而 NNTP 只使用一个标准端口 119 来做通讯。你给服务器一个请求，它做相应的反馈，见图 17-2。

![](img/00046.jpg)

> > 图 17-2　因特网上的 NNTP 客户端和服务器。客户端主要阅读新闻，有时也发帖子。文章会在服务器之间做同步

### 17.3.3　Python 和 NNTP

由于之前已经有了 Python 和 FTP 的经验，你也许可以猜到，一定有一个库 nntplib 和一个类 nntplib.NNTP，你要实例化这个类。你猜对了。和 FTP 一样，我们所要做的就是导入那个 Python 模块，然后调用相应的方法。我们先大致看一下这个协议：

1.连接到服务器；

2.登录（如果需要的话）；

3.发送请求；

4.退出。

是不是有点熟悉？是的，这几乎就是完全复制了 FTP 协议。唯一的不同就是根据 NNTP 服务器的配置不一样，登录这一步是可选的。

下面是一段 Python 的伪代码：

![](img/00048.jpg)

一般来说，在你登录完成后，你要调用 group()方法来选择一个感兴趣的新闻组。方法返回服务器的返回信息、文章的数量、第一个和最后一个文章的 ID 和组的名字。在有了这些信息后，你会做一些其他的操作，如从头到尾看文章、下载整个帖子（文章的标题和内容）或发表一篇文章等。

在看真实的例子之前，我们要先介绍一下 nntplib.NNTP 类的一些常用的方法。

### 17.3.4　nntplib.NNTP 类方法

跟前一节列出 ftplib.FTP 类的方法时一样，我们不会列出 nntplib.NNTP 的所有方法，只列出你创建 NNTP 客户端程序时可能用得着的方法。

跟上一节的 FTP 对象表一样，还有一些 NNTP 对象的方法没有提及。为了避免混乱，我们只列出了你可能用得到的。其余的，我们再次建议你参考 Python 手册。

### 17.3.5　交互式 NNTP 举例

接下来，是一个如何使用 Python 中 NNTP 库的交互式的例子。它看上去跟交互式的 FTP 的例子差不多（出于保密的原因，电子邮件地址都做了修改）。

在调用表 17.2 中所列的 group()方法连接到一个组的时候，你会得到一个 5 元组。

![](img/00050.jpg)

![](img/00052.jpg)

![](img/00054.jpg)

### 17.3.6　客户端程序 NNTP 举例

在 NNTP 客户端例子中，我们来点更复杂的。在之前的 FTP 客户端例子中，我们是下载最新的文件，这一次，我们要下载 Python 语言新闻组 com.lang.python 里的最后一篇文章。下载完成后，我们会显示文章的前 20 行，而且是前 20 行有意义的内容。有意义的内容是指那些不是被引用的文本（引用以“>”或 “|”开头），也不是像这样的文本 “ In article <…>,soAndSo@some.domain wrote：”。

最后，我们要智能地处理空行。在文章中出现了一行空行，那我们就显示一行空行，但如果有多行连续的空行，那只显示一行空行。只有有数据的行才算在“前 20 行”之中。所以，最多可能显示 39 行输出，20 行实际数据间隔了 19 行空行。

如果脚本的运行正常的话，我们可能会看到这样的输出：

![](img/00056.jpg)

这个脚本下载并显示 Python 新闻组 comp.lang.python 最后一篇文章的前 20 个“有意义的”行。

![](img/00058.jpg)

![](img/00614.jpg)

![](img/00062.jpg)

这个输出显示了新闻组帖子的原始内容，如下：

![](img/00064.jpg)

![](img/00066.jpg)

当然，由于新文章不断出现，输出经常会不一样。只要你的服务器里一有文章更新，输出就会不一样了。

逐行解释

1 ~ 9 行

程序开始是一些导入语句和常量定义，跟 FTP 客户端差不多。

11 ~ 40 行

在第一部分，我们尝试连接到 NNTP 服务器，如果失败就退出（13〜24 行）。第 15 行故意注释掉了，如果需要输入用户名和密码进行认证的话，可以打开这一行，并修改第 14 行。后面是尝试读取指定的新闻组。同样，如果新闻组不存在，服务器没有保存这个新闻组，或是需要认证的话，退出（26〜40 行）。

42 ~ 55 行

下面这一部分，我们读一些头信息，并显示出来（42〜51 行）。最有用处的头信息包括作者、主题和日期。这些数据会被读取并显示给用户。在每一次调用 xhdr()方法时，都要给定想要提取信息头的文章的范围。我们只想取一条信息，所以范围就是“X-X”，其中，X 是最后一条信息的号码。

xhdr()方法返回一个 2 元组，包含了服务器的返回信息（rsp）和我们指定范围的信息头的列表。由于我们只指定了一个消息（最后一个），我们只取列表的第一个元素（hdr[0]）。数据元素是一个 2 元组，包含文章号和数据字符串。由于我们已经知道了文章号（我们在请求中给出了），我们只关心第二个元素，数据字符串（hdr[0][1]）。

最后一部分是下载文章的内容（53〜55 行）。先调用 body()方法，然后显示前 20 个有意义的行，最后登出服务器，完成执行。

57 ~ 80 行

主要的处理任务由 displayFirst200 函数完成（57~80 行）。它接受文章的所有行作为参数，并做一些预处理，如把计数器清 0，创建一个生成器表达式对文章内容的所有行做一些处理，然后“假装”我们刚碰到并显示了一行空行（59〜61 行，稍后细说）。由于前导空格可能是 Python 代码的一部分，所以在我们去掉字符串中的空格的时候，只删除字符串右边的空格（rstrip()）。

我们要做的是，我们不要显示引用的文本和引用文本指示行。这就是 65〜71 行（也包含 64 行）的那个大 if 语句所要做的事。如果这一行不是空行的时候，才做这个检查（63 行）。检查的时候，会把字符串转成小写，这样就能做到比较的时候大小写无关（64 行）。

如果一行以“>”或“|”开头，说明这一般是一个引用。不过，我们认为“>>>”是一个例外，因为这有可能是交互命令行的提示，虽然这样可能有问题，因为它也可能是一段被引用了三次的消息（1 段文本到第 4 个回复的帖子时被引用了 3 次）却被显示了。

现在来处理空行。我们想让程序聪明一些，它应该能显示文章中的空行，但对空行的处理要做到智能。如果有多个连续的空行，则只显示第一个，这样用户不用看那么多行信息，导致有用的信息却在屏幕之外。我们也不能把空行计算到 20 行有意义的行之中。所有这些要求都在 72〜78 行内实现。

72 行的 if 语句表示只有在上一行不为空，或者上一行为空但当前行不为空的时候才显示。也就是说，如果显示了当前行的话，就说明要么当前行不为空，要么当前行为空但上一行不为空。这是另一个比较有技巧的地方：如果我们碰到了一个非空行，计数器加 1，并设置 lastBlank 标志为 False，以表示这一行非空（74〜76 行）。否则，表示我们碰到了空行，把标志设为 True。

现在回到第 61 行，我们设 lastBlank 标志为 True，是因为，如果内容的第一行实际数据（不是前导数据或是引用数据）是一个空行，我们不会显示它。因为我们想要看第一行实际数据！

最后，如果我们已经显示了 20 行非空行，则退出，放弃其余的行（79〜80 行）。否则，我们应该已经遍历了所有行，循环也正常结束了。

### 17.3.7　NNTP 的其他方面

从 NNTP 协议定义/规范（RFC 977）中，你可以得到更多关于 NNTP 的信息：ftp://ftp.isi. edu/in-notes/rfc977.txt 以及网页 http://www.networksorcery.com/enp/protocol/nntp.htm。其他相关的 RFC 有 1036、2980。想了解更多 Python 对 NNTP 的支持，可以从这里开始：http://python.org/docs/current/lib/module-nntplib.html

## 17.4　电子邮件

电子邮件既古老又现代。对于我们这些从很早就开始用因特网的人来说，电子邮件看上去是如此的“古老”，尤其是相对于基于网页的在线聊天，即时通讯（IM）和数字电话即 VOIP （Voice Over Internet Protocol）等更新更快的通讯方式来说更是如此。下一节中，我们将从宏观上介绍一下电子邮件是如何工作的。如果你已经对此相当了解，只想看如何用 Python 做电子邮件相关的开发，你可以跳到后续章节。

在看电子邮件的底层的结构之前，你有没有问过自己，电子邮件的确切定义到底是什么？根据 RFC2822，“消息由头域（合起来叫消息头）以及后面可选的消息体组成”。对于一般用户来说，一说起电子邮件就会让我们想到它的内容，不管它是一封真的邮件还是一封不请自来的商业广告（即 spam，垃圾邮件），都应该有内容。不过，RFC 规定，邮件体是可选的，只有邮件头是必要的。这一点要特别注意。

### 17.4.1　电子邮件系统组件和协议

不管你是怎么样想的，电子邮件实际上在现代的因特网出现之前就已经出现了。它一开始用于大型机的用户之间简单的交换信息。注意，由于他们都在使用同一台电脑，所以，这里甚至都没有涉及到网络。后来，当网络成为现实的时候，用户就可以在不同的主机之间交换信息。当然，由于用户使用着不同的电脑，电脑之间使用着不同的协议，信息交换成了一个很复杂的概念。直到 20 世纪 80 年代，因特网上用电子邮件进行信息交换才有了一个事实上的统一的标准。

在深入细节之前，我们先问问自己，电子邮件是怎么工作的？一条消息是如何从发件人那通过浩瀚的因特网，到达收件人的？简单点来说，有一台发送电脑（发件人的消息从这里发送出去），和一台目的电脑（收件人的信件服务器）。最好的解决方案是发送电脑知道如何连接到接收电脑，这样一来，它就可以直接把消息发送过去。不过，实际上一般并不这么顺利。

发送电脑要查询到某一台中间主机，这台中间主机能到达最后的收件主机。然后这台中间主机要找一台离目的主机更近一些的主机。所以，在发送主机和目的主机之间，可能会有多台叫做“跳板”的主机。如果你仔细看看你收到的电子邮件的邮件头，你会看到一个“passport”标记，其中记录了邮件寄给你这一路上都到过了哪些地方。

为了让描述清楚一些，让我们先看看电子邮件系统的各个组件。最主要的组件是消息传输代理（messagetransport agent, MTA）。这是一个在邮件交换主机上运行的一个服务器程序，它负责邮件的路由、队列和发送工作。它们就是邮件从源主机到目的主机所要经过的跳板。所以也被称为是“信息传输”的“代理”。

要让所有这些工作起来，MTA 要知道两件事情：1）如何找到消息应该去的下一台 MTA;2）如何与另一台 MTA 通讯。第一件事由域名服务（domain name service,DNS）来查找目的域名的 MX（邮件交换，Mail eXchange）来完成。这对于最后的收件人是不必要的，但对其他的跳板来说，则是必要的。对于第二件事，MTA 怎么把消息转给其他的 MTA 呢？

### 17.4.2　发送电子邮件

要能发送电子邮件，你的邮件客户端一定要连接到一个 MTA，它们靠某种协议进行通讯。MTA 之间通讯所使用的协议叫消息传输系统（MTS）。只有两个 MTA 都使用这个协议时才能进行通讯。在本节开始时就说过，由于以前存在很多不同的计算机系统，每个系统都使用不同的网络软件，这种通讯很危险，具有不可预知性。更复杂的是，有的电脑使用互连的网络，而有的电脑使用调制解调器拨号，消息的发送时间也是不可预知的。事实上，笔者曾经有一封邮件在发送 9 个月后才收到！互连网的速度怎么会这么慢？出于对这些复杂度的考虑，现代电子邮件的基础之一，简单邮件传输协议（Simple Mail Transfer Protocol，SMTP）于 1982 年出现了。

SMTP

SMTP 由已故的乔纳森•波斯特（Jonathan Postel，加利福尼亚大学信息学院）创建，记录在 RFC 821 中，于 1982 年 8 月公布。其后的修改记录在 RFC 2821 中，于 2001 年 4 月公布。一些已经实现了 SMTP 的著名 MTA 包括：

开源 MTA

•　Sendmail

•　Postfix

•　Exim

•　qmail（免费发布，但不开源）

商业 MTA

•　Microsoft Exchange

•　Lotus Notes Domino Mail Server

注意，虽然它们都实现了 RFC 2821 中定义的最小化 SMTP 协议，它们中的大多数，尤其是一些商业 MTA，都在服务器中加入了协议定义之外的特有的功能。

SMTP 是在因特网上 MTA 之间用于消息交换的最常用的 MTS。它被 MTA 用来把电子邮件从一台主机传送到另一台主机。在你发电子邮件的时候，你必须要连接到一个外部的 SMTP 服务器，这时，你的邮件程序是一个 SMTP 客户端。你的 SMTP 服务器也因此成为了你的消息的第一个跳板。

### 17.4.3　Python 和 SMTP

是的，也存在一个 smtplib 模块和一个 smtplib.SMTP 类要实例化。再来看看这个已经熟悉的过程吧：

1.连接到服务器；

2.登录（如果需要的话）；

3.发出服务请求；

4.退出。

像 NNTP 一样，登录是可选的，只有在服务器打开了 SMTP 认证（SMTP-AUTH）时才要登录。SMTP-AUTH 在 RFC 2554 中定义。还是跟 NNTP 一样，SMTP 通讯时，只要一个端口 25。

下面是一些 Python 的伪代码：

![](img/00068.jpg)

在看真实的例子之前，我们要先介绍一下 smtplib.SMTP 类的一些常用的方法。

### 17.4.4　smtplib.SMTP 类方法

跟之前一样，我们会列出 smtplib.SMTP 类的方法，但不会列出所有的方法，只列出你创建 SMTP 客户端程序所需要的方法。对大多数电子邮件发送程序来说，只有两个方法是必须的，即 sendmail()和 quit()。

sendmail()的所有参数都要遵循 RFC 2822，即电子邮件地址必须要有正确的格式，消息体要有正确的前导头，前导头后面是两个回车和换行（\r\n）对。

注意，实际的消息体不是必要的。根据 RFC 2822, “唯一要求的头信息只有发送日期和发送地址”，即“Date:”和“From:”:（MAIL FROM, RCPT TO,DATA）。

还有一些方法没有被提到，不过，一般来说，它们不是发送电子邮件所必须的。请参考 Python 文档以获取 SMTP 对象的所有方法的信息。

![](img/00070.jpg)

### 17.4.5　交互式 SMTP 示例

同样地，我们先给一个交互式的例子：

![](img/00072.jpg)

![](img/00074.jpg)

### 17.4.6　SMTP 的其他方面

从 SMTP 协议定义/规范（RFC 2821）中，你可以得到更多关于 SMTP 的信息：ftp://ftp.isi.edu/in-notes/rfc2821.txt 以及网页 http://www.networksorcery.com/enp/protocol/smtp.htm。想了解更多 Python 对 SMTP 的支持，可以从这里开始：http://python.org/docs/current/lib/module-smtplib.html。

我们还没有讨论的电子邮件的一个很重要的方面是如何正确地设定因特网地址的格式和电子邮件消息。这些信息详细记录在因特网信息格式 RFC 2822 中，可以在 ftp://ftp.isi.edu/in-notes/rfc2822\. txt 下载。

### 17.4.7　接收电子邮件

在以前，在因特网上用电子邮件通讯的只有大学学生、研究人员和工商企业的雇员。桌面电脑还都是类 Unix 操作系统。家庭用户只是拨号到 PC 上，并不真的使用电子邮件。在 20 世纪 90 年代中期因特网大爆炸的时候，电子邮件进入了千家万户。

对于家族用户来说，在家里放一个工作站来运行 SMTP 是不现实的。必须要设计一种新的系统，能够周期性地把信件下载到本地计算机，以供离线时使用。这样的系统就要有一套新的协议和新的应用程序来与邮件服务器通讯。

在家用电脑中运行的应用程序叫邮件用户代理（mail user agent, MUA）。MUA 从服务器上下载邮件，在这个过程中可能会自动删除它们（也可能不删除，留在服务器上，让用户手动删除）。不过，MUA 也必须要能发送邮件。也就是说，在发送邮件的时候，它要能直接与 MTA 用 SMTP 进行通讯。在前面讲 SMTP 的章节中，我们已经看过这种客户端了。那下载邮件的呢？

### 17.4.8　POP 和 IMAP

用于下载邮件的第一个协议叫邮局协议，记录在 RFC 918 中，于 1984 年 10 月公布。“邮局协议（POP）的目的是让用户的工作站可以访问邮箱服务器里的邮件。邮件要能从工作站通过简单邮件传输协议（SMTP）发送到邮件服务器”。POP 协议的最新版本是第 3 版，也叫 POP3。POP3 在 RFC 1939 中定义，至今为止仍在被广泛地使用，也是我们下面的客户端例子的主要内容。

在 POP 之后几年，出现了另一个协议，叫交互式邮件访问协议（Interactive Mail Access Protocol， IMAP）。第一个版本是实验性的，直到第 2 版时，其 RFC 1064 才在 1988 年被公布。现在被使用的 IMAP 版本是 IMAP4revl，它也被广泛地使用。事实上，当今世界上占有邮件服务器大多数市场的 Microsoft Exchange 就使用 IMAP 作为其下载机制。IMAP4revl 协议定义在 RFC 3501，于 2003 年 3 月公布。IMAP 的目的是要提供一个更全面的解决方案。不过，它比 POP 更为复杂。对 IMAP 的进一步讨论超出了本章剩余部分的范围。我们建议感兴趣的用户参考上述 RFC 文档。图 17-3 展示的复杂系统就是我们所认为的简单的电子邮件。

![](img/00076.jpg)

> > 图 17-3　因特网上的电子邮件发件人和收件人。客户端通过他们的 MUA 和相应的 MTA 进行通讯，来下载和发送邮件。电子邮件从一个 MTA “跳”到另一个 MTA，直到到达目的地为止

### 17.4.9　Python 和 POP3

毫不奇怪，我们要做的是导入 poplib，实例化 poplib.POP3 类。标准的做法如下：

1.连接到服务器；

2.登录；

3.发出服务请求；

4.退出。

Python 的伪代码如下：

![](img/00078.jpg)

在看真实的例子之前，我们要先看一个交互式的例子以及介绍一下 poplib.POP3 类的一些基本的方法。

### 17.4.10　交互式 POP3 举例

下面是使用 Python poplib 模块的交互式的例子：

![](img/00080.jpg)

![](img/00082.jpg)

### 17.4.11　poplib.POP3 类方法

POP3 类有无数的方法来帮助你下载和离线管理你的邮箱。最常用的列在表 17.4 中。

![](img/00084.jpg)

在登录时，user()方法不仅向服务器发送了用户名，也要等待服务器正在等待用户密码的返回信息。如果 pass_()方法认证失败，会引发一个 poplib. error_proto 的异常。如果成功，会得到一个以“+”号开头的返回信息，如“+OK ready”，然后服务器上的该邮箱就被锁定了，直到调用了 quit()方法为止。

调用 list()方法时，msg_list 的格式为[‘msgnum msgsiz’,…]，其中 msgnum 和 msgsiz 分别是每个消息的编号和消息的大小。

还有一些方法未被列出，想要了解更多信息，请参考 Python 手册里 poplib 的文档。

### 17.4.12　客户端程序 SMTP 和 POP3 举例

下面的例子演示了如何使用 SMTP 和 POP3 来创建一个既能接收和下载电子邮件也能上传和发送电子邮件的客户端。我们将要先用 SMTP 发一封电子邮件给自己（或其他测试账户），等待一段时间——我们随便选了一个时间，10 秒钟——然后使用 POP3 下载这封电子邮件，下载下来的内容跟发送的内容应该是完全一样的。如果程序悄无声息地结束，没有输出也没有异常，那就说明我们的操作都成功了。

这个脚本（通过 SMTP 邮件服务器）发送一封测试电子邮件到目的地址，并马上（通过 POP）把电子邮件从服务器上收回来。要让程序能正常工作，你需要修改服务器的名字和电子邮件的地址。

![](img/00086.jpg)

![](img/00088.jpg)

逐行解释

1 ~ 8 行

跟本章前面的例子一样，程序一开始是一些导入语句和常量的定义。常量分别是发送邮件和接收邮件的服务器。

10 ~ 14 行

这几行是消息内容的准备工作。这里，我们放了三行消息头然后是消息体。From 和 To 两个头分别表示消息的发件人和收件人。14 行把消息头和消息体放在一起组成一个可以发送的消息，按 RFC 2822 的要求，这两部分用空行隔开。

16 ~ 21 行

我们连接到发送（SMTP）服务器来发送我们的消息。这里还有一对 From 和 To 的地址，这些地址是“真实”的电子邮件地址，或者说是信封格式（envlelope）的地址。收件人参数应该是一个可迭代的对象，如果传的是一个字符串，就会被转成一个只有一个元素的列表。不请自来的垃圾邮件中，消息头和信封头总是不一致的。

sendmail()的第三个参数是电子邮件信息本身。这个函数返回之后，我们就登出 SMTP 服务器，并判断是否有错误发生过。我们要等待一段时间，等待服务器完成消息的发送与接收。

23 ~ 30 行

程序的最后一部分是下载刚刚发送的消息，并断言发送的和接收的消息是完全一样的。先给出用户名和密码，连接到 POP3 服务器，在登录成功后，调用 stat()方法得到有效的消息的列表。我们先选第一条消息（[0]），然后调用 retr()下载这个消息。

我们用空行来分隔头和信息，去掉头部分，比较原始信息体和收到的信息体。如果它们相同，什么都不显示，程序正常退出；否则，会出现一个断言失败的错误。

由于错误的类型太多，我们在这个脚本里不做错误检查，这样的好处是你可以直接看到出现了什么错误。在本章末尾有一个习题就是做错误检查的。

现在，你对如何发送和接收电子邮件有了一个很全面的了解。如果你想深入了解这一方面的编程，请参阅下一章里介绍的电子邮件相关的模块，它们在程序开发方面有相当大的帮助。

## 17.5　相关模块

Python 最好的一个方面就是它在标准库中提供了相当的全面的网络支持，尤其在网际协议和客户端开发方面的支持更为全面。下面列出了一些相关模块，首先是电子邮件相关的，随后是一般用途的网际协议相关的。

### 17.5.1　电子邮件

Python 自带了很多电子邮件模块和包可以帮助你创建应用程序。表 17.5 中列出了一部分。

![](img/00090.jpg)

### 17.5.2　其他网络协议

![](img/00092.jpg)

## 17.6　练习

FTP

> > 17-1.简单 FTP 客户端。参考本章的 FTP 例子，写一个小的 FTP 客户端程序，能够去你喜欢的网站下载你使用的软件的最新版本。这个脚本应该每几个月就运行一次，以确保你在用的软件是“最新和最好的”。你应该把 FTP 地址，登录信息放在一个表里，省得每次都要修改。

> > 17-2.简单 FTP 客户端和模式匹配。在上一个练习的基础上创建一个新的 FTP 客户端程序。它可以上传和下载指定模式的文件。比方说，如果想把一些 Python 的文件和 PDF 文件从一台电脑传到另一台电脑上，那用户可以输入“*.py”或“doc*.pdf”，程序会只传这些文件名匹配的文件。

> > 17-3.智能 FTP 命令行客户端程序。创建一个跟 Unix 下/bin/ftp 类似的命令行下的 FTP 程序，不过，这个 FTP 客户端要更好一些，能提供更有用的功能。你可以看看 http://ncftp.com 的 ncFTP 作为样板。它有以下功能：历史记录、书签（可以保存 FTP 地址和登录信息）、下载进度显示等。你可以使用 readline 来记录历史命令，用 curses 来控制屏幕。

> > 17-4.FTP 和多线程。创建一个能使用 Python 的线程库下载文件的 FTP 客户端程序。你可以通过修改上一个练习的程序或者重写一个简单的客户端来下载文件。你可以在命令行参数里指定要下载的文件，也可以做一个 GUI，在界面中让用户选择要下载的文件。附加题：要能支持模式，如*.exe。要使用不同的线程来下载每个文件。

> > 17-5.FTP 和 GUI。在你上面写的 FTP 客户端程序中加入 GUI，让你的程序成为一个完整的 FTP 应用程序。你可以使用 Python 的任何 GUI 工具包。

> > 17-6.子类化。从 ftplib. FTP 派生出一个类 FTP2，在这个类中，你不用像之前那 4 个 retr*()和 stor*()方法中那样要给定“STOR filename”或“RETR filename”这样的命令，只要传文件名就好了。你可以重写已有的方法也可以在方法后加一个 2，如 retrlines2()。Python 发布包中有一个 Tools/scripts/ftpmirror.py 脚本，它使用 ftplib 模块，可以对整个 FTP 站点或 FTP 站点的一部分做镜像。它可以作为 ftplib 模块应用的扩展例子来使用。解答下面 5 个问题时，可以参考这个脚本。你可以直接使用 ftpmirror.py 里的代码，也可以以这个脚本为样板，自己重新写一个。

> > 17-7.递归。ftpmirror.py 脚本递归的复制一个远程的目录。写一个与 ftpmirror.py 相似的脚本，它的默认行为是不递归的。只有在传入了“-r”参数的时候，才递归的把文件复制到本地目录。

> > 17-8.模式匹配。ftpmirror.py 脚本支持”-s”参数让用户指定能匹配模式的文件不下载，如“*.exe”。重新写一个简单的 FTP 客户端程序或修改之前的程序，实现让用户指定通配符程序只下载能匹配模式的文件。可以在你之前练习的答案基础上实现。

> > 17-9.递归和模式匹配。写一个 FTP 客户端程序，把上面两个练习的脚本集成在一起。

> > 17-10.递归和 ZIP 文件。这个练习与上面的第一个递归练习有些相似，只是不再直接把文件下载到本地文件系统，而是文件下载后压缩到一个 ZIP（或 TGZ，或 BZ2）文件中。同样，你可以在之前脚本的基础上改，也可以重写一个。使用“-z”参数让用户可以自动地备份一个 FTP 站点。

> > 17-11.集成。实现一个最终的，全功能的 FTP 应用程序，包含上面几个练习的所有功能。即，支持“-r” “-s”和“-z”参数。

> > > > NNTP

> > 17-12.NNTP 介绍。修改例 17.2 （getLatestNNTP.py），让它显示第一封（而不是最后一封）有效文章的有意义的内容。

> > 17-13.代码改进。修正 getLatestNNTP.py 的会输出 3 次引用问题，这是因为我们想输出 Python 交互解释的内容，而不是被 3 次引用的文本。用检查“>>>”后的代码是否为合法 Python 代码的方式来解决这个问题。如果合法，那就显示这一行数据，如果不合法，认为是引用文本，不显示。附加题：你的解决方案再解决这样一个小问题：我们没有去掉前导的空格，因为它可能是 Python 代码的缩进。如果真的是代码的缩进，就显示它，否则，认为它是一般的文本，先对字符串用 Istrip()方法处理后再显示。

> > 17-14.查找文章。写一个 NNTP 客户端程序，让用户能选择并登录感兴趣的新闻组。在登录成功后，提示用户输入一些关键字，使用这些关键字来查找文章的标题。把符合要求的文章列出来显示给用户。用户可以在列表中选择某一篇文章进行阅读，这时要能显示选定文章的内容。程序还要有简单的导航功能，如分页等。如果没有给出搜索关键字，则显示所有的文章。

> > 17-15.搜索内容。修改上一题你的脚本，让脚本同时搜索主题和文章内容。允许关键字的“与”（AND）和“或”（OR）的操作。也要允许指定在标题和文章内容的“与”（AND）和“或”（OR）即，关键字要只在标题里出现，只在内容里出现或两者里面都要出现。

> > 17-16.线索化的新闻阅读工具。把不同的回帖组织到一个“文章线索”中。也就是说，把相关的文章放在一起，与文章什么时候发的没有关系。同一个线索中的文章按时间顺序排列。用户可以：

> > > > （a）选择某一篇文章进行阅读，然后可以选择回到文章列表，顺序阅读当前线索的前一篇文章或是后一篇文章。

> > > > （b）允许回复线索，可以选择复制并引用之前文章，用跟贴的方式回复到整个新闻组。附加题：也允许私下用电子邮件进行回复。

> > > > （c）永久地删除线索，即后续的相关文章不会在文章列表中显示。要实现这个功能，你应该把要删除的文章的列表暂时记录下来。一个线索在几个月之后还没有人回复的话，你可以认为这个线索已经死了。

> > 17-17.GUI 新闻阅读工具。跟上面的 FTP 练习差不多，选择一个 GUI 工具包来实现一个完整的、独立的 GUI 新闻阅读工具。

> > 17-18.重构。跟 FTP 的 ftpmirror.py 一样，NNTP 也有一个示例脚本：Demo/scripts/newslist. py。运行它。这个脚本在很久之前就写好了，你可以做一些翻新工作。作为练习，你要用 Python 新版本的一些特性和你的 Python 开发技巧来重构这个脚本，让这个脚本运行得更快。你可以使用列表解析和生成器表达式，用更智能的字符串连接而不是调用不必要的函数等。

> > 17-19.缓冲。如其作者所说，newslist.py 的另一个问题是，“我应该把要忽略的空的新闻组的列表保存下来，在每次运行的时候检查一下是否有新的文章，但我真的抽不出时间”。你来实现这个功能。你可以直接修改它，也可以修改你之前的脚本。电子邮件

> > 17-20.标识符。POP3 的 pass_()方法用于在调用 login()方法传了用户名之后传递密码。你能不能说出，为什么这个方法命名时要在后面加一个下划线，即“pass_()”，而不是“pass()”?

> > 17-21.IMAP。现在，你已经熟悉了 POP 是怎么工作的。这方面的经验对你写一个 IMAP 客户端程序也是有帮助的。研究一下 IMAP 协议的 RFC 文档，使用 Python 的 imaplib 模块来实现一个 IMAP 客户端程序。

> > > > 下面的练习题跟本章（例 17.3）中的 myMail.py 程序有关。

> > 17-22.电子邮件头。在 myMail.py 的最后几行，比较了发送的信息体与接收到的电子邮件的信息体。写一段相似的代码，比较信息头。注意，要忽略新加入的头。

> > 17-23.错误检查。加入 SMTP 和 POP3 的错误检查。

> > 17-24.SMTP 和 IMAP。在简单的 myMail.py 中，加入 IMAP 的支持。附加题：支持两种邮件下载协议，让用户选择要使用哪一种协议。

> > 17-25.撰写电子邮件。再次扩展你之前的程序，允许用户撰写和发送电子邮件。

> > 17-26.电子邮件应用程序。再次扩展你的电子邮件应用程序，在其中加入更有用的邮箱管理功能。你的程序要能读出当前所有电子邮件的信息，并显示其主题。用户可以选择想要看的邮件。附加题：要能支持用外部程序查看附件。

> > 17-27\. GUI。给你的脚本加入 GUI 的功能，让它成为一个实用的完整的电子邮件应用程序。

> > 17-28.垃圾邮件的特点。不请自来的垃圾邮件（spam）是当今的一大问题。所幸，针对这个问题有不少好的解决方案。我们不用你来重新发明轮子，我们想让你了解一些垃圾邮件的特点。

> > > > （a） “mbox”格式。在开始之前，我们要把你想处理的电子邮件信息转为一个公共的格式。

> > > > 比如“mbox”格式。（如果你愿意，你也可以使用别的格式。）如果你已经有了一些 mbox 格式的消息，把它们合并到一个文件中。

> > > > （b）头。很多电子邮件的头上就看出有垃圾邮件的线索。（你可以用 email 包或自己解析头）。写一段代码来回答以下问题：

> > > > -发送这个消息的电子邮件客户端软件是什么？（检查 X-Mailer 头）

> > > > -报文 ID （Message-ID 头）的格式是否合法？

> > > > -From, Received 和 Return-Path 头的域名是否不匹配？域名和 IP 地址是否不匹配？有没有 X-Authentication-Warning 头？如果有的话，内容是什么？

> > > > （c）信息服务器。一些服务器如 WHOIS, SenderBase.org 等可以根据 IP 地址或域名帮助你找到电子邮件来自何方。找到一些这样的服务，写一些代码来得到来源地的国别、城市、网络所有者的名字、联系方法等。

> > > > （d）关键字。垃圾邮件中，有一些字经常出现。你之前一定见过，它们是单个的字母，开头大写的随机字母等。把你常见的一些大量在垃圾邮件中出现的词汇放在一个列表中。把出现了这些词汇的邮件作为疑似垃圾邮件隔离。附加题：设计一种算法或加入一些关键字的变形来找出这些邮件。

> > > > （e）钓鱼。这些垃圾邮件总是想把他们伪装成来自大银行或某个知名的网站的合法的电子邮件。里面包含某种链接，引诱用户输入自己私密的或是敏感的信息，如登录用户名、密码和信用卡的卡号等。这些骗子往往做得足以以假乱真。不过，他们还是免不了要让用户登录到与他们声称的并不相符的网站。这里，就可能会透露出很多信息，如，看上去很乱七八糟的域名，只用了 IP 地址，或是 32 位整型形式而不是字节形式的 IP 地址等。写一段代码来判断一封看上去像正式交流的电子邮件是真的还是假的。

> > > > 其他

> > > > 可以在 http://www.networksorcery.com/enp/topic/ipsuite.htm#Application%201ayer%20protocols 找到包含本章中所列的那些协议在内的各种网际协议的列表。Python（当前）所支持的网际协议列表可以在 http://docs.python.org/lib/internet.html 找到。

> > 17-29.开发其他因特网客户端程序。现在，你已经看到了 4 个 Python 开发因特网客户端程序的例子。选一种 Python 标准库中支持的其他协议，开发一个对应的客户端程序。

> > 17-30.*开发一种新的因特网客户端程序。这个难度比较大：找到一个不常用的，或是还未成型的 Python 尚未支持的协议，实现它。如果做得好的话，你可以考虑提交一个 PEP，把你的实现加入到以后版本 Python 的标准库中发布。