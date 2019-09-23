---
layout: article
title: 不用中间变量交换两个数字变量
tags:
  - code
lang: zh-Hans
---

<!--more-->

C#或其他：  
```java
int a = 3, b = 5;

if(a <= b){
    a = a + b;
    b = a - b;
    a = a - b;
}
```

Python:  
```python
a, b = 3, 5

a, b = b, a
```