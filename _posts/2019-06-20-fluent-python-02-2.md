---
layout: article
title: 第2章 序列构成的数组(二)
tags:
  - python
  - 笔记
  - 《流畅的Python》
---

<!--more-->

### 2.4 切片

在切片和区间操作里不包含区间范围的最后一个元素是Python的风格

seq[start:stop:step]进行求值的时候，Python 会调用seq.__getitem__(slice(start, stop, step))  
> slice() 函数实现切片对象，主要用在切片操作函数里的参数传递。 
[Python3 内置函数整理：slice](/2019/03/10/python-functions.html#slice)

#### 多维切片

[] 运算符里还可以使用以逗号分开的多个索引或者是切片，外部库NumPy 里就用到了这个特性，二维的 numpy.ndarray 就可以用 a[i,
j] 这种形式来获取，抑或是用 a[m:n, k:l] 的方式来得到二维切片。 
Python 内置的序列类型都是一维的，因此它们只支持单一的索引。 
切片还可以用来就地修改可变序列，也就是说修改的时候不需要重新组建序列。 
> 要正确处理这种 [] 运算符的话，对
象的特殊方法 __getitem__ 和 __setitem__ 需要以元组的形式来接收a[i, j] 中的索引。也就是说，如果要得到 a[i, j] 的值，Python 会
调用 a.__getitem__((i, j))。

#### 省略
省略（ellipsis）的正确书写方法是三个英语句号（...）  
如果 x 是四维数组，那么 x[i, ...] 就是 x[i, :, :, :] 的缩写  
> 省略在 Python 解析器眼里是一个符号，而实际上它是 Ellipsis 对象的别名，而 Ellipsis 对象又是 ellipsis 类的单一实例。 
这些句法上的特性主要是为了支持用户自定义类或者扩展，比如NumPy就是个例子。

#### 切片赋值
如果把切片放在赋值语句的左边，或把它作为del操作的对象，就可以对序列进行嫁接、 切除或就地修改操作。  
如果赋值的对象是一个切片，那么赋值语句的右侧必须是个可迭代对象。

### 2.5 对序列使用+和*
+拼接后产生新对象；  
\*复制后进行拼接，如果是**引用对象**则只是复制引用；

### 2.6 序列的增量赋值
以+=举例，+=背后的特殊方法是 __iadd__ （用于“就地加法”）。但是如果一个类没有实现这个方法的话，Python 会退一步调用 __add__。 
变量名会不会被关联到新的对象，完全取决于这个类型有没有实现__iadd__ 这个方法。 
尽量不要把可变对象放在元组里面。 
> 对不可变序列进行重复拼接操作的话，效率会很低，因为每次都有一个新对象  
str 是一个例外，CPython对它做了优化。为str初始化内存的时候，程序会为它留出额外的可扩展空间，因此进行增量操作的时候，并不会涉及复制原有字符串到新位置这类操作。

> [Python Tutor](http://www.pythontutor.com) 是一个对 Python 运行原理进行可视化分析的工具。

### 2.7 list.sort方法和内置函数sorted
list.sort方法会就地排序列表，方法的返回值是None，就是说不会把原列表复制一份。 
> 在这种情况下返回 None 其实是 Python 的一个惯例：
如果一个函数或者方法对对象进行的是就地改动，那它就应该返回 None ，好让调用者知道传入的参数发生了变动，而且并未产生新的对象。

与 list.sort 相反的是内置函数 sorted ，它会新建一个列表作为返回值。这个方法可以接受任何形式的可迭代对象作为参数，甚至包括不可变序列或生成器。

list.sort和内置函数sorted都有两个可选的关键字参数：
* reverse：True 被排序的序列里的元素会以降序输出，默认值是False
* key：
一个只有一个参数的函数，这个函数会被用在序列里的每一个元素上，所产生的结果将是排序算法依赖的对比关键字。比如说，在对一些字符串排序时，可以用 key=str.lower 来实现忽略大小写的排序，或者是用 key=len 进行基于字符串长度的排序。这个参数的默认值是恒等函数（identity function），也就是默认用元素自己的值来排序。

> 可选参数 key 还可以在内置函数 min() 和 max() 中起作用。另外，还有些标准库里的函数也接受这个参数，像itertools.groupby() 和 heapq.nlargest() 等。

### 2.8 用bisect 来管理已排序的序列
bisect 模块包含两个主要函数，bisect和insort ，两个函数都利用二分查找算法来在有序序列中查找或插入元素。
> bisect(haystack, needle) 在 haystack （干草垛） 里搜索 eedle（针） 的位置，该位置满足的条件是，把 needle 入这个位置之后，haystack 还能保持升序。就是在说这个函数返回的位置前面的值，都小于或等于 needle 值。其中 haystack 必须是一个有序的序列。你可以先用 bisect(haystack, needle) 查找位置 index ，再用haystack.insert(index, needle) 来插入新值。也可用insort 来一步到位，并且后者的速度更快一些。

#### bisect.bisect
bisect.bisect==bisect.bisect_right
bisect.bisect_left(a, x[, lo[, hi]]) -> index
bisect.bisect_left(a, x[, lo[, hi]]) -> index
> bisect_left 返回的插入位置是原序列中跟被插入元素相等的元素的位置，也就是新元素会被放置于它相等的元素的前面，而 bisect_right 返回的则是跟它相等的元
素之后的位置。

根据一个分数，找到它所对应的成绩
```python
def grade(score, breakpoints=[60, 70, 80, 90], grades='FDCBA'):
    i = bisect.bisect(breakpoints, score)
    return grades[i]
print([grade(score) for score in [33, 99, 77, 70, 89, 90, 100]])
# result 
# ['F', 'A', 'C', 'C', 'B', 'A', 'A']
```

#### bisect.insort
排序很耗时，因此在得到一个有序序列之后，我们最好能够保持它的有序。bisect.insort 就是为了这个而存在的。 
bisect.insort==bisect.insort_right
bisect.insort_right(a, x[, lo[, hi]])
bisect.insort_left(a, x[, lo[, hi]])













