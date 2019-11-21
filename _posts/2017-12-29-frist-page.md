---
layout: article
title: Hello, Github Pages
tags:
  - code
lang: zh-Hans
---

我的第一篇`github pages`文章
<!--more-->

## github pages是什么?  

`github Pages`可以被认为是用户编写的、托管在github上的静态网页

历经九九八十一难千辛万苦众多波折以后, `github pages`终于测试通过了
 网上又各种各样的github pages教程, 看了N多个教程以后,才开始弄
## 1.首先,要有一个`github`账号
嗯, 我已经有了, 下一个

## 2.建一个名称为`username.github.io`的项目
文章里出现的`username`均为自己的`github`用户名

## 3.在项目目录里添加`index.html`文件,打开`username.github.io`网址进行测试
测试可以打开，显示`index.html`里面的内容
## 4.套用模板
单单这样是可以显示网页了, 可是毕竟...嗯...有点丑了, 即使作为一个程序员也没那么在意美丑, 可是它的确是太丑了, 挑选了一番，我选择用jekyll模板。
我fork了`kitian616`的[jekyll-TeXt-theme](https://github.com/kitian616/jekyll-TeXt-theme)主题。
> 稍加改动就ok啦  

按照教程装完环境后的快捷启动命令，可以在本地进行预览（需要ruby环境或Docker->[安装开发环境](https://tianqi.name/jekyll-TeXt-theme/docs/zh/quick-start#%E5%AE%89%E8%A3%85%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)）：  
```
bundle exec jekyll serve
```

## 5.解析域名到**github pages**
如果想使用自己的域名打开Github Pages的话：
1. 在域名提供商的域名管理中 CNAME解析到`username.github.io`  
或 A解析到
`192.30.252.153`
`192.30.252.154`
2. 在项目的根目录下中创建一个名为`CNAME`的文件，里边的内容填你的自定义域名（不需要加`http://`、`https://`、`/`这些）
    > 官方教程->[Troubleshooting custom domains](https://help.github.com/articles/troubleshooting-custom-domains/)

## 6.完结撒花
