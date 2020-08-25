[Contents](../Contents.md) \| [Previous (2.1 Datatypes)](01_Datatypes.md) \| [Next (2.3 Formatting)](03_Formatting.md)

#  2.2 容器

本节我们将讨论 lists 列表，dictionaries 字典 和 sets 集合。

### 概述

程序通常需要处理很多的对象。

* 股票的投资组合
* 股票价格表

有三种主要方式可供使用

* 列表：有序的数据。
* 字典：无序的数据。
* 集合：唯一项的无序集合。

### 列表

当数据的顺序很重要时，请使用列表。需要记住的是，列表可以容纳任何类型的对象。例如，元组的列表：

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.3),
    ('CAT', 150, 83.44)
]
portfolio[0]            # ('GOOG', 100, 490.1)
portfolio[2]            # ('CAT', 150, 83.44)
```

### 列表结构

从头开始构建一个列表

```python
records = []  # 初始化列表
# 使用 .append() 来添加项
records.append(('GOOG', 100, 490.10))
records.append(('IBM', 50, 91.3))
...
```

从文件读取记录的示例

```python
records = []  # 初始化列表
with open('Data/portfolio.csv', 'rt') as f:
    next(f) # 忽略标题头
    for line in f:
        row = line.split(',')
        records.append((row[0], int(row[1]), float(row[2])))
```

### 字典

如果你需要（按键名）快速随机查找，则字典非常有用。例如，股票价格的字典：

```python
prices = {
   'GOOG': 513.25,
   'CAT': 87.22,
   'IBM': 93.37,
   'MSFT': 44.12
}
```

一下是一些简单的查询方式：

```python
>>> prices['IBM']
93.37
>>> prices['GOOG']
513.25
>>>
```

### 字典结构

从头开始构建字典的示例

```python
prices = {} # 初始化空字典
# 增加新的项
prices['GOOG'] = 513.25
prices['CAT'] = 87.22
prices['IBM'] = 93.37
```

使用文件中的内容填充字典的示例：

```python
prices = {} # 初始化空字典
with open('Data/prices.csv', 'rt') as f:
    for line in f:
        row = line.split(',')
        prices[row[0]] = float(row[1])
```


注意：如果你在`Data/prices.csv` 文件上操作，你会发现它基本是可以工作——但末尾有一个空行会导致它崩溃。你需要找出一些方法来修改代码以解决此问题（请参阅 练习2.6）

### 字典查询

你可以检测某个key是否存在

```python
if key in d:
    # YES
else:
    # NO
```

你可以查询一个有可能不存在的值，当不存在的情况下提供一个默认的值

```python
name = d.get(key, default)
```

示例：

```python
>>> prices.get('IBM', 0.0)
93.37
>>> prices.get('SCOX', 0.0)
0.0
>>>
```

### 复合键

在 Python 中，几乎任何类型的值都可以作为字典的键。字典的键必须是不可变类型。例如，元组：

```python
holidays = {
  (1, 1) : 'New Years',
  (3, 14) : 'Pi day',
  (9, 13) : "Programmer's day",
}
```

然后访问：

```python
>>> holidays[3, 14]
'Pi day'
>>>
```

*列表，集合以及字典都不能作为字典的键，因为列表和字典都是可变的。*

### 集合

集合是无序唯一项的集合

```python
tech_stocks = { 'IBM','AAPL','MSFT' }
# 替代语法
tech_stocks = set(['IBM', 'AAPL', 'MSFT'])
```

集合非常适用于检测成员是否存在

```python
>>> tech_stocks
set(['AAPL', 'IBM', 'MSFT'])
>>> 'IBM' in tech_stocks
True
>>> 'FB' in tech_stocks
False
>>>
```

集合也非常适用于消除重复数据

```python
names = ['IBM', 'AAPL', 'GOOG', 'IBM', 'GOOG', 'YHOO']
unique = set(names)
# unique = set(['IBM', 'AAPL','GOOG','YHOO'])
```

其他的一些操作：

```python
names.add('CAT')        # 添加一项数据
names.remove('YHOO')    # 删除一项数据
s1 | s2                 # 合并集合
s1 & s2                 # 求集合的交集
s1 - s2                 # 求集合的不同
```

## 练习

在这些练习中，你将开始构建本课程其余部分使用的主要程序之一。在 `Work/report.py` 中进行练习。

### 练习 2.4: 一个元组的列表

文件 `Data/portfolio.csv` 包含了股票投资组合总的股票列表。在 练习1.30 中，你已经编写了一个函数 portfolio_cost(filename) 来读取该文件并执行简单的计算。

你的代码看起来应该是如下所示这样的： 

```python
# pcost.py
import csv
def portfolio_cost(filename):
    '''计算投资组合文件的总成本（份额*价格）'''
    total_cost = 0.0
    with open(filename, 'rt') as f:
        rows = csv.reader(f)
        headers = next(rows)
        for row in rows:
            nshares = int(row[1])
            price = float(row[2])
            total_cost += nshares * price
    return total_cost
```

使用该代码创建一个新的文件 `report.py`。在该文件中，定义一个函数 `read_portfolio(filename)` ，该函数读取给定的文件并将数据存储到元组列表。为此，你需要对上述代码做一些小的改动。

首先，创建一个初始为空的列表变量，而不是定义 `total_cost = 0`。示例：

```python
portfolio = []
```

接下来，你不需要计算总数，而是将每一行转换成一个元组，正如上一个练习中所做的那样，并将元组添加到这个空列表中。如下所示：

```python
for row in rows:
    holding = (row[0], int(row[1]), float(row[2]))
    portfolio.append(holding)
