---
title: "python3.7及其module的安装--win7系统"
date: "2018-08-07"
tags:
- python
- module
- install
---
   
很早就买了本书《Python科学计算》，却一直未曾动过。直到前些天，看到报道说，python已占据编程语言之巅。人生时间有限，要学就学最好的。因此，打算开始学学python。不料win7系统安装python及其module，遇见了不少问题。本文将描述安装所遇到的问题及其解决方法。我在本人网盘^[本人网盘http://pan.scau.edu.cn/l/2nfiM5]里收录了本文在python软件及其安装遇到问题所需的全部文件。
<!--more-->

# python安装
长话短说，首先，安装python软件，简单，从[python官网](https://www.python.org/downloads/windows/)下载直接安装。安装时记得选`add to path`，作用是将python程序添加到path后，可以直接运行python。如果您懒得去官网，可到本文附录的本人网盘下载安装。

安装过程很顺利，但运行python，却出现了**<span style="color:red">提示api-ms-win-crt-process-l1-1-0.dll丢失</span>**，程序无法运行·，乖乖！人生第一次遇到软件顺利安装却无法运行的问题。

于是，[yahoo](https://www.yahoo.com/)和[百度](https://www.baidu.com/)搜索之，大部分的结果都说，系统里缺少`api-ms-win-crt-process-l1-1-0.dll`，解决方法就是下载该dll文件，复制到`C:\Windows\System32`（32位系统）或`C:\Windows\SysWOW64`（64位系统），然后再进行注册^[https://blog.csdn.net/gb4215287/article/details/78247568]。

但实践结果，上述方法无效！即从网络下载`api-ms-win-crt-process-l1-1-0.dll`，无法解决
python的运行失败问题。

但这个帖子^[https://blog.csdn.net/gangeqian2/article/details/79307416]提供了正确的解决方法。原因是python依赖`Windows通用C运行库`，因此安装windows相关的更新`KB2999226（10.0.10240.16390）或KB3118401（10.0.10586.9）`，问题得以解决！这两个更新已含在本文附录的本人网盘里。

看来，python比R娇气，软件安装都要费一番功夫。

# module安装
python现在可以运行了，但问题又出现了，我想要安装module `numpy`，按官网方法^[https://pip.pypa.io/en/stable/installing/]尝试多次，失败告终！不像R，通过命令 `install.packages()`即可完成程序包的安装。python再次娇气！

于是，再次[百度](https://www.baidu.com/)搜索之。得到module的安装方法有多种方法，但似乎仍然无效。于是，我按最笨的方法来处理。

首先，下载[setuptools](https://files.pythonhosted.org/packages/d3/3e/1d74cdcb393b68ab9ee18d78c11ae6df8447099f55fe86ee842f9c5b166c/setuptools-40.0.0.zip)，然后解压到`C:\Python\Python37-32\Scripts`，文件夹名去掉版本号，即文件夹名为‘setuptools’。

现在建议按帖子^[https://jingyan.baidu.com/article/3f16e003c408142591c103b2.html]将命令提示符加到鼠标右键。方法很简单，就是将下述代码存到名为CMD.reg的文件里，再运行CMD.reg。

```r
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\folder\shell\cmd]
@="命令提示符"
[HKEY_CLASSES_ROOT\folder\shell\cmd\command]
@="cmd.exe /k cd %1"
```

回到之前解压后的‘setuptools’文件夹，右击鼠标，选择‘命令提示符’，进入CMD模式，输入下述代码进行setuptools的安装：

```r
python setup.py install
```

同理，下载[pip](https://files.pythonhosted.org/packages/69/81/52b68d0a4de760a2f1979b0931ba7889202f302072cc7a0d614211bc7579/pip-18.0.tar.gz)，按同样的方法安装pip。

现在就可以通过pip来轻松安装所需的module `numpy`，方法如下：

```r
pip.exe install numpy
```

但另一个问题是，默认的module是在外国网站，速度超慢！于是再次[百度](https://www.baidu.com/)搜索之。采用国内的python镜像站，按此贴子^[https://www.cnblogs.com/wqpkita/p/7248525.html]操作。其实，方法非常简单，就是将下述内容创建名为pip.ini的文件里，然后复制到`C:\Users\yzhlin\pip`里(不同电脑路径稍有区别，只要找到电脑用户里的pip文件夹即可)。

```r
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```

现在就可以通过pip通过国内镜像站来轻松安装所需的module `numpy`，方法如下：

```r
pip.exe install numpy
```    

乖乖，兜了好大一圈，以为可以轻松安装module，没想到问题又出现了，我想安装作图module 
`matplotlib`，按之前的安装方法如下：

```r
pip.exe install matplotlib
```    

不料安装失败了！出现了**<span style="color:red">`Microsoft Visual C++ 14.0 is required`</span>**的告示。我就差爆粗口了！python实在是娇气！只好再[yahoo](https://www.yahoo.com/)搜之。这个帖子^[https://www.scivision.co/python-windows-visual-c++-14-required/]给出了解决方法。VC14.0在附录的本人网络里也提供了。

装完VC14.0后，发现C盘只剩1G了。一查，原来VC14.0竟然3G多，大爷的，真占地盘！无奈之下，只能卸掉SAS，给VC让道！

# 安装spyder IDE
现在给python安装spyder IDE，之前的方法可用：

```r
pip.exe install spyder
``` 
安装成功后，输入spyder3即可使用spyder IDE：

```r
spyder3
``` 

# 结语
在python及其module安装过程中出现的种种问题，我只想说，python有点操蛋！太折腾人！软件与module的安装，应该像R学习！要让用户简单上手，而非一遍遍得检索与尝试！

# 参考文献

