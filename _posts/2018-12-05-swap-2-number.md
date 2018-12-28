---
layout: article
title: 不用中间变量交换两个数字变量
tags:
  - code
lang: zh-Hans
---

<!--more-->

```
if(A[j] <= A[i]){
    A[j] = A[j] + A[i];
    A[i] = A[j] - A[i];
    A[j] = A[j] - A[i];
}
```