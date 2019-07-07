---
layout: article
title: 第1章 Python数据模型
tags:
  - python
  - 笔记
  - 《流畅的Python》
---

<!--more-->

目标读者：本书的目标读者是那些正在使用 Python，又想熟悉 Python 3 的程序员。主要目的是为了充分地展现 Python 3.4 的魅力。主要会强调 Python 作为编程语言独有的特性，这些特性或者是只有 Python 才具备的，或者是在其他大众语言里很少见的。Python 语言核心以及它的一些库会是本书的重点。

测试驱动开发（TDD）的精髓就是先写测试
作者认为：在考虑如何实现一个功能之前，先严格地列出这个功能能做什么，这能帮助我们在编程时把精力花在该花的地方。

验证代码的准确性
```shell
python -m doctest *.py
```

### 第 1 章 Python数据模型
Python 最好的品质之一是一致性。   
魔术方法：magic method，即以双下划线开头和结尾的特殊方法，也叫双下方法（dunder method）。

### 1.1 一摞Python风格纸牌

```python
import collections

Card = collections.namedtuple('Card', ['rank', 'suit'])


# 1.1 一摞Python风格的纸牌
class FrenchDeck:
    ranks = [str(n) for n in range(2, 11)] + list('JQKA')
    suits = 'spades diamonds clubs hearts'.split()

    def __init__(self):
        self._cards = [Card(rank, suit) for suit in self.suits for rank in self.ranks]

    def __len__(self):
        return len(self._cards)

    def __getitem__(self, position):
        return self._cards[position]


if __name__ == '__main__':
    # 自 Python 2.6 开始， namedtuple 就加入到 Python 里， 用以
    # 构建只有少数属性但是没有方法的对象， 比如数据库条目
    beer_card = Card('7', 'diamonds')
    print(beer_card)

    # FrenchDeck
    deck = FrenchDeck()
    # __len__方法
    print('\nFrenchDeck\nlen:', len(deck))
    # __getitem__方法 索引
    print(deck[0], deck[-1])
    from random import choice

    print(choice(deck), choice(deck))
    # __getitem__方法 切片
    print(deck[:2], '\n', deck[12:14])
    # __getitem__方法 迭代-0
    for card in deck:
        print(card)
    # __getitem__方法 反向迭代
    for card in reversed(deck):
        print(card)

    # 某种计算方式
    suit_values = dict(spades=3, hearts=2, diamonds=1, clubs=0)


    def spades_high(card):
        rank_value = FrenchDeck.ranks.index(card.rank)
        return rank_value * len(suit_values) + suit_values[card.suit]


    # 根据spades_high的计算方法进行排序
    for card in sorted(deck, key=spades_high):
        print(card)
    # 在 Python 2 中， 对 object 的继承需要显式地写为 FrenchDeck(object) ； 而在 Python3中， 这个继承关系是默认的。
```
collections.namedtuple用以构建只有少数属性但是没有方法的对象 

通过实现 \_\_len\_\_ 和 \_\_getitem\_\_，实现这两个特殊方法，会使自定义类就跟一个 Python 自有的序列数据类型一样，可以体现出 Python 的核心语言特性（例如迭代和切片）。同时这个类还可以用于标准库中诸如 random.choice、reversed 和 sorted 这些函数。另外， 对合成的运用使得\_\_len\_\_和\_\_getitem\_\_ 的具体实现可以代理给 self.\_cards 这个 Python 列表（即 list 对象） 。  
> 在 Python 2 中， 对 object 的继承需要显式地写为 FrenchDeck(object) ； 而在 Python 3中， 这个继承关系是默认的。  

### 1.2 如何使用特殊方法

特殊方法的调用大多是隐式的， 比如 for i in x: 这个语句，背后其实用的是 iter(x) ， 而这个函数的背后则是 x.\_\_iter\_\_() 方法。 当然前提是这个方法在 x 中被实现了。  
通过内置的函数（例如 len 、 iter 、 str ， 等等） 来使用特殊方法是最好的选择。 这些内置函数不仅会调用特殊方法， 通常还提供额外的好处， 而且对于内置的类来说， 它们的速度更快。 

