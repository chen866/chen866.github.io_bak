---
layout: article
title: Python运行环境搭建
tags:
  - python
lang: zh-Hans
---

学习python半年多了，前几天做了一个winsows  python环境ppt，现在发出来以后抛链接用

<!--more-->

首先，从[`python官网`](https://www.python.org)下载自己平台适用的安装包并安装，推荐使用迅雷下载，浏览器下载较慢

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-01.png)

默认安装会安装到用户目录中，不推荐这么做。  
推荐做法是：勾选最下面的“Add python to Path”，免去我们手动配置环境变量，然后点击"Customize"进行自定义安装。

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-02.png)

选择安装位置和一些非必选项，点击安装。

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-03.png)

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-04.png)

安装以后不要忽略了点击红框中的选项，去除Path的长度限制，点击"Close"Python环境就安装好了

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-05.png)

在命令行中输入“python”，如果提示版本信息并出现“>>>”说明python环境成功安装；  
在命令行中输入“pip”，提示帮助信息说明pip也成功安装了。

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-06.png)

> pip 是一个现代的，通用的 Python 包管理工具  。提供了对Python 包的查找、下载、安装、卸载的功能。

示例：  
```python
pip install requests  # 安装requests
pip uninstall requests  # 卸载requests
pip search xml  # 搜索XML相关
pip show beautifulsoup4  # 显示已安装库
```

Pip的默认下载源在国外，下载软件包的时候比较慢，国内阿里、豆瓣、科大等很多都有镜像服务器，我们可以在用户目录下手动增加`pip.ini`配置文件的方式修改源，来提高下载速度，这里我用的是豆瓣的源。

```xml
[global]
index-url = https://pypi.doubanio.com/simple/
[install]
trusted-host=pypi.doubanio.com
```

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-07.png)

演示一下pip的卸载requests库和安装requests库

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-07-17-08.png)
