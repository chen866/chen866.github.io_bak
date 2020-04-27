---
layout: article
title: js插入的中文乱码
tags:
  - js
  - nginx
---
<!--more-->
昨晚把静态页面中的javascript单独提取出来作为单独的js文件部署后没有再检查一遍(😭)，第二天就看到js填充的中文文本都乱码了，记录下解决过程。

1. 先检查页面，查看浏览器调试面板中的`network`中的js文件请求，看到请求的js文件中的中文是乱码的；  
2. 直接运行django项目查看，看到页面没有乱码，js文件也没有乱码，但是`network`中js文件的`preview`里面的中文是乱码的，确认下js文件是utf-8编码，script标签添加'charset'解决；  
```html
<script charset="utf-8" src="XXX" type="text/javascript"></script>
```
3. 此时页面还是乱码，应该是nginx的原因，查看nginx的配置文件；  
`server`配置下面的`charset`设置的是`gbk,utf8`，修改为`utf8`后重新部署，乱码问题解决。

> 中间搜索资料看到网上有人说需要把`sendfile`设置成`off`，查了下感觉莫名其妙，文件传输方式不同不应该会导致乱码，试了下果然不是这个的原因。。。[nginx中配置sendfile及详细说明](https://www.jianshu.com/p/70e1c396c320?utm_campaign)  
> 快捷键：Ctrl+F5->硬性重新加载；F5->正常重新加载；  
