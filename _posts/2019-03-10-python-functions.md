---
layout: article
title: Python3 内置函数整理
tags:
  - python
---

<!--more-->

> 整理自[菜鸟教程](http://www.runoob.com/)的[Python3 内置函数](http://www.runoob.com/python3/python3-built-in-functions.html)

最近学习的劲头没有那么去年那么高了，生活的琐事也多了，不过兴趣还在。  
很多坑还没看完，又挖了几个新坑，看《流畅的Python》发现好几个Python的内置函数之前都没仔细留意过，整理下附带着过一遍，get了很多新姿势。

### 常用
#### print
print() 方法用于打印输出，最常见的一个函数。
````python
print(*objects, sep=' ', end='\n', file=sys.stdout)
````
[more](http://www.runoob.com/python/python-func-print.html)

#### len
len() 方法返回对象（字符、列表、元组等）长度或项目个数。
````python
len(s)
````

#### input
input() 函数接受一个标准输入数据，返回为 string 类型。注意：在 Python3.x 中 raw_input() 和 input() 进行了整合，去除了 raw_input( )，仅保留了input( )函数，其接收任意任性输入，将所有输入默认为字符串处理，并返回字符串类型。
````python
input([prompt])
````

#### range
range() 函数可创建一个整数列表，一般用在 for 循环中。函数返回的是一个可迭代对象（类型是对象），而不是列表类型。
````python
range(stop)
range(start, stop[, step])
````

#### help
help() 函数用于查看函数或模块用途的详细说明。
````python
help([object])
````

### 数值相关

#### abs
abs() 函数返回数字的绝对值。
````python
abs(x)
````

#### min
min() 方法返回给定参数的最小值，参数可以为序列。
````python
min(x, y, z, ....)
````

#### max
max() 方法返回给定参数的最大值，参数可以为序列。
````python
max(x, y, z, ....)
````

#### round
round() 方法返回浮点数x的四舍五入值。x 数字表达式；n  表示从小数点位数，其中 x 需要四舍五入，默认值为 0。
````python
round(x [, n])
````

#### hex
hex() 函数用于将一个指定数字转换为 16 进制数。
````python
hex(x)
````

#### oct
oct() 函数将一个整数转换成8进制字符串。
````python
oct(x)
````

#### pow
pow() 方法返回x的y次方的值，对z取模。函数是计算x的y次方，如果z在存在，则再对结果进行取模，其结果等效于pow(x,y) %z。
````python
pow(x, y[, z])
````

#### math.pow
math.pow() 方法返回x的y次方的值，对z取模。math模块。内置方法会把参数作为整型，而 math 模块则会把参数转换为 float。
````python
math.pow(x, y)
````

#### bin
bin() 返回一个整数 int 或者长整数 long int 的二进制表示。
````python
bin(x)
````

#### divmod
divmod() 函数把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a // b, a % b)。在 python 2.3 版本之前不允许处理复数。
````python
divmod(a, b)
````

#### sum
sum() 方法对系列进行求和计算。start指和的起始值，而不是迭代对象的下标。
````python
sum(iterable[, start])
````

#### chr
chr() 用一个整数作参数，返回一个对应的字符。
````python
chr(i)
````

#### ord
ord() 函数是 chr() 函数（对于 8 位的 ASCII 字符串）的配对函数，它以一个字符串（Unicode 字符）作为参数，返回对应的 ASCII 数值，或者 Unicode 数值。
````python
ord(c)
````

#### str.format
str.format() 函数可以接受不限个参数，位置可以不按顺序。Python2.6 开始，新增了一种格式化字符串的函数 str.format()，它增强了字符串格式化的功能。基本语法是通过 {} 和 : 来代替以前的 % 。
````python
str.format()
````
[more](http://www.runoob.com/python/att-string-format.html)

### 内置类
#### int
int() 函数用于将一个字符串或数字转换为整型。
````python
class int(x, base=10)
````

#### bool
bool() 函数用于将给定参数转换为布尔类型，如果没有参数，返回 False。bool 是 int 的子类。
````python
class bool([x])
````

#### float
float() 函数用于将整数和字符串转换成浮点数。
````python
class float([x])
````

#### list
list() 方法用于将元组转换为列表。元组与列表是非常类似的，区别在于元组的元素值不能修改，元组是放在括号中，列表是放于方括号中。
````python
list(seq)
````

#### tuple
tuple 函数将列表转换为元组。
````python
tuple(seq)
````

#### dict
dict() 函数用于创建一个字典。
````python
#### # **kwargs -- 关键字
#### # mapping -- 元素的容器。
# iterable -- 可迭代对象。
class dict(**kwarg)
class dict(mapping, **kwarg)
class dict(iterable, **kwarg)
````

#### str
str() 函数将对象转化为适于人阅读的形式。
````python
class str(object='')
````

#### long
long() 函数将数字或字符串转换为一个长整型。
````python
class long(x, base=10)
````

#### frozenset
frozenset() 返回一个冻结的集合，冻结后集合不能再添加或删除任何元素。
````python
class frozenset([iterable])
````

#### property
property() 函数的作用是在新式类中返回属性值。
````python
class property([fget[, fset[, fdel[, doc]]]])
````
[more](http://www.runoob.com/python/python-func-property.html)

#### bytearray
bytearray() 方法返回一个新字节数组。这个数组里的元素是可变的，并且每个元素的值范围: 0 <= x < 256。
````python
class bytearray([source[, encoding[, errors]]])
````
[more](http://www.runoob.com/python/python-func-bytearray.html)

#### bytes
bytes 函数返回一个新的 bytes 对象，该对象是一个 0 <= x < 256 区间内的整数不可变序列。它是 bytearray 的不可变版本。
````python
class bytes([source[, encoding[, errors]]])
````
[more](http://www.runoob.com/python3/python3-func-bytes.html)

#### complex
complex() 函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数。
````python
class complex([real[, imag]])
````
[more](http://www.runoob.com/python/python-func-complex.html)

#### set
set() 函数创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。
````python
class set([iterable])
````
[more](http://www.runoob.com/python/python-func-set.html)

### 类相关
#### super
super() 函数是用于调用父类(超类)的一个方法。super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。
````python
super(type[, object-or-type])
````
[more](http://www.runoob.com/python/python-func-super.html)

#### isinstance
isinstance() 函数来判断一个对象是否是一个已知的类型，类似 type()。
````python
isinstance(object, classinfo)
````
[more](http://www.runoob.com/python/python-func-isinstance.html)

#### type
type() 函数如果你只有第一个参数则返回对象的类型，三个参数返回新的类型对象。isinstance() 与 type() 区别：type() 不会认为子类是一种父类类型，不考虑继承关系。isinstance() 会认为子类是一种父类类型，考虑继承关系。如果要判断两个类型是否相同推荐使用 isinstance()。
````python
type(object)
type(name, bases, dict)
````
[more](http://www.runoob.com/python/python-func-type.html)

#### issubclass
issubclass() 方法用于判断参数 class 是否是类型参数 classinfo 的子类。
````python
issubclass(class, classinfo)
````
[more](http://www.runoob.com/python/python-func-issubclass.html)

### 文件相关
#### open
open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。注意：使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。
````python
open(file, mode='r')
open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
````
[more](http://www.runoob.com/python3/python3-func-open.html)

#### file
file() 函数用于创建一个 file 对象，它有一个别名叫 open()，更形象一些，它们是内置函数。参数是以字符串的形式传递的。
````python
file(name[, mode[, buffering]])
````
[more](http://www.runoob.com/python/python-func-file.html)

### 属性相关

#### vars
vars() 函数返回对象object的属性和属性值的字典对象。返回对象object的属性和属性值的字典对象，如果没有参数，就打印当前调用位置的属性和属性值 类似 locals()。
````python
vars([object])
````
[more](http://www.runoob.com/python/python-func-vars.html)

#### getattr
getattr() 函数用于返回一个对象属性值。
````python
getattr(object, name[, default])
````
[more](http://www.runoob.com/python/python-func-getattr.html)

#### setattr
setattr() 函数对应函数 getattr()，用于设置属性值，该属性不一定是存在的。
````python
setattr(object, name, value)
````

#### hasattr
hasattr() 函数用于判断对象是否包含对应的属性。
````python
hasattr(object, name)
````

#### delattr
delattr 函数用于删除属性。delattr(x, 'foobar') 相等于 del x.foobar。
````python
delattr(object, name)
````

### 迭代相关

#### iter
iter() 函数用来生成迭代器。
````python
iter(object[, sentinel])
````
[more](http://www.runoob.com/python/python-func-iter.html)

#### filter
filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回一个迭代器对象，如果要转换为列表，可以使用 list() 来转换。该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。注意: Pyhton2.7 返回列表，Python3.x 返回迭代器对象。
````python
filter(function, iterable)
````
[more](http://www.runoob.com/python3/python3-func-filter.html)

#### next
next() 返回迭代器的下一个项目。
````python
next(iterator[, default])
````

#### reversed
reversed 函数返回一个反转的迭代器。
````python
reversed(seq)
````

#### all
all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE，如果是返回 True，否则返回 False。元素除了是 0、空、None、False 外都算 True。Python 2.5 以上版本可用。
````python
all(iterable)
````

#### any
any() 函数用于判断给定的可迭代参数 iterable 是否全部为 False，则返回 False，如果有一个为 True，则返回 True。元素除了是 0、空、FALSE 外都算 TRUE。Python 2.5 以上版本可用。
````python
any(iterable)
````

#### enumerate
enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中（返回值：\[(index,item),...\]）。Python 2.3. 以上版本可用，2.6 添加 start 参数。
````python
enumerate(sequence, [start=0])
````

#### sorted
sorted() 函数对所有可迭代的对象进行排序操作。sort 与 sorted 区别：sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。list 的 sort 方法返回的是对已经存在的列表进行操作，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。
````python
sorted(iterable, key=None, reverse=False)  
````
[more](http://www.runoob.com/python3/python3-func-enumerate.html)

#### slice
slice() 函数实现切片对象，主要用在切片操作函数里的参数传递。
````python
class slice(stop)
class slice(start, stop[, step])
````
[more](http://www.runoob.com/python/python-func-slice.html)

#### map
map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。
````python
map(function, iterable, ...)
````
[more](http://www.runoob.com/python/python-func-map.html)

#### reduce
reduce() 函数会对参数序列中元素进行累积。函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。
````python
reduce(function, iterable[, initializer])
````
[more](http://www.runoob.com/python/python-func-reduce.html)

### 其他

#### staticmethod
staticmethod 返回函数的静态方法。该方法不强制要求传递参数，如下声明一个静态方法。
````python
staticmethod(function)
````

#### eval
eval() 函数用来执行一个字符串表达式，并返回表达式的值。
````python
eval(expression[, globals[, locals]])
````
[more](http://www.runoob.com/python/python-func-eval.html)

#### exec
exec 执行储存在字符串或文件中的 Python 语句，相比于 eval，exec可以执行更复杂的 Python 代码。exec 返回值永远为 None。
````python
exec(object[, globals[, locals]])
````
[more](http://www.runoob.com/python3/python3-func-exec.html)

#### callable
callable() 函数用于检查一个对象是否是可调用的。如果返回 True，object 仍然可能调用失败；但如果返回 False，调用对象 object 绝对不会成功。对于函数、方法、lambda 函式、 类以及实现了 \_\_call\_\_ 方法的类实例, 它都返回 True。
````python
callable(object)
````
[more](http://www.runoob.com/python/python-func-callable.html)

#### locals
locals() 函数会以字典类型返回当前位置的全部局部变量。对于函数, 方法, lambda 函式, 类, 以及实现了 \_\_call\_\_ 方法的类实例, 它都返回 True。
````python
locals()
````
[more](http://www.runoob.com/python/python-func-locals.html)

#### globals
globals() 函数会以字典类型返回当前位置的全部全局变量。返回全局变量的字典。
````python
globals()
````

#### reload
reload() 用于重新载入之前载入的模块。
````python
reload(module)
````

#### \_\_import\_\_
\_\_import\_\_() 函数用于动态加载类和函数 。如果一个模块经常变化就可以使用 \_\_import\_\_() 来动态载入。
````python
__import__(name[, globals[, locals[, fromlist[, level]]]])
````

#### classmethod
classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。返回函数的类方法。
````python
classmethod
````

#### repr
repr() 函数将对象转化为供解释器读取的形式。返回一个对象的 string 格式。
````python
repr(object)
````

#### ascii
ascii() 函数类似 repr() 函数, 返回一个表示对象的字符串, 但是对于字符串中的非 ASCII 字符则返回通过 repr() 函数使用 \x, \u 或 \U 编码的字符。 生成字符串类似 Python2 版本中 repr() 函数的返回值。
````python
ascii(object)
````

#### dir
dir() 函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属性、方法列表。如果参数包含方法__dir__()，该方法将被调用。如果参数不包含__dir__()，该方法将最大限度地收集参数信息。
````python
dir([object])
````

#### id
id() 函数用于获取对象的内存地址。
````python
id([object])
````

#### hash
hash() 用于获取取一个对象（字符串或者数值等）的哈希值。
````python
hash(object)
````

#### zip
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。我们可以使用 list() 转换来输出列表。如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。在 Python 2.x zip() 返回的是一个列表。
````python
zip([iterable, ...])
````

#### compile
compile() 函数将一个字符串编译为字节代码。
````python
compile(source, filename, mode[, flags[, dont_inherit]])
````
[more](http://www.runoob.com/python/python-func-compile.html)

#### memoryview
memoryview() 函数返回给定参数的内存查看对象(Momory view)。所谓内存查看对象，是指对支持缓冲区协议的数据进行包装，在不需要复制对象基础上允许Python代码访问。
````python
memoryview(obj)
````
[more](http://www.runoob.com/python/python-func-memoryview.html)