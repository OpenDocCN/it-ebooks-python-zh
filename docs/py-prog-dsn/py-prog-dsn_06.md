# 第五章 图形编程

# 第五章 图形编程

在现实中，人们经常利用直观的图形来表达抽象的思想，图形可以帮助人们设计产品、 理解数据、洞察规律。同样地，在用计算机解决问题时，也经常需要绘制图形。有些应用本 身就是图形图像应用，而另一些应用只是利用图形来使计算可视化。本章主要介绍 Python 图形编程。由于图形是复杂数据，对复杂数据的表示和操作最适合采用面向对象方法，因此 本章还将初步介绍面向对象的基本概念①。

# 5.1 概述

## 5.1 概述

实际应用中经常需要利用图形、图像和动画。例如，在大量数据的统计与分析中，仅仅 算出数学期望、标准差之类的统计指标，并不能使普通人很好地理解数据；但是如果画出直 方图、趋势曲线之类的图形，就能使人们洞悉数据所蕴涵的意义。又如，假设小学教师希望 向学生讲授太阳、地球和月亮三者之间的位置和运动的知识，如果他完全用文字语言来表述 天文知识，恐怕小学生会很难理解；但如果他用图形或动画来演示，相信小学生立刻就能明 白三个天体间的复杂运动。

可见，计算机图形能够帮助我们更好地解决问题，是非常重要的编程技术。本节对图形 编程的意义进行简单讨论，从下一节开始具体介绍 Python 的图形编程。

# 5.1.1 计算可视化

### 5.1.1 计算可视化

随着计算机硬件和软件技术的发展，计算机图形技术越来越成熟，如今已经在各行各业中得到了广泛应用。有一些应用本身的任务就是绘制图形，例如制作动画片、艺术设计之类； 还有一些应用不以绘图为目的，但会利用图形来辅助完成任务，例如统计应用的目的是计算 各种数值指标，但常用图形来直观地展示统计结果。

可视化（visualization）是指将抽象事物和过程转变成视觉可见的、形象直观的图形图 像表示。计算可视化就是在用计算机解决问题的过程中，使用图形图像来表达数据和操作。 图形图像所具有的直观性能使我们更有效地传达信息，即使这信息是非常抽象的。在历史上， 用可视的图形图像来展现信息是很常见的，在有文字之前人类就用图画表达信息，甚至文字 本身也是从图形发展而来的。如今，计算机图形技术为计算可视化提供了强大的支持，促进 了可视化计算在科学、工程、教育等领域的广泛应用。应用中常见的图形包括柱状图、直方 图、散点图、网络图、流程图、树、地图、图像、动画等等。

科学可视化

可视化术语最初是指科学可视化，也就是将科学与工程计算、实验中的大规模数据用直 观的计算机图形图像呈现出来，以便人们理解数据、增强对事物现象的认识和对内在规律的 洞察。

计算机图形技术从诞生起就被用于研究科学问题，如今科学可视化在物理、化学、医学、 空间科学等领域得到了大量应用。通过科学可视化，我们看清了台风的形成、太空飞行器的 活动、分子原子的结构以及人体内部的病灶，并能将纯抽象的概念和构造在 3 维空间中展现 出来②。

工程设计可视化 在工程领域，可视化被计算机辅助设计和制造（CAD/CAM）系统广泛地使用。无论是土木工程还是机械工程、电子工程，设计人员借助计算机图形软件和设备从事产品设计工作， 例如利用计算机自动生成设计图，对设计图进行编辑、缩放、旋转，对不同方案进行比较和 优选等等。此外，可视化还可以使工业过程控制、系统模拟、生产管理等任务以直观的方式 进行，以实现更有效的控制和管理。

> ① 本书第七章详细介绍面向对象编程。
> 
> ② 美国 Science 杂志和 NSF 每年都举办“国际科学与工程可视化挑战赛”，建议读者搜索获奖作品看看。

数据可视化

数据可视化是指利用计算机图形学和图像处理技术，将海量数据转化为数据图像，以便 帮助人们直观地观察数据。对于多维数据（例如人事数据包含姓名、性别、学位、收入等多 种维度），利用数据图像还可以从不同的维度观察数据。一般认为，数据、信息、知识构成 由低到高的三个层次，因此从数据可视化可以进而发展到更高层次的信息可视化（发现数据 中隐藏的模式、关联或趋势）和知识可视化（促进知识的传播）。

图形用户界面

可视化最常见的应用当属图形用户界面（GUI）。计算机软件的用户界面负责支持用户 与计算机进行交互。早期软件的用户界面都是文字式的，用户在屏幕上看到的输出都是文本 信息，并且只能通过键盘输入文本命令来控制软件执行。如今的软件几乎都具有图形用户界 面，屏幕上展现给用户的是各种可视的图形元素，如窗口、图标、按钮和菜单等等；而用户 可以使用鼠标来点击图形元素以控制程序的执行。这样的 GUI 软件使用起来非常直观、高 效，具有所谓的“用户友好性”。本书第八章将详细介绍 GUI 编程。

除了上述领域之外，人们还在教育领域利用可视化创建现实中难以见到的事物（如血液 循环系统、化合物分子、恐龙等）的图形图像，以使教学形象直观；在刑事侦查领域利用可 视化重建犯罪现场、绘制案犯相貌；在娱乐领域利用可视化制作计算机电影特效、动画；等 等。

总之，计算机图形技术极大地增强了人们利用计算机解决问题的能力。因此，学习图形 编程是非常重要的。

# 5.1.2 图形是复杂数据

### 5.1.2 图形是复杂数据

图形编程就是编写能创建和处理图形的程序。从一般的意义上说，图形也是数据，只不过与数值、字符串、列表等类型的数据相比，图形数据是非常复杂的数据。 首先，一个图形包含的信息是复杂的。例如，一个圆形需要用一个圆心和一个半径来定义。半径可以用一个简单的数值来表示，但圆心（平面上的一个点）却需要用两个数值型坐 标组成的元组来表示。这还只是大家在平面几何里认识的圆形，在实际的图形应用中还会考 虑圆形内部和轮廓线的颜色、轮廓线线条的粗细等问题，其中颜色又是由红绿蓝三种颜色分 量构成的复杂数据。可见图形确实是很复杂的数据。

其次，对图形的处理是复杂的。对数值，可以加减乘除；对字符串，可以求子串或连接 两个串；对列表可以取成员或求长度。对一个圆形，能做什么呢？数学中会去求面积、求周 长，但在图形应用中更有意思的操作是改变颜色、移动到另一个位置等，这些操作相对于加 减乘除之类显然复杂得多。

那么，在编程语言中如何表示图形这一类的复杂数据、如何操作这类复杂数据呢？编程 语言一般没有内建的图形数据类型和对图形的操作，但会提供标准图形库用于支持图形编 程。本章将介绍如何使用 Python 语言的标准图形库 Tkinter 来进行图形编程。在介绍 Tkinter 之前，需要先简要介绍对象的概念，因为现代的图形库一般都是采用面向对象技术来实现图 形数据类型的。

# 5.1.3 用对象表示复杂数据

### 5.1.3 用对象表示复杂数据

程序是对数据进行操作的过程，因此数据表示和操作过程是编程时要考虑的两大问题。 我们已经熟悉用编程语言提供的数据类型来表示数据，例如用字符串表示雇员姓名，用整数表示年龄，用浮点数表示工资等。对于某些稍微复杂一点的数据我们也有适合的数据类 型来表示，例如雇员名单可以用一个字符串数据构成的列表来表示。当数据表示确定之后， 我们接着用各种数据类型所支持的数据操作来处理数据，例如对于工资数据可以执行加减乘 除操作，对于姓名数据可以分别抽取出姓和名。

先考虑数据的表示，然后再考虑对数据的操作，这就是迄今为止我们在编程序时常用的 思考方式。在这种思考方式下，数据和对数据的操作被看作是两件相互分离的事情，因而可 以分别考虑。例如，在一元二次方程求解程序中，我们先获得所需的数据（方程系数 a、b、 c），然后才去考虑对这些数据的操作过程，即先计算判别式的正负，再去求方程根。

然而还有另一种思考方式，那就是将数据和对数据的操作视为不可分离的，并将两者组 合在一起形成一个实体——对象（object）。显然，对象是对传统“数据”概念的发展：传统 数据只是存储一些信息，而对象中不但存储了一些信息，而且还掌握了对这些信息的操作。 在面向对象术语中，对象的数据称为属性，对象的操作称为方法。

以一个简单数据"Lu Chaojun"为例，在传统观点下，可用字符串类型来表示这个数 据：

```
name = "Lu Chaojun" 
```

现在，数据 name 仅仅存储了一个姓名，对这个数据能执行什么操作不由 name 决定，而是 由程序的其他部分决定。例如，如果希望按西方习惯将姓放在名的后面显示，则程序中可以 对 name 进行如下操作：

```
lastname = name[0:2] 
firstname = name[3:] 
print firstname,lastname 
```

而在对象观点下，我们将把 name 和能对 name 执行的操作相结合，形成一种对象（如图 5.1 所示 ），该对象 不但存储了 信息 "Lu Chaojun" ，而且还拥有 对信息的操 作 last_first()、first_last()、first()、last()等。这种对象本质上仍然是 name 数据，而且是具有数据操作能力的数据。

