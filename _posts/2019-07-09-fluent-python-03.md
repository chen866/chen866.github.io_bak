---
layout: article
title: 第3章 字典和集合(一)
tags:
  - python
  - 笔记
  - 《流畅的Python》
---

<!--more-->

### 3.1 泛映射类型

#### 什么是可散列的数据类型
如果一个对象是可散列的， 那么在这个对象的生命周期中， 它的散列值是不变的， 而且这个对象需要实现__hash__() 方法。 另外可散列对象还要有__qe__() 方法， 这样才能跟其他键做比较。 如果两个可散列对象是相等的， 那么它们的散列值一定是一样的……  
原子不可变数据类型（str、bytes 和数值类型）都是可散列类型， frozenset 也是可散列的， 因为根据其定义， frozenset 里只能容纳可散列类型。 元组的话， 只有当一个元组包含的所有元素都是可散列类型的情况下， 它才是可散列的。

一般来讲用户自定义的类型的对象都是可散列的， 散列值就是它们的 id() 函数的返回值， 所以所有这些对象在比较的时候都是不相等的。 如果一个对象实现了__eq__方法， 并且在方法中用到了这个对象的内部状态的话， 那么只有当所有这些内部状态都是不可变的情况下， 这个对象才是可散列的。

例 创建字典的不同方式：
```python
a = dict(one=1, two=2, three=3)
b = {'one': 1, 'two': 2, 'three': 3}
c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
d = dict([('two', 2), ('one', 1), ('three', 3)])
e = dict({'three': 3, 'one': 1, 'two': 2})
a == b == c == d == e
#True
```

### 3.2 字典推导
字典推导（dictcomp） 可以从任何以键值对作为元素的可迭代对象中构建出字典。 

字典推导的使用
```python
DIAL_CODES = [
	(86, 'China'),
	(91, 'India'),
	(1, 'United States'),
	(62, 'Indonesia'),
	(55, 'Brazil'),
	(92, 'Pakistan'),
	(880, 'Bangladesh'),
	(234, 'Nigeria'),
	(7, 'Russia'),
	(81, 'Japan'),
	]
# 构建字典
country_code = {country: code for code, country in DIAL_CODES}
print(country_code)
#{'China': 86, 'India': 91, 'Bangladesh': 880, 'United States': 1,'Pakistan': 92, 'Japan': 81, 'Russia': 7, 'Brazil': 55, 'Nigeria':234, 'Indonesia': 62}
# 颠倒字典的键、值关系 筛选
print({code: country.upper() for country, code in country_code.items() if code < 66})
#{1: 'UNITED STATES', 55: 'BRAZIL', 62: 'INDONESIA', 7: 'RUSSIA'}
```

### 3.3 常见的映射方法

#### Reading...