```

最终，你将返回投资组合文件的结果数据列表。

使用交互式操作来测试你的函数（为了做到这一点，你首先应该在编译器里执行`report.py` ）

*提示: 在终端中执行文件时使用 `-i`  （python -i report.py）*

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> portfolio
[('AA', 100, 32.2), ('IBM', 50, 91.1), ('CAT', 150, 83.44), ('MSFT', 200, 51.23),
    ('GE', 95, 40.37), ('MSFT', 50, 65.1), ('IBM', 100, 70.44)]
>>>
>>> portfolio[0]
('AA', 100, 32.2)
>>> portfolio[1]
('IBM', 50, 91.1)
>>> portfolio[1][1]
50
>>> total = 0.0
>>> for s in portfolio:
        total += s[1] * s[2]
>>> print(total)
44671.15
>>>
```

你创建的这个元组列表与二维数组非常相似。例如，你可以使用命令 `portfolio[row][column]`  查询特定的行和列，其中 `row` 和 `column` 都是整数。

也就是说，你还可以使用如下语句重写最后一个for循环：

```python
>>> total = 0.0
>>> for name, shares, price in portfolio:
            total += shares*price
>>> print(total)
44671.15
>>>
```

### 练习 2.5：列表字典

使用 练习2.4 中编写的函数，进行修改，用字典而不是元组来表示投资组合中的每只股票。在这个字典中，使用字段名 "name""shares" 和 "price" 来表示输入文件中的不同列。

使用与练习2.4中相同的方式尝试这个新功能。

```python
>>> portfolio = read_portfolio('Data/portfolio.csv')
>>> portfolio
[{'name': 'AA', 'shares': 100, 'price': 32.2}, {'name': 'IBM', 'shares': 50, 'price': 91.1},
    {'name': 'CAT', 'shares': 150, 'price': 83.44}, {'name': 'MSFT', 'shares': 200, 'price': 51.23},
    {'name': 'GE', 'shares': 95, 'price': 40.37}, {'name': 'MSFT', 'shares': 50, 'price': 65.1},
    {'name': 'IBM', 'shares': 100, 'price': 70.44}]
>>> portfolio[0]
{'name': 'AA', 'shares': 100, 'price': 32.2}
>>> portfolio[1]
{'name': 'IBM', 'shares': 50, 'price': 91.1}
>>> portfolio[1]['shares']
50
>>> total = 0.0
>>> for s in portfolio:
        total += s['shares']*s['price']
>>> print(total)
44671.15
>>>
```

在这里，您会注意到，每个条目的不同字段都是通过键名而不是数字列号访问的。这通常是首选，因为生成的代码后续更容易阅读。

直接查看大型字典和列表可能很混乱。可以考虑使用 `pprint` 方法来整理输出的方式用以调试。

```python
>>> from pprint import pprint
>>> pprint(portfolio)
[{'name': 'AA', 'price': 32.2, 'shares': 100},
    {'name': 'IBM', 'price': 91.1, 'shares': 50},
    {'name': 'CAT', 'price': 83.44, 'shares': 150},
    {'name': 'MSFT', 'price': 51.23, 'shares': 200},
    {'name': 'GE', 'price': 40.37, 'shares': 95},
    {'name': 'MSFT', 'price': 65.1, 'shares': 50},
    {'name': 'IBM', 'price': 70.44, 'shares': 100}]
>>>
```

### 练习 2.6: 字典用作容器

当需要使用非整数索引查找项时，字典是一种有用的方式。在 Python Shell 中，尝试使用字典：

```python
>>> prices = { }
>>> prices['IBM'] = 92.45
>>> prices['MSFT'] = 45.12
>>> prices
... 查看执行结果 ...
>>> prices['IBM']
92.45
>>> prices['AAPL']
... 查看执行结果 ...
>>> 'AAPL' in prices
False
>>>
```

文件`Data/prices.csv` 包含一系列包含股票价格的行。

文件内容如下所示：

```csv
"AA",9.22
"AXP",24.85
"BA",44.85
"BAC",11.27
"C",3.72
```

编写一个函数 `read_prices(filename)` 将这写数据读取到字典中，字典的键为股票的名称，字典的值为股票的价格。

为此，要从一个空字典开始，然后向上面一样开始往字典内插入值。但是，现在需要从文件中读取值。

我们将使用此数据结构快速查找给定股票名称的价格。

这里我们可以使用一些小技巧。首先，可以像以前一样使用 `csv`  模块 — 无需再重新发明轮子。

```python
>>> import csv
>>> f = open('Data/prices.csv', 'r')
>>> rows = csv.reader(f)
>>> for row in rows:
        print(row)
['AA', '9.22']
['AXP', '24.85']
...
[]
>>>
```

另外一个小麻烦是`Data/prices.csv` 文件中有可能有一些空行。注意看上面数据的最后一行是一个空列表，这意味着该行上没有数据。

这有可能导致你的程序异常终止。使用 `try` 和 `except`  语句来进行适当的捕获。
思考一下：如果使用 `if` 语句来防止不良数据会更好吗？

编写完 `read_prices()`  函数后，需要对其进行测试以确保可以正常执行。

```python
>>> prices = read_prices('Data/prices.csv')
>>> prices['IBM']
106.28
>>> prices['MSFT']
20.89
>>>
```

[Contents](../Contents.md) \| [Previous (2.1 Datatypes)](01_Datatypes.md) \| [Next (2.3 Formatting)](03_Formatting.md)
