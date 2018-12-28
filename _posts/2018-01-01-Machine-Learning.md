---
layout: article
title: Google Machine Learning Recipes
tags:
  - python
lang: zh-Hans
---

<!--more-->

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-01-01.png)

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-01-02.png)

![](https://raw.githubusercontent.com/chen866/chen866.github.io/master/assets/images/2018-01-01-03.png)

无意中发现一篇16年9月的文章：
Google有三个.cn网站可以访问了，但它们只面向开发者

[developers.google.cn](http://developers.google.cn)

[firebase.google.cn](http://firebase.google.cn)

[developer.android.google.cn](http://developer.android.google.cn)


挨个进去看了看，第一个页面下边有一个面向中国开发者的，里面是优酷视频, 里面有几节关于机器学习的， 仔细一看还是入门篇：

[Google开发者的自频道-优酷视频](http://i.youku.com/i/UMjczOTc0NDkzNg==/custom?spm=a2hzp.8244740.0.0&id=87105)

开始学习...

第一节
Hello World - Machine Learning Recipes #1
使用到了scikit-learn开源框架
scikit-learn开源框架需要先安装numpy和scipy

pip安装`numpy`和`scipy`:
`python -m pip install --user numpy scipy`
只需要这两个所以没有安装后面官方给的` matplotlib ipython jupyter pandas sympy nose`

安装`scikit-learn`:
`pip install -U scikit-learn`

### 区分苹果和橙子
这是视频中给出的一个样例,第一个机器学习程序
> ps: 真的是我写的第一个机器学习程序 好激动啊

构造出一组训练数据:

| Weight | Texture | Label |
|:--------:|:--------:|:--------:|
| 150g | Bumpy | Orange |
| 170g | Bumpy | Orange |
| 140g | Smooth | Apple |
| 130g | Smooth | Apple |

在代码中以0和1来代表表面是否光滑，以0和1来代表橙子和苹果，python代码如下

```
from sklearn import tree
feature = [[140,1],[130,1],[150,0],[170,0]]
labels = [0,0,1,1]
#决策树 Decision Tree
clf=tree.DecisionTreeClassifier()
#fit 发现数据固有模式
clf=clf.fit(feature, labels)
print(clf.predict([[150,0]]))
```

结果输出为
`[1]`

第二节会讲到
`How is the tree created?`