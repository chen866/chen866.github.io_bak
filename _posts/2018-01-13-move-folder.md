---
layout: post
title: 日常重装电脑系统
tags:
  - 搞机
lang: zh-Hans
---

以及 移动User文件夹到其他位置

<!--more-->

昨天看到[俄罗斯大神](https://tieba.baidu.com/f?kw=%B6%ED%C2%DE%CB%B9%B4%F3%C9%F1%CF%B5%CD%B3&fr=ala0&tpl=5)1月2号更新系统了，实在是受不了VS的占地面积，下午没事的时候就下载下来准备更新（重装）一下系统。

先下载了`ISO系统镜像`2.3G左右，备份一下`环境变量`，`user文件夹`，`驱动`，以及`U盘里的文件`

把ISO系统镜像用软碟通刻录到`格式化好的U盘`上，重启电脑，确认电脑为UEFI启动，然后从U盘启动

把原有的C盘分区删除，选择删除后的未分配区域安装系统

进入系统后，先用KMS激活系统，然后去磁盘管理里面把乱掉的驱动器号改回来，在`个性化->桌面图标`里面把`此电脑`打勾

因为必须要用到VS，担心装不好，所以先装的VS。离线安装了两次都没成功，联网、把VS主程序卸载，第三次安装成功，接着跳回正常的流程

VS自带超多的VC运行库所以没单独再装，用驱动精灵恢复了驱动，配置`jdk`、`python`、`temp`等环境变量，安装像`QQ`、`chrome`、`Office`等等的“非绿色版应用”，把`桌面`、`图片`、`文档`、`下载`文件夹的位置设置到D盘

平时的时候基本到这里就结束了，想起前些天转移`USERS`文件夹失败的经历，我又默默打开了百度。。。

看了好几篇文章，我发现上次主要是由于文件被占用的问题，有两篇文章一个采用Administrator用户解决，一个采用恢复模式户解决

> [恢复模式Win10移动User文件夹到其他位置](http://blog.csdn.net/CrowNAir/article/details/78533051)

这篇文章用了三个命令移动User文件夹

> `xcopy` 复制文件夹
> `ren` 重命名文件夹以防不测
> `mklink` 建立映射

亲手操作，把`Administrator`账号启用，注销当前用户，切换至`Administrator`登陆：

1.复制文件夹

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_01.png)

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_02.png)

2.重命名原文件夹

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_03.png)

3.建立映射

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_04.png)

4.顺便把权限修改下

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_05.png)

5.记得把`Administrator`用户禁用

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_06.png)

重启了下发现没问题就把重命名后的文件夹删除了，完美收工

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-13_07.png)