abs 是一个内置函数， 如果输入是整数或者浮点数， 它返回的是输入值的绝对值； 如果输入是复数（complex number） ， 那么返回这个复数的模。 为了保持一致性， 我们的 API 在碰到 abs 函数的时候， 也应该返回该向量的模

一个简单的二维向量类:  
```python
from math import hypot


class Vector:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def __repr__(self):
        return 'Vector(%r, %r)' % (self.x, self.y)

    def __abs__(self):
        return hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

    def __add__(self, other):
        x = self.x + other.x
        y = self.y + other.y
        return Vector(x, y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)


if __name__ == '__main__':
    # __init__
    v1 = Vector(2, 4)
    v2 = Vector(3, 4)
    # __repr__ __add__
    print(v1, v1 + v2)
    # abs 是一个内置函数， 如果输入是整数或者浮点数， 它返回的是输入值的绝对值； 如果输入是复数（complex number） ， 那么返回这个复数的模。 为了保持一致性， 我们的 API 在碰到 abs 函数的时候， 也应该返回该向量的模
    print(abs(v2))
    # __mul__ 标量乘法
    print(v1, v1 * 2)
```

虽然代码里有 6 个特殊方法， 但这些方法（除了 \_\_init\_\_ ） 并不会在这个类自身的代码中使用。 即便其他程序要使用这个类的这些方法， 也不会直接调用它们， 就像我们在上面的控制台对话中看到的。 上文也提到过， 一般只有 Python 的解释器会频繁地直接调用这些方法。

Python 有一个内置的函数叫 repr ， 它能把一个对象用字符串的形式表达出来以便辨认， 这就是“字符串表示形式”。 repr 就是通过 \_\_repr\_\_这个特殊方法来得到一个对象的字符串表示形式的。 如果没有实现\_\_repr\_\_ ， 当我们在控制台里打印一个向量的实例时， 得到的字符串可能会是 <Vector object at 0x10e100070> 。  \_\_repr\_\_ 所返回的字符串应该准确、 无歧义， 并且尽可能表达出如何用代码创建出这个被打印的对象。 因此这里使用了类似调用对象构造器的表达形式。  
\_\_repr\_\_ 和 \_\_str\_\_ 的区别在于， 后者是在 str() 函数被使用， 或是在用 print 函数打印一个对象的时候才被调用的， 并且它返回的字符串对终端用户更友好。  
如果你只想实现这两个特殊方法中的一个， \_\_repr\_\_ 是更好的选择，因为如果一个对象没有 \_\_str\_\_ 函数， 而 Python 又需要调用它的时候， 解释器会用 \_\_repr\_\_ 作为替代。  

默认情况下， 我们自己定义的类的实例总被认为是真的， 除非这个类对\_\_bool\_\_ 或者 \_\_len\_\_ 函数有自己的实现。 bool(x) 的背后是调用x.\_\_bool\_\_() 的结果； 如果不存在 \_\_bool\_\_ 方法， 那么 bool(x) 会尝试调用 x.\_\_len\_\_() 。 若返回 0， 则 bool 会返回 False ； 否则返回 True 。`bool(self.x or self.y)`会更高效一些。

`s.count(x)`作用是计算 s 中 x 出现的次数

### 本章小结
通过实现特殊方法， 自定义数据类型可以表现得跟内置类型一样， 从而让我们写出更具表达力的代码——或者说， 更具 Python 风格的代码。Python 对象的一个基本要求就是它得有合理的字符串表示形式， 我们可以通过 \_\_repr\_\_ 和 \_\_str\_\_ 来满足这个要求。 前者方便我们调试和记录日志， 后者则是给终端用户看的。 这就是数据模型中存在特殊方法\_\_repr\_\_ 和 \_\_str\_\_ 的原因。   
Python 通过运算符重载这一模式提供了丰富的数值类型， 除了内置的那些之外， 还有 decimal.Decimal 和 fractions.Fraction 。 这些数据类型都支持中缀算术运算符。 