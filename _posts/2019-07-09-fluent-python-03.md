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

字典推导的使用，和列表推导基本一致：
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
用setdefault 处理找不到的键：`d.get(k, default)`  
更新某个键对应的值的时候：`d.setdefault(k, [])`  
```python
if key not in my_dict:
my_dict[key] = []
my_dict[key].append(new_value)
```
等同写法，
```python
my_dict.setdefault(key, []).append(new_value)
```

二者的效果是一样的， 只不过后者至少要进行两次键查询——如果键不存在的话， 就是三次， 用 setdefault 只需要一次就可以完成整个操作。

### 3.4 映射的弹性键查询
defaultdict：处理找不到的键的一个选择
> collections.defaultdict

使用方法：
```python
from collections import defaultdicr=t
dd = defaultdict(list) 
dd[key].append(new_value)
```
需要注意的点：
1. 用来生成默认值的可调用对象存放在名为 default_factory 的实例属性里
2. 如果在创建 defaultdict 的时候没有指定 default_factory，查询不存在的键会触发 KeyError
3. defaultdict 里的 default_factory 只会在 \_\_getitem\_\_ 里被调用，即dd[k] 查找不到key会自动创建，而 dd.get(k) 则会返回 None (特殊方法\_\_missing\_\_，该方法适用于所有映射对象)

### 3.5 字典的变种

- collections.OrderedDict  
在添加键的时候会保持顺序，popitem 方法默认删除并返回字典里的最后一个元素，加入last=False参数后删除并但会第一个元素
- collections.ChainMap  
容纳数个不同的映射对象， 然后在进行键查找操作的时候， 这些对象会被当作一个整体被逐个查找， 直到键被找到为止
- collections.Counter  
这个映射类型会给键准备一个整数计数器。 每次更新一个键的时候都会增加这个计数器。  
    ```python
    >>> ct = collections.Counter('abracadabra')
    >>> ct
    Counter({'a': 5, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
    >>> ct.update('aaaaazzz')
    >>> ct
    Counter({'a': 10, 'z': 3, 'b': 2, 'r': 2, 'c': 1, 'd': 1})
    >>> ct.most_common(2)
    [('a', 10), ('z', 3)]
    ```
- colllections.UserDict  
子类化UserDict