![](img/程序设计思想与方法 143641.png)

图 5.1 对象：数据与操作相结合

总之，一个对象不但知道一些信息，并且还负责操作这些信息。要想对对象的数据执行特定操作，只需向对象发出请求消息，由对象来执行所需的方法。 对象概念通常并不是用来描述如上例那样的简单数据的。事实上，对象概念主要用于描述复杂数据、设计复杂系统。对象将若干相关数据连同若干操作组合在一起，形成一种结构单元，从而复杂系统可以方便地设计成由许多对象组成，对象之间通过交互、协作来完成系 统功能。

图形应用程序涉及图形这样的复杂数据以及对图形的各种操作，因此非常适合采用面向 对象概念。许多语言的图形库都是面向对象风格的，其中包括我们将介绍的 Python 标准图 形库 Tkinter。

# 5.2 Tkinter 图形编程

## 5.2 Tkinter 图形编程

Python 语言自带一个标准模块 Tkinter，这是一个功能强大的图形用户界面工具包，能 够用来开发像 Windows 应用程序一样具有窗口、菜单、按钮等图形构件的程序。本章只介 绍 Tkinter 中的绘图功能，基于 Tkinter 的 GUI 编程将在第八章中介绍。

# 5.2.1 导入模块及创建根窗口

### 5.2.1 导入模块及创建根窗口

为了使用 Tkinter 模块中提供的绘图功能，首先要将该模块导入到程序中，就像我们以 前导入 math 模块以使用其中的数学函数、导入 string 模块以使用其中的字符串操作函数一 样。可以用下列两种方式中的任何一种导入 Tkinter：

```
import Tkinter 
```

或者

```
from Tkinter import * 
```

如我们以前所说，这两种导入方法的差别仅在于以后调用模块中的函数时是否要加上模块名 作为前缀。注意，以下我们总是假设使用第二种方式导入 Tkinter 模块。

导入 Tkinter 之后，第二步要做的就是创建一个窗口（称为根窗口），所有图形都是在这 个窗口中绘制的。下列语句创建根窗口并赋值给一个变量 root：

```
root = Tk() 
```

接下去就可以在根窗口中绘制图形了。

以下我们将采用交互式环境来演示 Tkinter 的绘图语句，读者可以照样子键入这些语句 并得到和本书图示一样的结果。注意，由于 IDLE 本身是用 Tkinter 写的程序，在 IDLE 中执 行 Tkinter 语句会有问题，因此本章所有交互式演示都是在命令行环境中执行的。另外，演 示中既可以在同一个窗口中连续执行绘图语句，也可以在每次演示新的图形语句时重新创建 根窗口。不管是哪一种做法，为了避免重复，我们总是假设已经执行了下面两条语句：

```
>>> from Tkinter import *
>>> root = Tk() 
```

这时可以在屏幕上看到如图 5.2 所示的窗口。

![](img/程序设计思想与方法 144795.png)

图 5.2 根窗口

根窗口实际上是一个对象，它有自己的属性（如宽度、高度、窗口标题），也有自己的 方法。本章只关注绘图功能，不需要对根窗口进行操作。有关内容可参见第八章。

# 5.2.2 创建画布

### 5.2.2 创建画布

为了绘图，首先要有画布。Tkinter 中提供了画布（Canvas），可以在画布上绘制图形、 文本，也可以在上面放置命令按钮等 GUI 构件。画布实际上是一个 Canvas 对象，它包含 一些属性（如画布的高度、宽度、背景色等），也包含一些方法（如在画布上创建图形、删 除或移动图形等）。

创建画布对象的语句模板如下：

```
c = Canvas(<窗口>,<选项 1>=<值 1>,<选项 2>=<值 2>,...) 
```

其中 Canvas 是 Tkinter 提供的类（class），所谓“类”其实就和 int、float 等一样是数据类型，只不过不是 Python 语言的内建类型，而是 Tkinter 模块带来的扩展类型。Canvas 就像一个制造画布的工厂，每次执行 Canvas()都能制造出一个画布对象。参数<窗口>表示画布所在的窗口，诸<选项>=<值>为画布对象的选项（即属性）进行赋值。总之，整条语句创建一个 Canvas 对象，对该对象的数据进行设置，并将该对象赋值给变量 c（更准确的 说法是变量 c 引用该对象）。

画布的常用选项包括 height（画布高度）、width（画布宽度）和 bg（或 background， 画布背景色）等，需要在创建画布对象时进行设置。创建画布对象时如果不设置这些选项的 值，则各选项取各自的缺省值，例如 bg 的缺省值为浅灰色。画布对象的所有选项都可以在 创建以后的任何时候重新设置。

下面的语句在根窗口 root 中创建一个宽度为 300 像素①、高度为 200 像素、背景为白 色的画布：

```
>>> c = Canvas(root,width=300,height=200,bg='white') 
```

注意，虽然至此已经创建了画布对象，但在根窗口中并没有看到这块白色画布，这就好 比从商店买来了画布，但还没有铺到桌子上一样。为了让画布在窗口中显现出来，还需要执 行如下“布置画布”的语句②：

```
>>> c.pack() 
```

现在，我们在屏幕上看到原来的根窗口（背景色为浅灰色）中放进了一个 300x200 的白色画 布。如图 5.3 所示。

![](img/程序设计思想与方法 145715.png)

图 5.3 放入画布后的根窗口

这里需要对 c.pack()所用到的“点表示法”加以说明。过去，当我们编写了一个函数 f()来操作数据 x，传统的表示法是 f(x)。而在面向对象编程中，数据和操作被结合在一 起形成了对象，如果要对对象中的数据执行操作，通常采用点表示法——“对象.操作”。在 c.pack()中，变量 c 表示一个 Canvas 对象，pack()是 Canvas 对象能够响应的一个方 法，故 c.pack()就表示向对象 c 发出执行 pack()方法的请求。

> ① 像素（pixel）是能显示的最小图像单元，通俗说就是一个点。数字图像是由很多点组成的。
> 
> ② 在窗口中布置各种构件需要使用布局管理器，这里的 pack()就是一种布局管理器。详见第八章。

坐标系

创建了画布，接下来就可以在画布上绘制各种图形了。为了在绘图时指定图形的绘制位 置，Tkinter 为画布建立了坐标系统。画布坐标系以画布左上角为原点，从原点水平向右为 x 轴，从原点垂直向下为 y 轴（图 5.4）。

![](img/程序设计思想与方法 146155.png)

图 5.4 画布的坐标系统

坐标如果以整数给出，则度量单位是像素，例如左上角原点的坐标为(0,0)，300x200 的画布的右下角坐标为(299,199)。像素是最基本、最常用的长度单位，但 Tkinter 也支持以字符串形式给出其他度量单位的长度值，例如"5c"（5 厘米）、"50m"（50 毫米）、"2i"（2 英寸）等。

图形项的标识

一个画布上可能创建多个图形项①，因此需要有办法来标识、引用其中某个图形项，以 便对该图形项进行处理。画布上的图形项有两种标识方式：

*   标识号：创建图形项时 Tkinter 自动为图形项赋予一个唯一的整数编号。

*   标签：图形项可以与字符串型的标签（tag）相关联，每个图形项可以与 0、1 乃至 多个标签相关联，而同一个标签可以与多个图形项相关联。

标签相当于为图形项命名，只不过一个图形项可以有多个名字，而且不同图形项可以有 相同的名字。为图形项指定标签的方法有三种：一是在创建图形时利用选项 tags 来指定， 可以为 tags 选项提供单个字符串（单个名字），也可以提供一个字符串元组（多个名字）； 二是在图形创建之后，任何时候都可以利用画布的 itemconfig()方法来设置；三是利用 画布的 addtag_withtag()方法来为图形项增添新标签。假设我们已经创建了画布 c，则 可以执行：

```
>>> r1 = c.create_rectangle(20,20,100,80,tags="#1")
>>> r2 = c.create_rectangle(40,50,200,180,tags=("myRect","#2"))
>>> c.itemconfig(r1,tags=("myRect","rectOne"))
>>> c.addtag_withtag("ourRect","rectOne") 
```

> ① 每个图形项可以理解成一个图形对象（有自己的属性和操作），不过 Tkinter 没有采用为每种图形提供单 独的类来创建图形对象的实现方式。5.4.2 中介绍的 graphics 库则采用了更符合面向对象概念的实现。

其中第一行的含义是在画布 c 上创建了一个矩形（稍后详述），create_rectangle()返 回的标识号被赋值给变量 r1，同时将该矩形与标签"#1"相关联。同样地，第二行创建了另 一个矩形，该矩形的标识号被赋值给变量 r2，该矩形还与两个标签"myRect"和"#2"相关 联。第三行的含义是将第一个矩形的标签重新设置为"myRect"和"rectOne"（注意原标签"#1"即告失效），这里使用了标识号 r1 来引用第一个矩形。第四行的含义是为具有标签 "rectOne"的图形项（即第一个矩形）添加一个新标签"ourRect"，这里使用了标签来引 用第一个矩形。至此，第一个矩形与 3 个标签"myRect"、"rectOne"和"ourRect"相关 联，其中任何一个都可用于引用该图形。注意，标签"myRect"同时引用两个矩形。

