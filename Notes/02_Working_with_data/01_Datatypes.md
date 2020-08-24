[Contents](../Contents.md) \| [Previous (1.6 Files)](../01_Introduction/06_Files.md) \| [Next (2.2 Containers)](02_Containers.md)


# 2.1 数据类型和数据结构
本节以元组和字典的形式来介绍数据结构。

### 基本据类型

Python 有一些基本的数据类型

* Integers 整数
* Floating point numbers 浮点数
* Strings (text) 字符串

我们在第一章节中已经了解了这些内容。

### 无类型

```python
email_address = None
```

`None` 通常作用可选值或缺失值的占位符。它在条件判断中被当做 `False` 。

```python
if email_address:
    send_email(email_address, msg)
```

### 数据结构

真实的应用程序拥有更复杂的数据。例如有关持股的信息：

```code
100 shares of GOOG at $490.10
```

这是一个包含三部分内容的 "Object (对象)"：

* 股票的名称或符合 ("GOOG", 字符串类型)
* 持股数量 (100, 整数)
* 价格 (490.10 浮点数)

### 元组

元组是一组值的集合。.

例如：:

```python
s = ('GOOG', 100, 490.1)
```

有时语法中会省略 `( )` 。

```python
s = 'GOOG', 100, 490.1
```

特殊情况 ( 0 元素元组，1 元素元组）。.

```python
t = ()            # 空元组
w = ('GOOG', )    # 一项元素的元组
```

元组通常用来表示简单的记录或结构。通常，它是由多个部分组成的单个*对象*。一个很好的类比：*元组就像数据库表中的一行。*

元组的数据是有序的 (跟数组类似)。

```python
s = ('GOOG', 100, 490.1)
name = s[0]                 # 'GOOG'
shares = s[1]               # 100
price = s[2]                # 490.1
```

但是，元组的内容无法被修改。

```python
>>> s[1] = 75
TypeError: object does not support item assignment
```

但是你可以基于当前元组创建一个新的元组

```python
s = (s[0], 75, s[2])
```

### 元组组包

元组更多地是将相关的内容封装成一个 *实体*。

```python
s = ('GOOG', 100, 490.1)
```

然后，该元组可以很容易作为单个对象传递给程序的其他部分。

### 元组解包

要在其他地方使用元组，你可以将其解包为变量。

```python
name, shares, price = s
print('Cost', shares * price)
```

左侧的变量数必须与元组结构匹配。

```python
name, shares = s     # ERROR
Traceback (most recent call last):
...
ValueError: too many values to unpack
```

### 元组 vs 列表

元组看起来像是只读列表。元组最常用于由多个部分组成的单个项。列表通常是不同项的集合，通常都是同一类型。

```python
record = ('GOOG', 100, 490.1)       # 代表投资记录的元组

symbols = [ 'GOOG', 'AAPL', 'IBM' ]  # 代表三个股票代码的列表
```

### 字典

字典是键到值的映射。有时也称为哈希表或关联数组。键用作访问值的索引。

```python
s = {
    'name': 'GOOG',
    'shares': 100,
    'price': 490.1
}
```

### 常用操作

使用键的名称从字典中获取值。

```python
>>> print(s['name'], s['shares'])
GOOG 100
>>> s['price']
490.10
>>>
```

通过键的名称来新增和修改值

```python
>>> s['shares'] = 75
>>> s['date'] = '6/6/2007'
>>>
```

使用 `del` 命令来删除值。

```python
>>> del s['date']
>>>
```

### 为什么选择字典？

当存在*许多*不同的值，并且这些值可能被修改或操作时，字典非常有用。字典使代码更具可读性。

```python
s['price']
# vs
s[2]
```

## 练习

最后几个练习，你需要编写一个程序来读取数据文件 `Data/portfolio.csv` 。使用 `csv` 模块，可以很容易地逐行读取文件。

```python
>>> import csv
>>> f = open('Data/portfolio.csv')
>>> rows = csv.reader(f)
>>> next(rows)
['name', 'shares', 'price']
>>> row = next(rows)
>>> row
['AA', '100', '32.20']
>>>
```

尽管读取文件很容易，但是与读取数据相比，你通常想对数据做更多的操作。比如，你可能想把数据存储起来并执行一些计算。不幸的是，原始的“行”数据不足以满足你的处理需求。即便是简单的数学计算都不行：

```python
>>> row = ['AA', '100', '32.20']
>>> cost = row[1] * row[2]
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: can't multiply sequence by non-int of type 'str'
>>>
```

为了进行耕作操作，你通常希望以某种方式来解释原始数据并将其转换为一种更有用的对象，以后便可以使用该对象。两个较容易的选择是元组和字典。

### 练习 2.1: 元组

在交互式操作下，创建代表一行数据的元组，并将数字列转换为正确的数字：

```python
>>> t = (row[0], int(row[1]), float(row[2]))
>>> t
('AA', 100, 32.2)
>>>
```

这样，你就可以使用价格乘以数量来计算出总费用了。

```python
>>> cost = t[1] * t[2]
>>> cost
3220.0000000000005
>>>
```

Python 的数学计算是不是坏了？3220.0000000000005 是个什么答案？

这是因为你计算机上的浮点硬件只能精确的表示二进制数，而不是十进制数。即便是简单的10进制小数计算，也会引入小误差。这是恒昌的，不过如果你之前没见过，可能会有点惊讶。

在使用浮点小数的所有编程语言中都会发生这种情况，但在打印时通常会隐藏它。例如：

```python
>>> print(f'{cost:0.2f}')
3220.00
>>>
```

元组是只读的。通过尝试将拥有数变更为 75 来验证这一点。

```python
>>> t[1] = 75
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
```

尽管无法更改元组的内容，但是你一直可以创建一个全新的元组来代替旧的元组。

```python
>>> t = (t[0], 75, t[2])
>>> t
('AA', 75, 32.2)
>>>
```

每当你使用一个已存在的变量名时，旧的数据将会被丢弃。尽管上面的操作看似是修改了元组，实际上你是创建了一个新的元组，并将旧的弃用了。

元组通常用于将值组包和解包到变量中。试试如下操作：

```python
>>> name, shares, price = t
>>> name
'AA'
>>> shares
75
>>> price
32.2
>>>
```

使用上述变量并将它们组包到元组

```python
>>> t = (name, 2*shares, price)
>>> t
('AA', 150, 32.2)
>>>
```

### 练习 2.2: 将字典作为数据结构

使用字典是替代元组的一种方式。

```python
>>> d = {
        'name' : row[0],
        'shares' : int(row[1]),
        'price'  : float(row[2])
    }
>>> d
{'name': 'AA', 'shares': 100, 'price': 32.2 }
>>>
```

计算持有的总成本：

```python
>>> cost = d['shares'] * d['price']
>>> cost
3220.0000000000005
>>>
```

将此示例与上面涉及元组的相同计算进行比较。将持有数量更改为75。

```python
>>> d['shares'] = 75
>>> d
{'name': 'AA', 'shares': 75, 'price': 32.2 }
>>>
```

与元组不同的是，字典可以自由修改、添加属性：

```python
>>> d['date'] = (6, 11, 2007)
>>> d['account'] = 12345
>>> d
{'name': 'AA', 'shares': 75, 'price':32.2, 'date': (6, 11, 2007), 'account': 12345}
>>>
```

### 练习 2.3: 一些其他的字典操作

如果你将字段转换成列表，你将得到所有的键：

```python
>>> list(d)
['name', 'shares', 'price', 'date', 'account']
>>>
```

同样的，如果你使用`for` 循环来遍历字典，你将得到所有的键：

```python
>>> for k in d:
        print('k =', k)
k = name
k = shares
k = price
k = date
k = account
>>>
```

尝试使用这种变体查找方式：

```python
>>> for k in d:
        print(k, '=', d[k])
name = AA
shares = 75
price = 32.2
date = (6, 11, 2007)
account = 12345
>>>
```

你也可以使用`keys()` 方法来获取所有的键：

```python
>>> keys = d.keys()
>>> keys
dict_keys(['name', 'shares', 'price', 'date', 'account'])
>>>
```

`keys()` 有点不同寻常，它返回的是一个特殊的`dict_keys` 对象。
这是基于原始字典的，即使字典发生了变化，它也总能给你提供当前的键。例如，尝试一下操作：

```python
>>> del d['account']
>>> keys
dict_keys(['name', 'shares', 'price', 'date'])
>>>
```

需要注意的是，即便你没有再次调用 `d.keys()` ，keys 中的 `'account'` 也不见了。

一种更优雅的同时操作键和值的方式是使用`items()` 方法。它将返回给你 `(key, value)`  的元组数据：

```python
>>> items = d.items()
>>> items
dict_items([('name', 'AA'), ('shares', 75), ('price', 32.2), ('date', (6, 11, 2007))])
>>> for k, v in d.items():
        print(k, '=', v)
name = AA
shares = 75
price = 32.2
date = (6, 11, 2007)
>>>
```

如果你有一个类似`items`的元组，你可以使用`dict()` 方法来创建字典。来试试吧：

```python
>>> items
dict_items([('name', 'AA'), ('shares', 75), ('price', 32.2), ('date', (6, 11, 2007))])
>>> d = dict(items)
>>> d
{'name': 'AA', 'shares': 75, 'price':32.2, 'date': (6, 11, 2007)}
>>>
```

[Contents](../Contents.md) \| [Previous (1.6 Files)](../01_Introduction/06_Files.md) \| [Next (2.2 Containers)](02_Containers.md)
