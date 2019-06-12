---
layout: article
title: 《流畅的Python》笔记 第2章 序列构成的数组(一)
tags:
  - python
---

<!--more-->

### 第 2 章 序列构成的数组

### 2.1 内置序列类型概览

在创造 Python 以前， Guido 曾为 ABC 语言贡献过代码。 ABC 语言是一个致力于为初学者设计编程环境的长达 10 年的研究项目， 其中很多点子在现在看来都很有 Python 风格： 序列的泛型操作、 内置的元组和映射类型、 用缩进来架构的源码、 无需变量声明的强类型， 等等。 Python对开发者如此友好， 根源就在这里。

Python 也从 ABC 那里继承了用统一的风格去处理序列数据这一特点。不管是哪种数据结构， 字符串、 列表、 字节序列、 数组、 XML 元素，抑或是数据库查询结果， 它们都共用一套丰富的操作： 迭代、 切片、 排序， 还有拼接。

深入理解 Python 中的不同序列类型， 不但能让我们避免重新发明轮子， 它们的 API 还能帮助我们把自己定义的 API 设计得跟原生的序列一样， 或者是跟未来可能出现的序列类型保持兼容。

### 2.1 内置序列类型概览

Python 标准库用 C 实现了丰富的序列类型， 列举如下。
* 容器序列  
list 、 tuple 和 collections.deque 这些序列能存放不同类型
的数据。  
* 扁平序列  
str 、 bytes 、 bytearray 、 memoryview 和 array.array ， 这类序列只能容纳一种类型。   
* 可变序列  
list 、 bytearray 、 array.array 、 collections.deque 和
memoryview 。
* 不可变序列  
tuple 、 str 和 bytes 。

> 容器序列  
存放的是它们所包含的任意类型的对象的引用， 而扁平序列
里存放的是值而不是引用。 换句话说， 扁平序列其实是一段连续的内存
空间。 由此可见扁平序列其实更加紧凑， 但是它里面只能存放诸如字
符、 字节和数值这种基础类型。  
序列类型还能按照能否被修改来分为`可变序列`和`不可变序列`。 

### 2.2 列表推导和生成器表达式

列表推导是构建列表（list ） 的快捷方式， 而生成器表达式则可以用来创建其他任何类型的序列。

```python
symbols = '$¢£¥€¤'
codes = []

# 把一个字符串变成 Unicode 码位的列表
for symbol in symbols:
    codes.append(ord(symbol))
print(codes)

# 使用列表推导式完成
codes = [ord(symbol) for symbol in symbols]
print(codes)
```

为了避免列表推导被滥用，通常的原则是，只用列表推导来创建新的列表，并且尽量保持简短。如果列表推导的代码超过了两行，可能就要考虑是不是得用 for 循环重写了。 

Python 会忽略代码里 `[]` 、 `{}` 和 `()` 中的换行， 因此如果你的代码里有多行的列表、 列表推导、 生成器表达式、 字典这一类的， 可以省略不太好看的续行符 `\` 。

在python3中，列表推导不会再有变量泄漏的问题，列表推导式中单独作为一个作用域。

用列表推导和 `map`/`filter` 组合来创建同样的表单  
```python
symbols = '$¢£¥€¤'
beyond_ascii = [ord(s) for s in symbols if ord(s) > 127]
print(beyond_ascii)
beyond_ascii = list(filter(lambda c: c > 127, map(ord, symbols)))
print(beyond_ascii)
```
> 作者的[速度测试代码](https://github.com/fluentpython/example-code/blob/master/02-array-seq/listcomp_speed.py)

使用列表推导计算笛卡儿积
```python
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
tshirts = [(color, size) for color in colors for size in sizes]
print(tshirts)

for color in colors: 
    for size in sizes:
        print((color, size))