Canvas 还预定义了 ALL（或"all"）标签，此标签与画布上的所有图形项相关联。

画布对象的方法

上面例子中介绍了画布对象的 itemconfig()和 addtag_withtag()方法，除此之 外，画布对象还提供很多方法用于对画布上的图形项进行各种各样的操作。将图形项的标识 号或标签作为参数传递给画布对象的方法，即可指定被操作的图形项。下面再介绍几个画布 对象的常用方法。

gettags()方法可用于获取与给定图形项相关联的所有标签。例如下面的语句显示标 识号为 r1 的图形项的所有关联标签：

```
>>> print c.gettags(r1) ('myRect', 'rectOne', 'ourRect') 
```

find_withtag()方法可用于获取与给定标签相关联的所有图形项。例如下面的语句显示与"myRect"标签相关联的所有图形项，返回结果为各图形项的标识号所构成的元组：

```
>>> print c.find_withtag("myRect") (1,2) 
```

delete()方法用于从画布上删除指定的图形项。例如下面的语句从画布上删除第一个矩形：

```
>>> c.delete(r1) 
```

move()方法用于在画布上移动指定图形。例如，为了将第二个矩形在 x 方向移动 10 像素，在 y 方向移动 20 像素，可以执行下列语句：

```
>>> c.move(r2,10,20) 
```

读者可查阅 Tkinter 资料以了解更多的画布对象方法。

# 5.2.3 在画布上绘图

### 5.2.3 在画布上绘图

本节介绍如何在画布上绘制图形。为了完整起见，我们将前面介绍过的首先需要执行的

几条语句合在一起复制如下：

```
>>> from Tkinter import *
>>> root = Tk()
>>> c = Canvas(root,width=300,height=200,bg='white')
>>> c.pack() 
```

如前所述，c 是一个画布对象，而画布对象提供了若干方法用于绘制矩形、椭圆、多边 形等图形。为了画特定图形，只要向画布对象 c 发出执行特定方法的请求即可。

矩形

画布对象提供 create_rectangle()方法，用于在画布上创建矩形。矩形的位置和大 小由两点定义：(x0,y0)给出矩形的左上角，(x1,y1)给出矩形的右下角①。用法如下：

```
create_rectangle(x0,y0,x1,y1,<选项设置>...)
id = create_rectangle(x0,y0,x1,y1,<选项设置>...) 
```

> ① 准确地说，矩形并不包含(x1,y1)，即(x1,y1)位于矩形右下角之外。例如，用左上角(10,10)和右下角(12,12)

定义的矩形实际上大小是 2 像素×2 像素，包含像素(11,11)但不包含像素(12,12)。

create_rectangle()的返回值是所创建的矩形的标识号，第一种用法没有保存这个 标识号，而第二种用法将标识号存入了一个变量。

例如，下面的语句创建一个以(50,50)为左上角、以(200,100)为右下角的矩形：

```
>>> c.create_rectangle(50,50,200,100) 1 
```

结果如图 5.5(a)所示。注意，语句返回的 1 是矩形的标识号，表示这个矩形是画布上的 1 号

图形项。为了将来在程序中引用图形，最好用变量来保存图形的标识号，或者将图形与某个 标签相关联。例如我们再创建一个矩形：

```
>>> r2 = c.create_rectangle(80,70,240,150,tags="rect#2")
>>> print r2 2 
```

结果如图 5.5(b)所示。可见，第二个矩形的标识号为 2，它还与标签"rect#2"相关联。

![](img/程序设计思想与方法 149115.png)

(a)

![](img/程序设计思想与方法 149116.png)

(b)

图 5.5 在画布上画矩形

矩形图形实际上可视为由两个部分组成：轮廓线和内部。矩形轮廓线可以用选项 outline 来设置颜色，其缺省值是黑色，即等同于设置 outline="black"。如果将 outline 设置为空串""，则不显示轮廓线（透明的轮廓线）。轮廓线的宽度可以用选项 width 来设置，缺省值为 1 像素。矩形内部可以用选项 fill 来设置填充颜色，此选项的 缺省值是空串，即等同于设置 fill=""，效果是内部透明。

轮廓线可以画成虚线形式，这需要用到选项 dash，该选项的值是整数元组。最常用的 是 2 元组(a,b)，其中 a 指定要画几个像素，b 指定要跳过几个像素，依此重复，直至轮廓 线画完。若 a、b 相等，可以简记为(a,)。

矩形还有个选项 state，用于设置图形的显示状态。缺省值是 NORMAL（或"normal"）， 即正常显示。另一个有用的值是 HIDDEN（或"hidden"），它使矩形在画布上不可见。使 一个图形在 NORMAL 和 HIDDEN 两个状态之间交替变化，能形成闪烁的效果。

另外，5.2.2 中介绍过可以用选项 tags 为矩形关联标签，这里就不重复了。 上面例子中没有为两个矩形设置任何选项，即所有选项都取各自的缺省值。如 5.2.2 中所介绍的，我们可以利用画布对象的方法 itemconfig()来设置选项值。例如：

```
>>> c.itemconfig(1,fill="black")
>>> c.itemconfig(r2,fil1="grey",outline="white",width=6) 
```

执行后的结果如图 5.6 所示。

![](img/程序设计思想与方法 149834.png)

图 5.6 修改图形选项

将图 5.5 和图 5.6 相比较即可看出，在画布上，后创建的矩形是覆盖在先创建的矩形之 上的，并且未涂色时矩形内部是透明的（能看到被覆盖的矩形的轮廓线）。事实上，画布上 的所有图形项都是按创建次序堆叠起来的，第一个创建的图形项处于画布最底部（最靠近背 景），最后创建的图形项处于画布最顶部（最靠近前景）①。图形的位置如果有重叠，则上面 的图形会遮挡下面的图形。

如上一小节所介绍的，可以使用画布对象的 delete()和 move()方法删除和移动图 形。例如下列语句删去第二个矩形（结果见图 5.7(a)），并将第一个矩形在 x 轴和 y 轴方向 都移动 50 个像素（结果见图 5.7(b)）：

```
>>> c.delete(r2)
>>> c.move(1,50,50) 
```

![](img/程序设计思想与方法 150192.png)

![](img/程序设计思想与方法 150194.png)(b)

图 5.7 图形的删除和移动

至此我们详细介绍了矩形的画法、选项设置和图形操作（删除、移动等），接下去介绍 其他图形时会有很多相似的内容，以后我们将不重复详述。

顺便提一下，画布对象没有提供画“点”的方法，但我们可以画一个极小的矩形来当作 点。例如：

```
>>> c.create_rectangle(50,50,51,51) 
```

> ① 即所有图形项形成一个显示列表。画布提供方法来对此列表重新排序。具体方法可参考 Tkinter 资料。

最后说明一下绘制矩形时如何提供坐标数据。create_rectangle()方法中坐标参数 的形式是很灵活的，既可以直接提供坐标值，也可以先将坐标数据存入变量，然后将该变量 传给该方法；既可以将所有坐标数据构成一个元组，也可以将它们组成多个元组。例如， create_rectangle()方法中的四个坐标参数既可以如上面例子那样作为四个值，也可以 定义成两个点（两个 2 元组）分别赋值给两个变量，还可以定义成一个 4 元组并赋值给一个 变量。形如：

```
>>> p1 = (10,10)
>>> p2 = (50,80)
>>> c.create_rectangle(p1,p2,tags="#3")
>>> xy = (100,110,200,220)
>>> c.create_rectangle(xy) 
```

将坐标存储在变量中的做法是值得推荐的，因为这更便于在绘制多个图形时计算、安排 它们的相互位置。要强调的是，这里介绍的内容对所有图形（只要用到坐标参数）都是适用 的。

椭圆形

画布对象提供 create_oval()方法，用于在画布上画一个椭圆形（特例是圆形）。椭 圆的位置和尺寸通过其限定框（bounding box，即外接矩形）决定，而限定框由左上角坐标 (x0,y0)和右下角坐标(x1,y1)定义（参见图 5.8）。

![](img/程序设计思想与方法 150862.png)

图 5.8 用限定框定义椭圆 创建椭圆的语句模板如下：

```
create_oval(x0,y0,x1,y1,<选项设置>...)
id = create_oval(x0,y0,x1,y1,<选项设置>...) 
```

create_oval()的返回值是所创建的椭圆形的标识号，第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。

例如，下面的语句序列描绘地球绕太阳旋转的轨道，其中分别创建了一个椭圆形和两个 圆形，并且为大圆形涂上红色表示太阳，为小圆形涂上蓝色表示地球（参见图 5.9）。

```
>>> o1 = c.create_oval(50,50,250,150)
>>> o2 = c.create_oval(110,85,140,115,fill='red')
>>> o3 = c.create_oval(245,95,255,105,fill='blue') 
```

![](img/程序设计思想与方法 151164.png)

图 5.9 椭圆和圆

和矩形类似，椭圆形的常用选项包括 fill、outline、width、dash、state 和 tags 等。

画布对象的 delete()方法、move()方法和 itemconfig()方法同样可用于椭圆形 的删除、移动和选项设置。

弧形

画布对象提供 create_arc()方法，用于在画布上创建一个弧形。与椭圆的绘制类似， create_arc()的参数是用来定义一个矩形的左上角和右下角的坐标，该矩形唯一确定了 一个内接椭圆形（特例是圆形），而最终要画的弧形是该椭圆的一段。创建弧形的语句模板 如下：

