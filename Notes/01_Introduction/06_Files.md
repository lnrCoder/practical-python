[Contents](../Contents.md) \| [Previous (1.5 Lists)](05_Lists.md) \| [Next (1.7 Functions)](07_Functions.md)

# 1.6 文件处理

大多数程序需要从某些地方读取输入的数据。本节将讨论文件的访问。

### 文件的输入和输出

打开一个文件

```python
f = open('foo.txt', 'rt')     # 打开文件来读
g = open('bar.txt', 'wt')     # 打开文件来写
```

读取所有的数据

```python
data = f.read()

# 最多读取 'maxbytes' 字节数据
data = f.read([maxbytes])
```

写入一些数据

```python
g.write('some text')
```

完成后关闭

```python
f.close()
g.close()
```

文件应正确关闭，但这是一个容易忘记的步骤。因此，首选的方法是下面这样使用 with 语句

```python
with open(filename, 'rt') as file:
    # 使用 `file`
    ...
    # 无需显式的关闭
...statements
```

当离开 with 缩进的代码块时，会自动关闭文件。

### 读取文件的习惯用法

以字符串形式一次读取整个文件

```python
with open('foo.txt', 'rt') as file:
    data = file.read()
    # `data` 是一个字符串，包含 `foo.txt` 文件的所有内容
```

通过迭代逐行读取数据

```python
with open(filename, 'rt') as file:
    for line in file:
        # 在这里处理每行数据
```

### 写入文件的习惯用法

写入字符串数据

```python
with open('outfile', 'wt') as out:
    out.write('Hello World\n')
    ...
```

重定向打印功能

```python
with open('outfile', 'wt') as out:
    print('Hello World', file=out)
    ...
```

## 练习

这些练习依赖于文件 `Data/portfolio.csv`。该文件包含行列表以及有关股票投资组合的信息。假设你正在 `practical-python/Work/` 目录中工作。如果不确定运行位置，可以通过执行以下操作找出 Python 的运行位置：

```python
>>> import os
>>> os.getcwd()
'/Users/beazley/Desktop/practical-python/Work' # Output vary
>>>
```

### 练习 1.26：文件练习准备工作

首先，将文件作为大字符串一次性读取。

```python
>>> with open('Data/portfolio.csv', 'rt') as f:
        data = f.read()

>>> data
'name,shares,price\n"AA",100,32.20\n"IBM",50,91.10\n"CAT",150,83.44\n"MSFT",200,51.23\n"GE",95,40.37\n"MSFT",50,65.10\n"IBM",100,70.44\n'
>>> print(data)
name,shares,price
"AA",100,32.20
"IBM",50,91.10
"CAT",150,83.44
"MSFT",200,51.23
"GE",95,40.37
"MSFT",50,65.10
"IBM",100,70.44
>>>
```

在上面的示例中，应注意 Python 具有两种输出模式。在第一种模式下，你在提示符下键入 `data`，Python 向你显示原始字符串的表示形式，包括引号和转义码。当你键入 `print(data)` 时，你将获得字符串实际的格式化输出。

尽管一次读取一个文件很简单，但通常不是最合适的处理方式 —— 尤其是当文件碰巧很大或包含需要一次处理一个的文本行时。

要逐行读取文件，请使用如下所示的for循环：

```python
>>> with open('Data/portfolio.csv', 'rt') as f:
        for line in f:
            print(line, end='')

name,shares,price
"AA",100,32.20
"IBM",50,91.10
...
>>>
```

当你用如上所示的代码时，将按行读取，直到文件末尾，然后循环停止。

在某些情况下，你可能希望手动读取或跳过一行文本（例如：你可能想跳过第一行的列标题数据。）

```python
>>> f = open('Data/portfolio.csv', 'rt')
>>> headers = next(f)
>>> headers
'name,shares,price\n'
>>> for line in f:
    print(line, end='')

"AA",100,32.20
"IBM",50,91.10
...
>>> f.close()
>>>
```

`next()` 用来返回文件中的下一行文本。如果反复的调用它，则会得到连续的行。然而，就像你知道的，我们已经在 `for`  循环中使用 `next()`  来获取其数据。因此，除非你视图显式的跳过或者读取一行，否者通常不会直接调用它。

读取文件行后，你可以执行更多的处理操作，例如进行拆分。尝试以下操作：

```python
>>> f = open('Data/portfolio.csv', 'rt')
>>> headers = next(f).split(',')
>>> headers
['name', 'shares', 'price\n']
>>> for line in f:
    row = line.split(',')
    print(row)

['"AA"', '100', '32.20\n']
['"IBM"', '50', '91.10\n']
...
>>> f.close()
```

*注意：在上面这些例子中，因为没有使用 `with` 语句，所以显式调用了 `f.close()` 。*

### 练习 1.27：读取一个数据文件

现在你已经学会了如何读取文件，让我们来编写一个程序来执行简单的计算。

`portfolio.csv` 中的列对应于股票名称，股票数量和单股股票的购买价格。编写一个名为`pcost.py` 的程序，打开该文件，读取所有行，并计算购买所有股票的费用。

*提示：将字符串转换为整数，请使用 `int(s)`。转换为浮点数，请使用`float(s)`。*

你的程序应该会打印如下所示的内容：

```bash
Total cost 44671.15
```

### 练习 1.28：其他类型的“文件”

如果您想读取非文本文件，如 gzip 压缩的数据文件，应该怎么办？内置的 `open()` 函数在这里无济于事，但是 Python 有一个模块库 `gzip` ，可以读取 gzip 压缩文件。

来尝试一下：

```python
>>> import gzip
>>> with gzip.open('Data/portfolio.csv.gz', 'rt') as f:
    for line in f:
        print(line, end='')

... 查看输出内容 ...
>>>
```

注意：在这里`'rt'` 的文件模式至关重要。如果您忘记了这一点，将获得字节字符串而不是普通的文本字符串。

### 评论：为什么我们不使用 Pandas 呢？

数据科学家很快指出，像 [Pandas](https://pandas.pydata.org)  这样的库已经具有读取 CSV 文件的功能。的确如此-而且效果很好。但是，这不是学习 Pandas 的课程。读取文件是比读取特定的 CSV 文件更普遍的问题。我们使用 CSV 文件的主要原因是，它是大多数开发者熟悉的格式，并且相对容易说明此过程中的许多 Python 特性。这就意味着，当你回去工作时，还是应该使用 Pandas。但是，在本课程的其余部分中，我们将继续使用标准 Python 功能。

[Contents](../Contents.md) \| [Previous (1.5 Lists)](05_Lists.md) \| [Next (1.7 Functions)](07_Functions.md)
