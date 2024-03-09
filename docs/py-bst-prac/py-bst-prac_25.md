### 导航

*   索引
*   下一页 |
*   上一页 |
*   Python 最佳实践指南 »

# 图像处理

多数图像处理与操作技术可以被两个库有效完成，它们是 Python Imaging Library (PIL)与 OpenSource Computer Vision (OpenCV)。

下面是这两个库的简略介绍。

## Python 图形库

> [Python Imaging Library](http://www.pythonware.com/products/pil/) [http://www.pythonware.com/products/pil/] ，或者叫 PIL，简略来说， 是 Python 图像操作的核心库。不幸的是，它的开发陷入了停滞，最后一次更新是 2009 年。
> 
> 对你而言幸运的是，存在一个活跃的 PIL 开发分支，叫做 [Pillow](http://python-pillow.github.io/) [http://python-pillow.github.io/] 它很容易安装，运行在各个操作系统上，而且支持 Python3。

### 安装

在安装 Pillow 之前，你应该先安装 Pillow 的前置部分。针对你的平台对此的特别指导可以在此找到 [Pillow installation instructions](https://pillow.readthedocs.org/en/3.0.0/installation.html) [https://pillow.readthedocs.org/en/3.0.0/installation.html].

完成之后，直接执行：

```
$ pip install Pillow 
```

### 例子

```
from PIL import Image, ImageFilter
#读取图像
im = Image.open( 'image.jpg' )
#显示图像
im.show()

#过滤图像
im_sharp = im.filter( ImageFilter.SHARPEN )
#保存过滤过的图像到文件中
im_sharp.save( 'image_sharpened.jpg', 'JPEG' )

#分解图像到三个 RGB 不同的通道（band）中。
r,g,b = im_sharp.split()

#显示被插入到图像中的 EXIF 标记
exif_data = im._getexif()
exif_data 
```

这里有一些 Pillow 库的例子： [Pillow tutorial](http://pillow.readthedocs.org/en/3.0.x/handbook/tutorial.html) [http://pillow.readthedocs.org/en/3.0.x/handbook/tutorial.html]。

## 开源计算机视觉（OpenCv）

OpenSource Computer Vision,其更广为人知的名字是 OpenCv，是一个在图像操作与处理上 比 PIL 更先进的库。它可以在很多语言上被执行并被广泛使用。

### 安装

在 Python 中，使用 OpenCV 进行图像处理是通过使用 `cv2` 与 `NumPy` 模块进行的。

[installation instructions for OpenCV](http://docs.opencv.org/2.4/doc/tutorials/introduction/table_of_content_introduction/table_of_content_introduction.html#table-of-content-introduction) [http://docs.opencv.org/2.4/doc/tutorials/introduction/table_of_content_introduction/table_of_content_introduction.html#table-of-content-introduction]

可以指导你如何为你自己的项目进行配置。

NumPy 可以从 Python Package Index （PyPI）中下载：

```
$ pip install numpy 
```

### 例子

```
from cv2 import *
import numpy as np
#读取图像
img = cv2.imread('testimg.jpg')
#显示图像
cv2.imshow('image',img)
cv2.waitKey(0)
cv2.destroyAllWindows()

#Applying Grayscale filter to image 作用 Grayscale（灰度）过滤器到图像上
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

#保存过滤过的图像到新文件中
cv2.imwrite('graytest.jpg',gray) 
```

更多的 OpenCV 在 Python 运行例子在这里可以找到： [collection of tutorials](http://opencv-python-tutroals.readthedocs.org/en/latest/py_tutorials/py_tutorials.html) [http://opencv-python-tutroals.readthedocs.org/en/latest/py_tutorials/py_tutorials.html].

© 版权所有 2014\. A <a href="http://kennethreitz.com/pages/open-projects.html">Kenneth Reitz</a> 工程。 <a href="http://creativecommons.org/licenses/by-nc-sa/3.0/"> Creative Commons Share-Alike 3.0</a>.