```
create_arc(x0,y0,x1,y1,<选项设置>...)
id = create_arc(x0,y0,x1,y1,<选项设置>...) 
```

create_arc()的返回值是所创建的弧形的标识号，第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。

弧形的开始位置由选项 start 定义，其值为一个角度（以 x 轴方向为 0 度）；弧形的结 束位置由选项 extent 定义，其值表示从开始位置逆时针旋转的角度。start 的缺省值为 0 度，extent 的缺省值为 90 度。显然，如果 start 设置为 0 度，extent 设置为 360 度， 则画出一个完整的椭圆形，效果和 create_oval()方法一样。

选项 style 用于规定弧形的式样，可以取三种值：PIESLICE 或"pieslice"是扇形（弧形两端与圆心相连），ARC 或"arc"是弧（圆周上的一段），CHORD 或"chord"是弓形（弧加连接弧两端点的弦）。style 的缺省值是 PIESLICE。如图 5.10 所示。

![](img/程序设计思想与方法 152143.png)

图 5.10 三种弧形

弧形的其他常用选项 outline、fill、width、dash、state 和 tags 的意义和缺 省值都和矩形类似。注意只有 PIESLICE 和 CHORD 形状才可填充颜色。

下面的例子画了一个扇形、一条弧和一个弓形，结果如图 5.11 所示。

```
>>> bbox = (50,50,250,150)
>>> c.create_arc(bbox)
>>> c.create_arc(bbox,start=100,extent=140,style="arc",width=4)
>>> c.create_arc(bbox,start=250,extent=110,style="chord") 
```

![](img/程序设计思想与方法 152462.png)

图 5.11 弧形

画布对象的 delete()方法、move()方法和 itemconfig()方法同样可用于弧形的 删除、移动和选项设置。

线条

画布对象提供 create_line()方法，用于在画布上创建连接多个点的线段序列，其语 句模板如下：

```
create_line(x0,y0,x1,y1,...,xn,yn,<选项设置>...)
id = create_line(x0,y0,x1,y1,...,xn,yn,<选项设置>...) 
```

create_line()方法将各点(x0,y0)、(x1,y1)、…、(xn,yn)按顺序用线条连接起来，返回值是所创建的线条的标识号。第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。 没有特别说明的话，相邻两点间用直线连接，即图形整体上是条折线。但如果将选项 smooth 设置成非 0 值，则各点被解释成 B-样条曲线的顶点，图形整体是一条平滑的曲线。 不同于矩形、椭圆、弧形中的扇形和弓形的是，线条不能形成轮廓线和内部区域两部分，因此没有 outline 选项，只有 fill 选项。选项 fill 在此意为线条的颜色，其缺省值为 黑色。选项 width 设置线条宽度，缺省值为 1 像素。

线条可以通过选项 arrow 来设置箭头，该选项的缺省值是 NONE（无箭头）。如果将 arrow 设置为 FIRST 或"first"，则箭头在(x0,y0)端；设置为 LAST 或"last"，则箭 头在(xn,yn)端；设置为 BOTH 或"both"，则两端都有箭头。

选项 arrowshape 用于描述箭头形状，其值为 3 元组(d1,d2,d3)，含义如图 5.12 所 示。缺省值为(8,10,3)。

![](img/程序设计思想与方法 153140.png)

图 5.12 箭头形状的定义

线条和前面介绍的各种图形一样，具有 dash、state、tags 等选项。

下面的语句序列画的是北斗七星（大熊座）和北极星：其中 s1 到 s7 以及 polaris

给出了各星的坐标，我们在这些位置创建了涂黑的圆形表示北斗七星和北极星，然后用直线 段依次连接 s1 到 s7，另外沿 s6－s7 延长线方向画了根带箭头的虚线指向北极星①。最下 方曲线表示地球表面，这里虽然只提供了三个点，但 Tkinter 仍能画出一条相当平滑的曲线。

```
>>> s1 = (20,20)
>>> s2 = (60,40)
>>> s3 = (80,60)
>>> s4 = (85,80)
>>> s5 = (70,100)
>>> s6 = (85,115)
>>> s7 = (110,100)
>>> polaris = (220,40)
>>> c.create_oval(s1,(23,23),fill='black')
>>> c.create_oval(s2,(63,43),fill='black')
>>> c.create_oval(s3,(83,63),fill='black')
>>> c.create_oval(s4,(88,83),fill='black')
>>> c.create_oval(s5,(73,103),fill='black')
>>> c.create_oval(s6,(88,118),fill='black')
>>> c.create_oval(s7,(113,103),fill='black')
>>> c.create_oval((222,36),(226,42),fill='black')
>>> c.create_line(s1,s2,s3,s4,s5,s6,s7,s4)
>>> c.create_line(s7,polaris,dash=(4,),arrow=LAST)
>>> c.create_line(5,190,150,160,295,190,smooth=1) 
```

结果如图 5.13 所示。

![](img/程序设计思想与方法 154104.png)

图 5.13 线条

画布对象的 delete()方法、move()方法和 itemconfig()方法同样可用于线条的 删除、移动和选项设置。

多边形

画布对象提供 create_polygon()方法，用于在画布上创建一个多边形。与线条类似， 多边形是用一系列顶点（至少三个）的坐标定义的，系统将把这些顶点按次序连接起来。与 线条不同的是，最后一个顶点需要与第一个顶点连接，从而形成封闭的形状。

> ① 这是利用北斗七星寻找北极星进行定向的常识方法。

创建多边形的语句模板如下：

```
create_polygon(x0,y0,x1,y1, ..., <选项设置>...)
id = create_polygon(x0,y0,x1,y1, ..., <选项设置>...) 
```

create_polygon()的返回值是所创建多边形的标识号，第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。

和矩形类似，outline 和 fill 分别设置多边形的轮廓线颜色和内部填充色；但与矩 形不同的是，多边形的 outline 选项缺省值为空串，即轮廓线不可见，而 fill 选项的缺 省值为黑色。

与线条类似，一般用直线连接顶点，但如果将选项 smooth 设置成非 0 值，则表示用 B-样条曲线连接顶点，这样绘制的是由平滑曲线围成的图形。

下面的语句序列以不同方式连接 5 个点，或设置不同的选项值，形成三种不同的五边形：

```
>>> p11,p21,p31 = (70,20),(70+100,20),(70,20+100)
>>> p12,p22,p32 = (35,50),(35+100,50),(35,50+100)
>>> p13,p23,p33 = (55,85),(55+100,85),(55,85+100)
>>> p14,p24,p34 = (85,85),(85+100,85),(85,85+100)
>>> p15,p25,p35 = (105,50),(105+100,50),(105,50+100)
>>> c.create_polygon(p11,p12,p13,p14,p15)
>>> c.create_polygon(p21,p23,p25,p22,p24,outline="black",fill="")
>>> c.create_polygon(p31,p32,p33,p34,p35,outline="black",fill="") 
```

执行结果如图 5.14 所示。

![](img/程序设计思想与方法 155198.png)

图 5.14 多边形

多边形的另几个常用选项 width、dash、state 和 tags 的用法都和矩形类似。 画布对象的 delete()方法、move()方法和 itemconfig()方法同样可用于多边形的删除、移动和选项设置。

文本

画布对象提供 create_text()方法，用于在画布上显示一行或多行文本。这里，文本 是作为图形对象看待的，与普通的字符串不同。创建文本的语句模板如下：

```
create_text(x,y,<选项设置>...)
id = create_text(x,y,<选项设置>...) 
```

其中(x,y)指定显示文本的参考位置。create_text()的返回值是所创建的文本的标识 号，第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。

文本内容由选项 text 设置，其值就是显示的字符串。字符串中可以使用换行字符"\n"， 从而实现多行文本的显示。

选项 anchor 用于指定文本的哪个“锚点”与显示位置(x,y)对齐。首先想象文本有 个界限框，Tkinter 为界限框定义了若干个“锚点”，锚点用东南西北等方位常量表示，如图 5.15 所示。通过锚点可以控制文本的相对位置，例如，若将 anchor 设置为 SW，则将文本界限框的左下角置于参考点(x,y)；若将 anchor 设置为 N，则将文本界限框的顶边中点置 于参考点(x,y)。anchor 的缺省值为 CENTER，表示将文本的中心置于参考点(x,y)。

![](img/程序设计思想与方法 155844.png)

图 5.15 锚点

选项 fill 用于设置文本的颜色，缺省值为黑色。如果设置为空串，则文本不可见。 选项 justify 用于控制多行文本的对齐方式，其值为 LEFT、CENTER 或 RIGHT，缺省值为 LEFT。而 width 用于控制文本的宽度，超出宽度就要换行。 选项 state、tags 的意义同前。 下面的语句序列演示了如何在画布上安排文本：

