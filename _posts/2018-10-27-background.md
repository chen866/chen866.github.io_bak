---
layout: post
title: 基于 html5 canvas 绘制的网页线条特效
tags:
  - code
lang: zh-Hans
---

<!--more-->

在laod博客上发现一个很有趣的网页特效，找了下项目地址，分享给大家。  
这是一个基于 html5 canvas 绘制的网页背景效果，在很多网站上都可以看得到，极易使用，已经应用。

项目链接：[canvas-nest.js](https://github.com/hustcc/canvas-nest.js)

## 特性

 - 不依赖 jQuery，使用原生的 javascript。
 - 非常小，只有 2 Kb。
 - 非常容易实现，配置简单，即使你不是 web 开发者，也能简单搞定。
 - 模块化 & 区域渲染。

## 使用

 - 快捷使用

将下面的代码插入到 `<body> 和 </body> 之间`.

```html
<script type="text/javascript" src="dist/canvas-nest.js"></script>
```

强烈建议在 `</body>`标签上方. 例如下面的代码结构:

```html
<html>
<head>
    ...
</head>
<body>
    ...
    ...
    <script type="text/javascript" src="dist/canvas-nest.js"></script>
</body>
</html>
```

然后就完成了，打开网页即可看到效果!`请注意不要将代码置于 <head> </head>里面`.**敲黑板**

## 配置

 - **`color`**: 线条颜色, 默认: `'0,0,0'` ；三个数字分别为(R,G,B)，注意用,分割
 - **`opacity`**: 线条透明度（0~1）, 默认: `0.5`
 - **`count`**: 线条的总数量, 默认: `150`
 - **`zIndex`**: 背景的z-index属性，css属性用于控制所在层的位置, 默认: `-1`

Example:

 - 快捷使用

```html
<script type="text/javascript" color="0,0,255" opacity='0.7' zIndex="-2" count="99" src="dist/canvas-nest.js"></script>
```

 - 模块化区域绘制（定制开发）

```js
{
  color: '0,0,255',
  opacity: 0.7,
  zIndex: -2,
  count: 99,
};
```

**注意: 所有的配置项都有默认值，如果你不知道怎么设置，可以先不设置这些配置项，就使用默认值看看效果也可以的。**