tshirts = [(color, size) for size in sizes for color in colors]
print(tshirts)
```

---

列表推导的作用只有一个： 生成列表。如果想生成其他类型的序列，生成器表达式就派上了用场。

生成器表达式背后遵守了迭代器协议，可以逐个地产出元素，而不是先建立一个完整的列表，然后再把这个列表传递到某个构造函数里。

> 计算机为数组分配一段连续的内存，从而支持对数组随机访问；
由于项的地址在编号上是连续的，数组某一项的地址可以通过将两个值相加得出，即将数组的基本地址和项的偏移地址相加。
数组的基本地址就是数组的第一项的机器地址。一个项的偏移地址就等于它的索引乘以数组的一个项所需要的内存单元数目的一个常量表示（在python中，这个值总是1）

> array模块是python中实现的一种高效的数组存储类型。它和list相似，但是所有的数组成员必须是同一种类型，在创建数组的时候，就确定了数组的类型

用生成器表达式初始化元组和数组
```python
symbols = '$¢£¥€¤'
print(tuple(ord(symbol) for symbol in symbols))

from array import array
print(array('I', (ord(symbol) for symbol in symbols)))
```

使用生成器表达式计算笛卡儿积
```python
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes):
    print(tshirt)
```

### 2.3 元组不仅仅是不可变的列表

元组其实是对数据的记录： 元组中的每个元素都存放了记录中一个字段的数据， 外加这个字段的位置。 正是这个位置信息给数据赋予了意义。

#### 元组拆包

`%`格式运算符能被匹配到对应的元组元素上。  
for 循环可以分别提取元组里的元素， 也叫作拆包（unpacking）。  
在此过程中可以使用`_`、`*`作为占位符。  
元组拆包可以应用到任何可迭代对象上， 唯一的硬性要求是， 被可迭代对象中的元素数量必须要跟接受这些元素的元组的空档数一致。除非我们用 `*` 来表示忽略多余的元素。也称作`可迭代元素拆包`

```python
traveler_ids = [('USA', '31195855'), ('BRA', 'CE342567'), ('ESP', 'XDA205856')]
for passport in sorted(traveler_ids):
    print('%s/%s' % passport)
```

一个很优雅的写法当属不使用中间变量交换两个变量的值
```python
>>> b, a = a, b
```

把一个可迭代对象拆开作为函数的参数
```python
>>> divmod(20, 8)
(2, 4)
>>> t = (20, 8)
>>> divmod(*t)
(2, 4)
>>> quotient, remainder = divmod(*t)
>>> quotient, remainder
(2, 4)
```

> divmod(a, b) = (a // b, a % b)

**元组拆包的用法则是让一个函数可以用元组的形式返回多个值， 然后调用函数的代码就能轻松地接受这些返回值**

#### 平行赋值

最好辨认的元组拆包形式就是平行赋值，即 把一个可迭代对象里的元素， 一并赋值到由对应的变量组成的元组中

```python
>>> a, b, *rest = range(5)
>>> a, b, rest
(0, 1, [2, 3, 4])
>>> a, b, *rest = range(3)
>>> a, b, rest
(0, 1, [2])
>>> a, b, *rest = range(2)
>>> a, b, rest
(0, 1, [])
```

> 在平行赋值中， * 前缀只能用在一个变量名前面， 但是这个变量可以出现在赋值表达式的任意位置：

#### 嵌套元组拆包

接受表达式的元组可以是嵌套式的，例如 (a, b, (c, d))。只要这个接受元组的嵌套结构符合表达式本身的嵌套结构，Python就可以作出正确的对应。

#### 具名元组

collections.namedtuple 是一个工厂函数， 它可以用来构建一个带字段名的元组和一个有名字的类——这个带名字的类对调试程序有很大帮助。

定义和使用具名元组
```python
>>> from collections import namedtuple
>>> City = namedtuple('City', 'name country population coordinates')
>>> tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))
>>> tokyo
City(name='Tokyo', country='JP', population=36.933, coordinates=(35.689722,
139.691667))
>>> tokyo.population
36.933
>>> tokyo.coordinates
(35.689722, 139.691667)
>>> tokyo[1]
'JP'
```
> 创建一个具名元组需要两个参数， 一个是类名， 另一个是类的各个字段的名字  
存放在对应字段里的数据要以一串参数的形式传入到构造函数中  
可以通过字段名或者位置来获取一个字段的信息

**常用方法:**  
_fields 属性是一个包含这个类所有字段名称的元组  
用 _make() 通过接受一个可迭代对象来生成这个类的一个实例， 它的作用跟 City(*delhi_data) 是一样的  
asdict() 把具名元组以 collections.OrderedDict 的形式返回， 我们可以利用它来把元组里的信息友好地呈现出来  

#### 作为不可变列表的元组

元组和列表的区别是：元组不支持改变操作