```
>>> t1 = c.create_text(10,10,text="NW@(10,10)",anchor=NW)
>>> c.create_text(150,10,text="N@(150,10)",anchor=N)
>>> c.create_text(290,10,text="NE@(290,10)",anchor=NE)
>>> c.create_text(10,100,text="W@(10,100)",anchor=W)
>>> c.create_text(150,100,text="CENTER@(150,100)\n2nd Line")
>>> c.create_text(290,100,text="E@(290,100)",anchor=E)
>>> c.create_text(10,190,text="SW@(10,190)",anchor=SW)
>>> c.create_text(150,190,text="S@(150,190)",anchor=S)
>>> c.create_text(290,190,text="SE@(290,190)",anchor=SE) 
```

![](img/程序设计思想与方法 156533.png)

结果如图 5.16 所示。

图 5.16 文本

程序中可能需要读取或修改文本的内容，画布对象的 itemcget()和 itemconfig()方法可用于此目的。例如：

```
>>> print c.itemcget(t1,"text")
NW@(10,10)
>>> c.itemconfig(t1,text="NorthWest") 
```

其中第一行读取标识号为 t1 的文本项的 text 选项值；第三行将标识号为 t1 的文本的 text 选项重新设置为"NorthWest"。

画布对象的 delete()方法、move()方法同样可用于文本的删除和移动。

图像

除了在画布上自己画图，还可以将来自文件的现成图像显示在画布上。Tkinter 针对不 同格式的图像文件有不同的显示方法，这里我们只介绍显示 gif 格式图像的方法。

第一步是利用 Tkinter 提供的 PhotoImage 类来创建图像对象，语句模板如下：

```
img = PhotoImage(file = <图像文件名>) 
```

其中选项 file 用于指定图像文件（支持 gif、pgm、ppm 格式①），PhotoImage()返回值是一个图像对象，这里我们用变量 img 引用这个对象，接下去将把这个图像对象显示在画布中②。

第二步是在画布上显示图像，这可通过画布对象提供的 create_image()方法完成。 该方法的用法如下：

```
c.create_image(x,y,image = <图像对象>,<选项设置>...)
id = c.create_image(x,y,image=<图像对象>,<选项设置>...) 
```

其中(x,y)是决定图像显示位置的参考点；image 选项决定显示的图像，其值就是第一步创建的图像对象。create_image()的返回值是所创建的图像在画布上的标识号，第一种用法没有保存这个标识号，而第二种用法将标识号存入了一个变量。

图像在画布上的位置由参考点(x,y)和 anchor 选项决定，具体设置与文本相同。 例如，假设电脑上有个文件 C:\WINDOWS\Web\exclam.gif ，则下列语句序列首先为该图像文件创建了一个图像对象 pic，然后将该图像对象显示在了画布中：

```
>>> pic = PhotoImage(file = "C:\WINDOWS\Web\exclam.gif")
>>> c.create_image(150,100,image=pic) 
```

![](img/程序设计思想与方法 157597.png)

结果如图 5.17 所示。

图 5.17 画布上显示图像 可以为图像设置选项 state、tags，意义同前。

画布对象的 delete()方法、move()方法同样可用于图像的删除和移动。

> ① 至于常见的 jpg 格式或其他格式的图像，可以利用 Python 图像库（PIL）转换成 Tkinter 图像对象。
> 
> ② 还可以显示在按钮（Button）、标签（Label）、文本（Text）等 GUI 构件中，参见第八章。

除了以上各种图形、文字和图像，我们还可以在画布上放置其他 GUI 构件，例如按钮、 勾选钮等，以便用户更好地与画布进行交互。读者学习了第八章后可以试着编写这样的程序。

# 5.2.4 图形的事件处理

### 5.2.4 图形的事件处理

面向对象的概念是和事件驱动编程联系在一起的。所谓事件是指在程序执行过程中发生的事情，例如点击了鼠标左键、按下了键盘上的回车键之类。某个对象可以与特定事件绑定 在一起，这样当特定事件发生时，可以调用特定的函数来处理这个事件。

画布及画布上的图形都是对象，都可以与交互事件绑定，这样用户可以利用键盘、鼠标 来操作、控制画布和图形。第八章将详细介绍 Tkinter 的事件驱动编程，这里我们只用一个 简单例子展示画布和图形对象的事件处理能力。

【程序 5.1】eg5_1.py

```
from Tkinter import *
def canvasFunc(event):
    if c.itemcget(t,"text") == "Hello!": 
        c.itemconfig(t,text="Goodbye!")
    else:
        c.itemconfig(t,text="Hello!")
def textFunc(event):
    if c.itemcget(t,"fill") != "white": 
        c.itemconfig(t,fill="white")
    else:
        c.itemconfig(t,fill="black")
root = Tk()
c = Canvas(root,width=300,height=200,bg='white') 
c.pack()
t = c.create_text(150,100,text="Hello!") 
c.bind("<Button-1>",canvasFunc) 
c.tag_bind(t,"<Button-3>",textFunc) 
root.mainloop() 
```

下面我们对此程序中与事件处理有关的几个要点进行说明。

事件绑定

对象 O 需要与特定事件 E 进行绑定，以便告诉系统当针对 O 发生了 E 之后该如何处理。 程序 5.1 的倒数第 3 行中，利用画布的 bind()方法将画布对象 c 与鼠标左键点击事件"<Button-1>"进行了绑定，其中告诉系统当用户在画布 c 上点击鼠标左键时，就去执行 函数 canvasFunc()。

程序 5.1 的倒数第 2 行中，利用画布的 tag_bind()方法将画布对象 c 上的图形项（文 本）t 与鼠标右键点击事件"<Button-3>"进行了绑定，其中告诉系统当用户在文本 t 上 点击鼠标右键时，就去执行函数 textFunc()。

事件处理函数

程序员可以自定义对事件的处理函数。

程序 5.1 中定义了 canvasFunc()函数用于处理画布上的鼠标左键点击事件，其功能 是改变文本 t 的内容：如果当前内容是"Hello!"就改成"Goodbye!"，如果当前是 "Goodbye!"就改成"Hello!"。每当用户在画布上点击鼠标左键时就执行一次这个函数， 形成文字内容随鼠标点击而切换的效果。

程序 5.1 中还定义了 textFunc()函数用于处理文本上的鼠标右键点击事件，其功能是 改变文本 txt 的颜色：如果当前不是白色则改为白色，否则改为黑色。每当用户在文本上 点击鼠标右键时就执行一次这个函数，形成文本随鼠标点击而出没的效果。注意画布背景色 是白色，因此将文本设置为白色就相当于隐去文本。

主事件循环

程序 5.1 中并没有调用上述两个事件处理函数的语句，它们是由系统根据所发生的事件 而 自 动调用 的 。系统 如 何知道 现 在发生 了 什么事 件 呢？程 序 5.1 中最后一 行 root.mainloop()的意义是进入根窗口的主事件循环。执行了这一条语句之后，系统就会 自行监控在根窗口上发生的各种事件，并触发相应的处理函数。

以上对 Tkinter 的事件处理作了简单介绍，如果读者仍有疑惑，第八章中有详细介绍。

# 5.3 编程案例

## 5.3 编程案例

# 5.3.1 统计图表

### 5.3.1 统计图表

图形的一个重要用途是为数据提供可视的表示，这在统计、汇总性质的应用程序中尤其 重要，因为汇总数据几乎都可以利用图形来改善表示。下面我们编写一个简单的统计汇总程 序，以演示图形编程在数据可视化方面的应用。

假设某高校的老师在考试后需要根据学生的考试成绩来分析试卷，以判断试卷是偏难、 偏容易还是适中。难度适中的试卷应该导致正态分布的成绩。为帮助老师完成试卷分析，我 们编写一个统计汇总程序，其功能是：老师输入考试分数（百分制），然后程序将分数换算 成等级制（分为 A、B、C、D、F 五等）并统计各等级分数的人数，最后画一个饼图来直观 地给出各等级人数的比例。

程序规格

输入：考试分数。

输出：以饼图表示的各分数段所占比例。

算法设计

本程序在算法上很简单，属于典型的 IPO（输入－处理－输出）模式。不过虽然算法很 简单，但是在绘制图形方面需要花费大量精力，因为绘图涉及精确的坐标、形状、颜色等细 节，还需要整个图形画面看上去整齐、匀称、美观。可以说，图形编程中大量时间都花在了 这类“美工”任务之上。

首先，由用户输入每个学生的分数（百分制）。然后根据该分数所对应的等级去累加各 等级人数变量。输入结束后，总人数和各等级人数就确定了。

其次，计算各分数等级人数占总人数的比例。

然后，根据比例绘制饼图。在 Tkinter 编程中，这需要先创建窗口和画布，然后利用画 布的 create_arc()方法绘制代表五个等级的五个扇形。扇形的角度反映了各分数等级的

比例，扇形具有不同填充色以相互区分。为了显示各扇形对应的等级，还需要绘制图例。 最后，用户通过饼图各扇形的大小只能看出各分数等级所占的大致比例。精确的比例值

当然可以固定显示在画面中，不过我们采用另一种更有趣的设计：当用户将鼠标指针移入某 个扇形中时，画布上就显示该扇形所代表的比例值。

以上步骤还需要进一步明确细节，最主要的就是窗口、画布的大小和各图形项的精确位 置等。通过用草图等手段做一些计算和试验，最终确定如图 5.18 所示的设计：

![](img/程序设计思想与方法 160327.png)

图 5.18 画布图形项设计

至此，可以写出本程序的算法伪代码。

