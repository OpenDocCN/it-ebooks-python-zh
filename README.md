<!--
    需要填充的占位符：
    
    README.md
    
        计算机中文电子书：Python：文档中文名
        {nameEn}：文档英文名
        {urlEn}：文档原始链接
        iteb-py：域名前缀
        飞龙：负责人名称
        wizardforcel：负责人 Github 用户名
        562826179：负责人 QQ
        it-ebooks-python-zh：ApacheCN 的 Github 仓库名称
        it-ebooks-python-zh：DockerHub 仓库名称
        it-ebooks-python-zh：PYPI 包名称
        it-ebooks-python-zh：NPM 包名称
    
    CNAME
    
        iteb-py：域名前缀

    index.html
    
        计算机中文电子书：Python：文档中文名
        #b6459a：显示颜色
        it-ebooks-python-zh：ApacheCN 的 Github 仓库名称

    asset/docsify-flygon-footer.js
    
        it-ebooks-python-zh：ApacheCN 的 Github 仓库名称
-->

# 计算机中文电子书：Python

> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)
> 
> 真相一旦入眼，你就再也无法视而不见。——《黑客帝国》

* [在线阅读](https://iteb-py.flygon.net)

## 下载

### Docker

```
docker pull apachecn0/it-ebooks-python-zh
docker run -tid -p <port>:80 apachecn0/it-ebooks-python-zh
# 访问 http://localhost:{port} 查看文档
```

### NPM

```
npm install -g it-ebooks-python-zh
it-ebooks-python-zh <port>
# 访问 http://localhost:{port} 查看文档
```