```
算法：
用户输入考试分数 mark，并根据 mark 对应的等级累加各等级的人数 a、b、c、d、f；
创建窗口和大小为 300x200 的画布；
计算各分数等级的比例（a/n 等），并据此确定每个扇形的起止角度（sA、eA 等）； 
绘制各个扇形；
绘制图例； 为各扇形绑定“鼠标进入”事件，并定义事件处理函数（inPieA()等）； 
进入主事件循环。 
```

代码实现

从上面的算法很容易翻译成 Python 代码。程序 5.2 中所用到的知识都在前面介绍过， 只有“鼠标进入”事件的处理需要说明一下。

当鼠标指针移到某个图形项上面时即发生事件"<Enter>"，这时系统触发所绑定的事 件处理函数（如 inPieA），这些函数的功能是计算该图形项对应的比例值，然后显示在画 布上的指定位置。另外由于事件处理函数中需要引用画布对象和各图形项，所以我们将这些 函数的定义放在了 main()函数内部，以便它们能引用 main()中定义的变量，即 cv、 piepct、a、b、c、d、f 和 n。

【程序 5.2】piechart.py

```
from Tkinter import *
def getMarks(): 
    a,b,c,d,f = 0,0,0,0,0
    mark = input("Enter a mark: ") 
    while mark >= 0:
        if mark >= 90:
            a = a + 1 
        elif mark &gt;= 80:
            b = b + 1 
        elif mark &gt;= 70:
            c = c + 1 
        elif mark &gt;= 60:
            d = d + 1 
        else:
            f = f + 1
        mark = input("Enter a mark: ") 
    return a,b,c,d,f
def main():
    a,b,c,d,f = getMarks()
    win = Tk()
    cv = Canvas(win,width=300,height=200,bg="white") 
    cv.pack()
    n = a+b+c+d+f
    eA,sA = 360.0*a/n,0 
    eB,sB = 360.0*b/n,eA 
    eC,sC = 360.0*c/n,eA+eB
    eD,sD = 360.0*d/n,eA+eB+eC 
    eF,sF = 360.0*f/n,eA+eB+eC+eD
    bb = (90,40,210,160)
    pieA = cv.create_arc(bb,start=sA,extent=eA,fill="yellow") 
    pieB = cv.create_arc(bb,start=sB,extent=eB,fill="green") 
    pieC = cv.create_arc(bb,start=sC,extent=eC,fill="black") 
    pieD = cv.create_arc(bb,start=sD,extent=eD,fill="gray") 
    pieF = cv.create_arc(bb,start=sF,extent=eF,fill="red")
    cv.create_rectangle(240,40,260,50,fill="yellow") cv.create_rectangle(240,40+24,260,50+24,fill="green") cv.create_rectangle(240,40+48,260,50+48,fill="black") cv.create_rectangle(240,40+72,260,50+72,fill="gray") cv.create_rectangle(240,40+96,260,50+96,fill="red")
    cv.create_text(270,40,text="A",anchor=N) 
    cv.create_text(270,40+24,text="B",anchor=N) cv.create_text(270,40+48,text="C",anchor=N) cv.create_text(270,40+72,text="D",anchor=N) cv.create_text(270,40+96,text="F",anchor=N)
    piepct = cv.create_text(40,100,text="")
    def inPieA(event):
        pct = "%5.1f%%" % (100.0*a/n) 
        cv.itemconfig(piepct,text=pct)
    def inPieB(event):
        pct = "%5.1f%%" % (100.0*b/n) 
        cv.itemconfig(piepct,text=pct)
    def inPieC(event):
        pct = "%5.1f%%" % (100.0*c/n) 
        cv.itemconfig(piepct,text=pct)
    def inPieD(event):
        pct = "%5.1f%%" % (100.0*d/n) 
        cv.itemconfig(piepct,text=pct)
    def inPieF(event):
        pct = "%5.1f%%" % (100.0*f/n) 
        cv.itemconfig(piepct,text=pct)
    cv.tag_bind(pieA,"&lt;Enter&gt;",inPieA) 
    cv.tag_bind(pieB,"&lt;Enter&gt;",inPieB) 
    cv.tag_bind(pieC,"&lt;Enter&gt;",inPieC) 
    cv.tag_bind(pieD,"&lt;Enter&gt;",inPieD) 
    cv.tag_bind(pieF,"&lt;Enter&gt;",inPieF)
    win.mainloop()
main() 
```

程序 5.2 的一次运行结果如图 5.19 所示。

![](img/程序设计思想与方法 162910.png)

图 5.19 程序 5.2 的一次执行结果

# 5.3.2 计算机动画

### 5.3.2 计算机动画

顾名思义，动画就是运动的画面，计算机动画就是通过计算机编程来实现运动的画面。计算机动画在很多领域中都有应用，例如游戏开发、电影电视制作、教学演示等。计算机动 画并不神秘，只要掌握了静止图形的绘制方法，就很容易学会活动画面的制作。

现实世界中运动是连续的，而数字计算机只能处理离散量，因此计算机动画本质上只能 是对连续运动的近似和模拟。具体来说，动画是通过在屏幕上快速地交替显示一组静止图形（图像），或者让一幅图形（图像）快速地移动而实现的。每一幅静止图形（图像）称为一 帧，帧与帧之间在画面上只有小部分的不同，于是人眼的视觉暂留现象会使我们产生运动的 感觉。实验表明，每秒显示 24 帧画面能使人眼感觉到最佳的连续运动效果，所以在连续两帧画面之间应该停顿 0.04 秒左右。 动画制作有很多现成软件工具可用，例如在网页和多媒体教学中常用的 Flash。而我们在此介绍的是直接编程实现动画。

下面我们利用 Tkinter 来实现一个简单的动画程序。程序的功能是演示太阳、地球和月 球三个天体之间的运动情况，即月球绕地球运动，并且和地球一起绕太阳运动。

程序规格

输入：没有输入。

输出：演示太阳、地球和月球之间相对运动的动画。

算法设计

首先当然是建立窗口和画布，然后画出太阳、地球和月球三个天体，具体做法与 5.2.3 节中绘制椭圆的例子相似（图 5.9），当然现在需要多画一个月球，并且需要移动地球和月球。 本程序最关键的部分是解决地球和月球沿椭圆轨道移动的计算（假设太阳位置固定不动），下面先解决地球的运动问题。中学数学告诉我们，椭圆可以用如下方程来刻划：

```
x = a cos t
y = b sin t 
```

因此，地球在轨道上自西向东旋转时的每一个位置(x,y)都可利用此方程算出，其中椭圆轨 道的 a、b 值是固定不变的，位置只由旋转角度 t 决定（参见图 5.20）。假设地球每次旋转 0.01p 弧度（这就是连续运动的离散化！），则地球的下一位置就是

```
x' = a cos(t + 0.01 pi)
y' = b sin(t + 0.01 pi) 
```

由此可算出

```
dx = x' - x
dy = y' - y 
```

于是可利用画布对象的 move()方法来移动地球到新的位置。

再看图 5.20，由于画布的坐标系原点不是椭圆轨道的中心，椭圆中心在画布坐标系中是(150,100)，故地球在 t 角度时的位置应该做个变换：

```
x = 150 + a cos t
y = 100 - b sin t 
```

注意画布坐标系中 y 轴是向下的，因此上式计算 x 和 y 坐标时有加减的不同。

![](img/程序设计思想与方法 164019.png)

图 5.20 地球沿椭圆轨道旋转位置的计算

接下来解决月球的运动问题。首先月球是与地球一起沿椭圆轨道绕太阳运动的，因此月球相对于太阳的位置变化与地球一样由上述 dx 和 dy 决定，即程序 5.3 中的 edx 和 edy。 此外，月球又在绕地球旋转，利用同样方法可算出月球沿绕地球椭圆轨道（设 a、b 的值分 别为 20 和 15）运动时相对于地球的位置变化，即程序 5.3 中的 mdx 和 mdy。最终月球的位 置变化为 edx+mdx 和 edy+mdy。注意，月球绕地球的旋转速度大约是地球绕太阳的旋转 速度的 12 倍（一年有十二个月）。

解决了关键的地球月球的位置计算问题，本程序的算法就明确了，伪代码如下：

```
算法： 
创建窗口和画布；
在画布上绘制太阳、地球和月球，以及地球的绕日椭圆轨道； 
设置地球和月球的当前位置；
进入动画循环： 
    旋转 0.01p；
    计算地球和月球的新位置； 
    移动地球和月球到新位置 
    更新地球和月球的当前位置； 
    停顿一会 
```

代码实现

上面的算法很容易翻译成如程序 5.3 所示的 Python 代码。代码中有两处需要说明一下： 第一，每次循环中修改图形位置后都必须执行一个更新画布显示的方法 c.update()，以 使新画面显示出来；第二，两个画面之间的停顿可以用 time 模块中的 sleep()函数来实 现，该函数的作用就是让程序休眠一会（参数以秒为单位）①。

【程序 5.3】animation.py

```
from Tkinter import *
from math import sin,cos,pi from time import sleep
def main():
    root = Tk()
    c = Canvas(root,width=300,height=200,bg='white') 
    c.pack()
    orbit = c.create_oval(50,50,250,150)
    sun = c.create_oval(110,85,140,115,fill='red') 
    earth = c.create_oval(245,95,255,105,fill='blue') 
    moon = c.create_oval(265,98,270,103)
    eX = 250 # earth's X
    eY = 100 # earth's Y
    m2eX = 20 # moon's X relative to earth 
    m2eY = 0 # moon's Y relative to earth 
    t = 0
    while True:
        t = t + 0.01*pi
        new_eX = 150 + 100 * cos(t) 
        new_eY = 100 - 50 * sin(t) 
        new_m2eX = 20 * cos(12*t) 
        new_m2eY = -15 * sin(12*t)
        edx = new_eX - eX 
        edy = new_eY - eY
        mdx = new_m2eX - m2eX 
        mdy = new_m2eY - m2eY
        c.move(earth,edx,edy) 
        c.move(moon,mdx+edx,mdy+edy) 
        c.update()
        eX = new_eX 
        eY = new_eY
        m2eX = new_m2eX 
        m2eY = new_m2eY
        sleep(0.04)
main() 
```

> ① 如果不知道这个 sleep 函数，也可以自己写一个纯粹消磨时间的循环语句，例如循环 1 百万次，每次 都执行无用语句。同样能起到让画面停顿的效果。

图 5.21 是程序 5.3 执行过程中的一个屏幕截图。

![](img/程序设计思想与方法 165638.png)

图 5.21 程序 5.3 的屏幕截图

# 5.4 软件的层次化设计：一个案例

## 5.4 软件的层次化设计：一个案例

一个复杂软件通常是由很多构件组成的，各构件之间的交互关系有多种模式。例如，在

面向过程编程中，一个程序通常是由多个子程序（过程或函数）组成的，各子程序之间通过 调用和返回来进行交互。又如，在面向对象编程中，一个程序是由许多对象组成的，对象之 间通过发送消息来进行交互。本节中我们通过案例来简单介绍一种常用的软件设计方法—— 层次化设计。

# 5.4.1 层次化体系结构

### 5.4.1 层次化体系结构

层次化设计是构造复杂系统的一个基本方法，按此方法设计出的系统具有层次化体系结构。现实世界中这种层次化结构俯拾皆是。例如，一幢高楼总是从最底层打基础开始，一层 一层地加高。又如，我国的行政组织具有街道、区、市、省、中央这样的层次化结构。

计算机软件的各个构件也经常组织成这样的层次体系结构。在层次体系中，下层构件为 上层构件提供服务，上层构件使用下层构件的服务，上层和下层之间形成一种类似“调用－ 返回”的关系。为了正确地调用和返回，每一层都需要提供一个界面（接口）给上层，以便 与之交互。层次体系顶层为程序员或最终用户提供界面。

我们在自顶向下逐步求精设计方法中也使用了层次化的设计，只不过那里的层次体现的 是功能的分解，即一个函数用更加细化的函数来实现，上下层之间就是函数的调用－返回关 系。而在这里讨论的是用于不同目的的层次化体系结构，其中上下层之间并非功能分解的关 系，分层是为了建立不同的界面。打个比方，假设有一种多功能电视机，其面板上有许多功 能按钮，然而多数老年人既不明白也不需要使用那些先进的功能，复杂的面板只会让老人连 简单的频道和音量按钮也搞不清。这时我们可以在原面板上覆盖一层新面板，其上只留下频 道和音量按钮，现在老人看到的电视机就有了简单易用的界面（图 5.22）。

![](img/程序设计思想与方法 166414.png)

图 5.22 为电视机加一层面板

采用层次化设计的计算机软件的构件分成若干层，先有低层构件，然后在其上架设高层 构件。高层构件的功能依赖于低层构件的功能，但高层构件一般更容易理解，程序员或用户 使用起来更方便。典型的层次化软件体系结构的例子包括数据库的 ANSI-SPARC 三层模式、 网络技术的 ISO/OSI 七层模型、Web 应用开发中的三层体系结构等等。

层次化体系结构的主要优点包括重用和标准化。重用是指同样的构件可以用在任何具有 相同界面要求的地方；同样，只要层次间界面不变，一个构件也可以换用以不同方式实现的 其他同类构件。还以图 5.22 例打比方，我们自制的面板可以用于同品牌型号的所有电视机， 并且木质的面板可以换用塑料面板，黑色面板可以换用彩色面板，等等。标准化是指由标准 化组织为某一类软件构件定义标准界面，而各软件厂商可以采取不同的低层实现技术来实现 高层的标准界面。就好比家电协会规定所有电视机的面板都必须包括电源开关，而各厂商可 以用按钮来实现电源开关，也可以使用红外遥控来开关。

层次化体系结构的主要缺点是效率不如整体式结构，这是因为当程序员或用户面对顶层 构件请求某项服务时，这个请求需要从高层到低层逐层下传，最终由底层构件来实现功能， 再将结果逐层上传，直至顶层用户。这个逐层转换的过程显然很耗费时间。假如用户能直接 与底层打交道，功能的实现就会高效的多。

# 5.4.2 案例：图形库 graphics

### 5.4.2 案例：图形库 graphics

如前所述，Tkinter 是 Python 语言的标准库，可以利用 Tkinter 中的画布构件来绘制图形。 虽然利用 Tkinter 来进行图形编程已经比较简单、方便，但对初学者来说可能还是有点小麻 烦。例如，画布甚至都没有提供画“点”的方法，初学者希望画点时往往不知怎么办。又如， 圆形一般都是通过圆心和半径来定义的，但在画布上画圆形时必须利用界限框（外接正方形） 来定义。另外，对图形的各种操作（如移动图形、修改图形的选项值等）都是通过调用画布 的方法来执行的，而根据面向对象的思想，更容易理解的做法应该是直接针对图形对象发出 操作请求。

由于上述理由，有人①在 Tkinter 之上写了一个更容易使用的图形库——graphics。这个 图形库是为教学目的而开发的，它将 Tkinter 的绘图功能以面向对象的方式重新包装了一下， 使得初学者更容易学习和应用。使用 graphics 提供的功能实际上就是使用 Tkinter 的功能， 但使用者并不知道这一点，也不需要知道这一点，这就是层次体系结构带来的效果。图 5.23 显示了 graphics 与 Tkinter 之间的关系，其中提到的 graphics 定义的各种图形类将在稍后介 绍。

> ① Python Programming: An Introduction to Computer Science 的作者 John Zelle。

![](img/程序设计思想与方法 167642.png)

图 5.23 在 Tkinter 之上开发的 graphics

graphics 模块和说明文档可以从下列网站下载：

[`mcsp.wartburg.edu/zelle/python`](http://mcsp.wartburg.edu/zelle/python)

下载后将 graphics.py 模块与你的图形程序放在一个目录中，或者放在 Python 安装目录 中即可。下面我们简要介绍如何使用 graphics 模块。

首先，需要导入 graphics 模块：

```
>>> from graphics import * 
```

其次，创建一个绘图窗口：

```
>>> win = GraphWin("My Graphics Window",300,200) 
```

这条语句的含义是在屏幕上创建一个窗口对象，窗口标题为"My Graphics Window"，宽 度为 300 像素，高度为 200 像素。三个参数都可以省略，缺省宽度和高度都是 200 像素。窗 口的坐标系仍然是我们熟悉的，即以窗口左上角为原点，x 轴向右，y 轴向下。

通过 Graphwin 类创建绘图窗口的界面实际上是对底层 Tkinter 中创建画布对象界面的重 新包装，也就是说，当程序员利用 graphics 模块创建绘图窗口时，系统会把这个请求向下转 达给 Tkinter 模块，而 Tkinter 模块就创建一个画布对象并返回给上层的 graphics 模块。这样 做不是没事找事多此一举，而是为了改善图形编程界面的易用性、易理解性。

接下去就可以在作图窗口中绘制图形了，稍后将介绍各种图形对象的创建方法。程序结 束后应该关闭图形窗口，为此只需向窗口对象发如下消息即可：

```
>>> win.close() 
```

下面介绍 graphics 模块支持的各种图形对象的用法。演示代码中总是假定已经导入了 graphics 模块并创建了绘图窗口 win。

点

graphics 模块提供了类型 Point 用于在窗口中画点。创建点对象的语句模式为：

```
>>> p = Point(<x 坐标>,<y 坐标>) 
```

下面通过一个交互过程来在窗口中创建 Point 对象，并演示 Point 对象的方法的使用。

```
>>> p = Point(100,80)
>>> p.draw(win)
>>> print p.getX(),p.getY() 100 80
>>> p.move(20,30)
>>> print p.getX(),p.getY() 120 110 
```

第一条语句创建了一个 Point 对象，该点的坐标为(100,80)，变量 p 被赋值为该对象。

这时在窗口中并没有显示这个点，因为还需要让这个点在窗口中画出来，为此只需向对象 p 发送消息 draw()，这就是第二条语句的目的，其意为“请求对象 p 执行 draw(win)方法， 即在窗口 win 中将自己画出来”。第三条语句演示了 Point 对象的另两个方法 getX()和 getY()的使用，分别是获得点的 x 坐标和 y 坐标。第四条语句的含义是请求 Point 对象 p 改变位置，向 x 方向移动 20 像素，向 y 方向移动 30 像素。

此外，Point 对象还提供以下方法：

*   p.setFill()：设置点 p 的颜色。

*   p.setOutline：设置轮廓线的颜色。对 Point 来说，与 setFill 没有区别。

*   p.undraw()：隐藏对象 p，即在窗口中变成不可见的。注意，隐藏并非删除，对象 p 仍然存在，随时可以重新执行 draw()。

*   p.clone()：复制。复制一个与 p 一模一样的对象。

读者一定会觉得通过 Point 类来画点非常容易，但也会奇怪：graphics 是建立在 Tkinter 之上的一层软件，graphics 的所有功能都是依赖于 Tkinter 的功能实现的，但是 Tkinter 中并未提供画点功能啊。对这个疑问的解答很简单：Point 对象其实是 Tkinter 中的一个很小的矩 形（参见图 5.23）！这是通过层次化改善图形编程界面的一个典型例子——当我们要画点时， 就直接创建 Point 对象，而不是像在 Tkinter 中那样很别扭地创建一个矩形。

接下去介绍的其他图形对象就不再像 Point 这样详细解释并演示用法了，希望使用 graphics 模块的读者可以自行练习。

直线

直线类型为 Line，创建直线对象的语句模式为：

```
>>> line = Line(&lt;端点 1&gt;,&lt;端点 2&gt;) 
```

其中两个端点都是 Point 对象。

和 Point 一样，Line 对象也支持 draw()、undraw()、move()、setFill()、setOutline()、clone()等方法。此外，Line 对象还支持 setArrow()方法，用于为直线画箭头。

圆形

圆形类型为 Circle，创建圆形对象的语句模式为：

```
>>> c = Circle(&lt;圆心&gt;,&lt;半径&gt;) 
```

其中圆心是 Point 对象，半径是个数值。

Circle 对象同样支持 draw()、undraw()、move()、setFill()、setOutline()、clone()等方法。此外，Circle 对象还支持 c.getRadius()方法，用于获取圆形对象 c 的半径。

椭圆

椭圆类型为 Oval，创建椭圆对象的语句模式为：

```
>>> o = Oval(&lt;左上角&gt;,&lt;右下角&gt;) 
```

其中左上角和右下角是两个 Point 对象，用于指定一个矩形，再由这个矩形定义一个内接椭圆。

椭圆对象同样支持 draw()、undraw()、move()、setFill()、setOutline()、 clone()等方法。

矩形

矩形类型为 Rectangle，创建矩形对象的语句模式为：

```
>>> r = Rectangle(&lt;左上角&gt;,&lt;右下角&gt;) 
```

其中左上角和右下角是两个 Point 对象，用于指定矩形。

矩形对象同样支持 draw()、undraw()、move()、setFill()、setOutline()、clone() 等方法。此外，矩形还支持的方法包括 r.getP1() 、 r.getP2() 和 r.getCenter()，分别用于获取左上角、右下角和中心，返回值都是 Point 对象。

多边形

多边形类型为 Polygon，创建多边形对象的语句模式为：

```
>>> poly = Polygon(&lt;顶点 1&gt;,..., &lt;顶点 n&gt;) 
```

将各顶点用直线相连，即成多边形。

矩形对象同样支持 draw()、undraw()、move()、setFill()、setOutline()、 clone()等方法。此外还支持方法 poly.getPoints()，用于获取多边形的各个顶点。

文本

文本类型为 Text，创建文本对象的语句模式为：

```
>>> t = Text(&lt;中心点&gt;,&lt;字符串&gt;) 
```

其中，中心点是个 Point 对象，字符串是显示的文本内容。

文本对象支持 draw()、undraw()、move()、setFill()、setOutline()、clone()等方法，其中 setFill()和 setOutline()方法都是设置文本的颜色。文本对象还支持方法 t.setText(<新字符串>)用于改变文本内容，方法 t.getText()用于获取文本内容，方法 t.setTextColor()用于设置文本颜色。

# 5.4.3 graphics 与面向对象

### 5.4.3 graphics 与面向对象

在 Tkinter 中，只为画布提供了类 Canvas，而画布上绘制的各种图形并没有对应的类。 因此画布是对象，而画布上的图形并不是对象，至少不是按面向对象风格构造的。graphics 模块就是为了改进这一点而设计的，它将 Tkinter 的绘图功能进行了全面的面向对象包装。 在 graphics 模块中，GraphWin、Point、Circle、Oval、Line、Text 和 Rectangle 等都是类，可以创建相应的对象。每个对象都是相应的类的实例，例如每个具体的“点”都 是 Point 的实例。所有点对象都具有自己的坐标值(x,y)，都支持 getX()、getY()和 draw()等方法（操作）。 为创建一个类的新实例，需要构造器（constructor）。调用构造器的语法模式如下：

```
<类名>(<参数 1>,<参数 2>, ...)
<变量名> = <类名>(<参数 1>,<参数 2>, ...) 
```

其中类名指定要创建什么样的实例，例如 Point 或 Circle；诸参数是对象初始化所需的信息，例如 Point 需要两个坐标作为参数，Circle 需要一个点（圆心）和一个数值（半径）作为参数。构造器创建对象后，通常需要将这个对象赋予某个变量，以便今后通过这个变量引用并操作对象。 我们来看一个例子：

```
p = Point(50,60) 
```

Point 构造器创建了一个点对象，变量 p 指向这个新创建的点对象。构造器的两个参数表 示点对象的 x 和 y 坐标，这两个值将存储在对象内部的实例变量（instance variable）中（图 5.24）。

![](img/程序设计思想与方法 171513.png)

图 5.24 Point 对象的创建 为了请求对象执行其内部定义的方法，需要向对象发消息。例如，对于点对象可以发送消息 p.getX()、p.getY()、p.move(dx,dy)等等。消息的一般形式如下：

```
<对象>.<方法名>(<方法参数 1>,<方法参数 2>, ...) 
```

有些对象的实例变量和方法的参数本身也可能是对象。例如，考虑如下语句：

```
>>> win = GraphWin()
>>> c = Circle(Point(100,100), 30)
>>> c.draw(win) 
```

上述语句的第一行创建 GraphWin 对象 win。第二行创建 Circle 对象 c，它的圆心是点 对象 Point(100,100），半径为 30。注意，Circle 构造器的第一个参数利用 Point 构 造器创建了圆心点对象。第三行请求 Circle 对象 c 执行它的 draw()方法。图 5.25 显示 了 GraphWin、Circle 和 Point 对象之间的相互关系。我们通常无需关心这些细节，而 只需要创建对象并调用对象的方法，对象自会完成任务，这就是面向对象编程的力量。

![](img/程序设计思想与方法 172014.png)

图 5.25 各种对象之间的关系

最后，我们用一个实例演示基于 graphics 模块的图形编程，读者可以自行比较它和 Tkinter 编程在风格上的异同。

【程序 5.4】sunmove.py

```
from graphics import * from time import sleep
def main():
    w = GraphWin("Demo",300,200)
    m1 = Polygon(Point(150,199),Point(200,100),Point(250,199)) 
    m1.setFill('green')
    m1.draw(w)
    m2 = Polygon(Point(200,199),Point(250,80),Point(350,199)) 
    m2.setFill('green')
    m2.draw(w)
    center = Point(0,100) 
    sun = Circle(center,10) 
    sun.setFill('red') 
    sun.draw(w)
    for i in range(31): 
        if i&lt;15:
            sun.move(10,-5) 
            center.move(10,-5)
        elif i&lt;20:
            sun.move(10,0) 
            center.move(10,0)
        else:
            sun.move(10,5) 
            center.move(10,5) 
            if i == 30:
                w.setBackground('black') 
        sleep(0.25)
    w.getMouse() 
    w.close()
main() 
```

本程序先创建图形窗口，再画两个多边形和一个圆形（表示两座山和太阳）。然后让圆 形不断移动：先向右上移动，再向右平移，最后向右下移动，显然这是太阳东升西落的模拟。 天黑后点击一下窗口即可关闭窗口结束程序。执行结果如图 5.26 所示。

![](img/程序设计思想与方法 172689.png)

图 5.26 程序 5.4 执行结果截图

# 5.5 练习

## 5.5 练习

1\. 在你的专业中，计算机图形编程可能有什么应用？

2\. 为什么说图形是复杂数据？

3\. 什么是对象？从你的专业中选择一个研究对象，用程序设计的对象概念来描述它，即列 出它的数据（属性）和操作（方法）。

4\. Tkinter 与 graphics 模块的关系是怎样的？

5\. 试试在画布上创建汉字文本。如果有乱码，请用汉字的 unicode 编码。

6\. 程序设计：画一个射箭运动所用的箭靶。从小到大分别为黄、红、蓝、黑、白色的同心 圆，每个环的宽度都等于黄色圆形的半径。

7\. 程序设计：绘制奥运五环旗。

8\. 程序设计：输入 r 和 y，以 r 为半径画一个圆，以 y 为截距画一条水平直线，然后计算直 线与圆的交点。

9\. 程序设计：绘制某个数学函数的曲线，例如正弦、余弦、指数函数等等。

10\. 程序设计：输入本金和利率，计算 10 年内每一年的本金加利息之和，并用柱状图显示。

11\. 程序设计：画一幅冬季景色，有雪人和圣诞树之类。

12\. 程序设计：将一圆周进行 n（例如 12 或 15）等分，然后用直线将所有等分点两两连接。