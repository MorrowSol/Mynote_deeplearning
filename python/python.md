# Python文档（3.9.7）

## 注释

Python 注释以 `#` 开头，直到该物理行结束。注释可以在行开头，或空白符与代码之后，但不能在字符串里面。字符串中的 # 号就是 # 号。注释用于阐明代码，Python 不解释注释，键入例子时，可以不输入注释。

几个例子：

```python
# this is the first comment
spam = 1  # and this is the second comment
          # ... and now a third!
text = "# This is not a comment because it's inside quotes."
```

## Python 用作计算器

### 数字

解释器像一个简单的计算器：输入表达式，就会给出答案。表达式的语法很直接：运算符 `+`、`-`、`*`、`/` 的用法和其他大部分语言一样（比如，Pascal 或 C）；括号 (`()`) 用来分组。例如：

```python
>>> 2 + 2
4
>>> 50 - 5*6
20
>>> (50 - 5*6) / 4
5.0
>>> 8 / 5  # division always returns a floating point number
1.6
```

整数（如，`2`、`4`、`20` ）的类型是 [`int`](https://docs.python.org/zh-cn/3/library/functions.html#int)，带小数（如，`5.0`、`1.6` ）的类型是 [`float`](https://docs.python.org/zh-cn/3/library/functions.html#float)。本教程后半部分将介绍更多数字类型。

除法运算（`/`）返回浮点数。用 `//` 运算符执行 [floor division](https://docs.python.org/zh-cn/3/glossary.html#term-floor-division) 的结果是整数（忽略小数）；计算余数用 `%`：

```python
>>> 17 / 3  # classic division returns a float
5.666666666666667
>>>
>>> 17 // 3  # floor division discards the fractional part
5
>>> 17 % 3  # the % operator returns the remainder of the division
2
>>> 5 * 3 + 2  # floored quotient * divisor + remainder
17
```

Python 用 `**` 运算符计算乘方：

`**` 比 `-` 的优先级更高, 所以 `-3**2` 会被解释成 `-(3**2)` ，因此，结果是 `-9`。要避免这个问题，并且得到 `9`, 可以用 `(-3)**2`。

```python
>>> 5 ** 2  # 5 squared
25
>>> 2 ** 7  # 2 to the power of 7
128
```

等号（`=`）用于给变量赋值。赋值后，下一个交互提示符的位置不显示任何结果：

```python
>>> width = 20
>>> height = 5 * 9
>>> width * height
900
```

如果变量未定义（即，未赋值），使用该变量会提示错误：

```python
>>> n  # try to access an undefined variable
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'n' is not defined
```

Python 全面支持浮点数；混合类型运算数的运算会把整数转换为浮点数：

```python
>>> 4 * 3.75 - 1
14.0
```

交互模式下，上次输出的表达式会赋给变量 `_`。把 Python 当作计算器时，用该变量实现下一步计算更简单，例如：

```python
>>> tax = 12.5 / 100
>>> price = 100.50
>>> price * tax
12.5625
>>> price + _
113.0625
>>> round(_, 2)
113.06
```

最好把该变量当作只读类型。不要为它显式赋值，否则会创建一个同名独立局部变量，该变量会用它的魔法行为屏蔽内置变量。

除了 [`int`](https://docs.python.org/zh-cn/3/library/functions.html#int) 和 [`float`](https://docs.python.org/zh-cn/3/library/functions.html#float)，Python 还支持其他数字类型，例如 [`Decimal`](https://docs.python.org/zh-cn/3/library/decimal.html#decimal.Decimal) 或 [`Fraction`](https://docs.python.org/zh-cn/3/library/fractions.html#fractions.Fraction)。Python 还内置支持 [复数](https://docs.python.org/zh-cn/3/library/stdtypes.html#typesnumeric)，后缀 `j` 或 `J` 用于表示虚数（例如 `3+5j` ）。

### 字符串

除了数字，Python 还可以操作字符串。字符串有多种表现形式，用单引号（`'……'`）或双引号（`"……"`）标注的结果相同 。反斜杠 `\` 用于转义：

和其他语言不一样，特殊字符如 `\n` 在单引号（`'...'`）和双引号（`"..."`）里的意义一样。这两种引号唯一的区别是，不需要在单引号里转义双引号 `"`，但必须把单引号转义成 `\'`，反之亦然。

```python
>>> 'spam eggs'  # single quotes
'spam eggs'
>>> 'doesn\'t'  # use \' to escape the single quote...
"doesn't"
>>> "doesn't"  # ...or use double quotes instead
"doesn't"
>>> '"Yes," they said.'
'"Yes," they said.'
>>> "\"Yes,\" they said."
'"Yes," they said.'
>>> '"Isn\'t," they said.'
'"Isn\'t," they said.'
```

交互式解释器会为输出的字符串加注引号，特殊字符使用反斜杠转义。虽然，有时输出的字符串看起来与输入的字符串不一样（外加的引号可能会改变），但两个字符串是相同的。如果字符串中有单引号而没有双引号，该字符串外将加注双引号，反之，则加注单引号。[`print()`](https://docs.python.org/zh-cn/3/library/functions.html#print) 函数输出的内容更简洁易读，它会省略两边的引号，并输出转义后的特殊字符：

```python
>>> '"Isn\'t," they said.'
'"Isn\'t," they said.'
>>> print('"Isn\'t," they said.')
"Isn't," they said.
>>> s = 'First line.\nSecond line.'  # \n means newline
>>> s  # without print(), \n is included in the output
'First line.\nSecond line.'
>>> print(s)  # with print(), \n produces a new line
First line.
Second line.
```

如果不希望前置 `\` 的字符转义成特殊字符，可以使用 *原始字符串*，在引号前添加 `r` 即可：

```python
>>> print('C:\some\name')  # here \n means newline!
C:\some
ame
>>> print(r'C:\some\name')  # note the r before the quote
C:\some\name
```

字符串字面值可以实现跨行连续输入。实现方式是用三引号：`"""..."""` 或 `'''...'''`，字符串行尾会自动加上回车换行，如果不需要回车换行，在行尾添加 `\` 即可。示例如下：

```python
print("""\
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
""")
```

输出如下（注意，第一行没有换行）：

```python
Usage: thingy [OPTIONS]
     -h                        Display this usage message
     -H hostname               Hostname to connect to
```

字符串可以用 `+` 合并（粘到一起），也可以用 `*` 重复：

```python
>>> # 3 times 'un', followed by 'ium'
>>> 3 * 'un' + 'ium'
'unununium'
```

相邻的两个或多个 *字符串字面值* （引号标注的字符）会自动合并：

```python
>>> 'Py' 'thon'
'Python'
```

拆分长字符串时，这个功能特别实用：

```python
>>> text = ('Put several strings within parentheses '
...         'to have them joined together.')
>>> text
'Put several strings within parentheses to have them joined together.'
```

这项功能只能用于两个字面值，不能用于变量或表达式：

```python
>>> prefix = 'Py'
>>> prefix 'thon'  # can't concatenate a variable and a string literal
  File "<stdin>", line 1
    prefix 'thon'
                ^
SyntaxError: invalid syntax
>>> ('un' * 3) 'ium'
  File "<stdin>", line 1
    ('un' * 3) 'ium'
                   ^
SyntaxError: invalid syntax
```

合并多个变量，或合并变量与字面值，要用 `+`：

```python
>>> prefix + 'thon'
'Python'
```

字符串支持 *索引* （下标访问），第一个字符的索引是 0。单字符没有专用的类型，就是长度为一的字符串：

```python
>>> word = 'Python'
>>> word[0]  # character in position 0
'P'
>>> word[5]  # character in position 5
'n'
```

索引还支持负数，用负数索引时，从右边开始计数：

```python
>>> word[-1]  # last character
'n'
>>> word[-2]  # second-last character
'o'
>>> word[-6]
'P'
```

注意，-0 和 0 一样，因此，负数索引从 -1 开始。

除了索引，字符串还支持 *切片*。索引可以提取单个字符，*切片* 则提取子字符串：

```python
>>> word[0:2]  # characters from position 0 (included) to 2 (excluded)
'Py'
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)
'tho'
```

切片索引的默认值很有用；省略开始索引时，默认值为 0，省略结束索引时，默认为到字符串的结尾：

```python
>>> word[:2]   # character from the beginning to position 2 (excluded)
'Py'
>>> word[4:]   # characters from position 4 (included) to the end
'on'
>>> word[-2:]  # characters from the second-last (included) to the end
'on'
```

**注意，输出结果包含切片开始，但不包含切片结束。因此，`s[:i] + s[i:]` 总是等于 `s`：**

```python
>>> word[:2] + word[2:]
'Python'
>>> word[:4] + word[4:]
'Python'
```

还可以这样理解切片，索引指向的是字符 *之间* ，第一个字符的左侧标为 0，最后一个字符的右侧标为 *n* ，*n* 是字符串长度。例如：

```python
 +---+---+---+---+---+---+
 | P | y | t | h | o | n |
 +---+---+---+---+---+---+
 0   1   2   3   4   5   6
-6  -5  -4  -3  -2  -1
```

第一行数字是字符串中索引 0...6 的位置，第二行数字是对应的负数索引位置。*i* 到 *j* 的切片由 *i* 和 *j* 之间所有对应的字符组成。

对于使用非负索引的切片，如果两个索引都不越界，切片长度就是起止索引之差。例如， `word[1:3]` 的长度是 2。

索引越界会报错：

```python
>>> word[42]  # the word only has 6 characters
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
IndexError: string index out of range
```

但是，切片会自动处理越界索引：

```python
>>> word[4:42]
'on'
>>> word[42:]
''
```

Python 字符串不能修改，是 [immutable](https://docs.python.org/zh-cn/3/glossary.html#term-immutable) 的。因此，为字符串中某个索引位置赋值会报错：

```python
>>> word[0] = 'J'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
>>> word[2:] = 'py'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

要生成不同的字符串，应新建一个字符串：

```python
>>> 'J' + word[1:]
'Jython'
>>> word[:2] + 'py'
'Pypy'
```

内置函数 [`len()`](https://docs.python.org/zh-cn/3/library/functions.html#len) 返回字符串的长度：

```python
>>> s = 'supercalifragilisticexpialidocious'
>>> len(s)
34参见
```

### 列表

Python 支持多种 *复合* 数据类型，可将不同值组合在一起。最常用的 *列表* ，是用方括号标注，逗号分隔的一组值。*列表* 可以包含不同类型的元素，但一般情况下，各个元素的类型相同：

```python
>>> squares = [1, 4, 9, 16, 25]
>>> squares
[1, 4, 9, 16, 25]
```

和字符串（及其他内置 [sequence](https://docs.python.org/zh-cn/3/glossary.html#term-sequence) 类型）一样，列表也支持索引和切片：

```python
>>> squares[0]  # indexing returns the item
1
>>> squares[-1]
25
>>> squares[-3:]  # slicing returns a new list
[9, 16, 25]
```

**切片操作返回包含请求元素的新列表。以下切片操作会返回列表的 [浅拷贝](https://docs.python.org/zh-cn/3/library/copy.html#shallow-vs-deep-copy)：**

```python
>>> squares[:]
[1, 4, 9, 16, 25]
```

列表还支持合并操作：

```python
>>> squares + [36, 49, 64, 81, 100]
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

与 [immutable](https://docs.python.org/zh-cn/3/glossary.html#term-immutable) 字符串不同, 列表是 [mutable](https://docs.python.org/zh-cn/3/glossary.html#term-mutable) 类型，其内容可以改变：

```python
>>> cubes = [1, 8, 27, 65, 125]  # something's wrong here
>>> 4 ** 3  # the cube of 4 is 64, not 65!
64
>>> cubes[3] = 64  # replace the wrong value
>>> cubes
[1, 8, 27, 64, 125]
```

`append()` *方法* 可以在列表结尾添加新元素（详见后文）:

```python
>>> cubes.append(216)  # add the cube of 6
>>> cubes.append(7 ** 3)  # and the cube of 7
>>> cubes
[1, 8, 27, 64, 125, 216, 343]
```

**为切片赋值可以改变列表大小，甚至清空整个列表：**

```python
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> letters
['a', 'b', 'c', 'd', 'e', 'f', 'g']
>>> # replace some values
>>> letters[2:5] = ['C', 'D', 'E']
>>> letters
['a', 'b', 'C', 'D', 'E', 'f', 'g']
>>> # now remove them
>>> letters[2:5] = []
>>> letters
['a', 'b', 'f', 'g']
>>> # clear the list by replacing all the elements with an empty list
>>> letters[:] = []
>>> letters
[]
```

内置函数 [`len()`](https://docs.python.org/zh-cn/3/library/functions.html#len) 也支持列表：

```python
>>> letters = ['a', 'b', 'c', 'd']
>>> len(letters)
4
```

还可以嵌套列表（创建包含其他列表的列表），例如：

```python
>>> a = ['a', 'b', 'c']
>>> n = [1, 2, 3]
>>> x = [a, n]
>>> x
[['a', 'b', 'c'], [1, 2, 3]]
>>> x[0]
['a', 'b', 'c']
>>> x[0][1]
'b'
```

## 其他流程控制工具

### if

if 语句包含零个或多个 [`elif`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#elif) 子句，及可选的 [`else`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#else) 子句。关键字 '`elif`' 是 'else if' 的缩写，适用于避免过多的缩进。可以把 `if` ... `elif` ... `elif` ... 序列看作是其他语言中 `switch` 或 `case` 语句的替代品。

### for

```python
>>> for w in words:
...     print(w, len(w))
```

遍历某个集合的同时修改该集合的内容，很难获取想要的结果。要在遍历时修改集合的内容，应该遍历该集合的副本或创建新的集合：

```python
# Strategy:  Iterate over a copy
for user, status in users.copy().items():
    if status == 'inactive':
        del users[user]

# Strategy:  Create a new collection
active_users = {}
for user, status in users.items():
    if status == 'active':
        active_users[user] = status
```

### range()

内置函数 [`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 常用于遍历数字序列，该函数可以生成算术级数：

```python
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4
```

生成的序列不包含给定的终止数值；`range(10)` 生成 10 个值，这是一个长度为 10 的序列，其中的元素索引都是合法的。range 可以不从 0 开始，还可以按指定幅度递增（递增幅度称为 '步进'，支持负数）：

```python
>>> list(range(5, 10))
[5, 6, 7, 8, 9]

>>> list(range(0, 10, 3))
[0, 3, 6, 9]

>>> list(range(-10, -100, -30))
[-10, -40, -70]
```

[`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 和 [`len()`](https://docs.python.org/zh-cn/3/library/functions.html#len) 组合在一起，可以按索引迭代序列：

```python
>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
>>> for i in range(len(a)):
...     print(i, a[i])
...
0 Mary
1 had
2 a
3 little
4 lamb
```

**不过，大多数情况下，[`enumerate()`](https://docs.python.org/zh-cn/3/library/functions.html#enumerate) 函数更便捷，详见 [循环的技巧](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#tut-loopidioms) 。**

如果只输出 range，会出现意想不到的结果：

```python
>>> range(10)
range(0, 10)
```

[`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 返回对象的操作和列表很像，但其实这两种对象不是一回事。迭代时，该对象基于所需序列返回连续项，并没有生成真正的列表，从而节省了空间。

这种对象称为可迭代对象 [iterable](https://docs.python.org/zh-cn/3/glossary.html#term-iterable)，函数或程序结构可通过该对象获取连续项，直到所有元素全部迭代完毕。[`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 语句就是这样的架构，[`sum()`](https://docs.python.org/zh-cn/3/library/functions.html#sum) 是一种把可迭代对象作为参数的函数：

```python
>>> sum(range(4))  # 0 + 1 + 2 + 3
6
```

### 循环中的 `break`、`continue` 语句及 `else` 子句

[`break`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#break) 语句和 C 中的类似，用于跳出最近的 [`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 或 [`while`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#while) 循环。

循环语句支持 `else` 子句；[`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 循环中，可迭代对象中的元素全部循环完毕时，或 [`while`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#while) 循环的条件为假时，执行该子句；[`break`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#break) 语句终止循环时，不执行该子句。 请看下面这个查找素数的循环示例：

```python
>>> for n in range(2, 10):
...     for x in range(2, n):
...         if n % x == 0:
...             print(n, 'equals', x, '*', n//x)
...             break
...     else:
...         # loop fell through without finding a factor
...         print(n, 'is a prime number')
...
2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

（没错，这段代码就是这么写。仔细看：`else` 子句属于 [`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 循环，**不属于** [`if`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#if) 语句。）

与 [`if`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#if) 语句相比，循环的 `else` 子句更像 [`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 的 `else` 子句： [`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 的 `else` 子句在未触发异常时执行，循环的 `else` 子句则在未运行 `break` 时执行。`try` 语句和异常详见 [处理异常](https://docs.python.org/zh-cn/3/tutorial/errors.html#tut-handling)。

[`continue`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#continue) 语句也借鉴自 C 语言，表示继续执行循环的下一次迭代：

```python
>>> for num in range(2, 10):
...     if num % 2 == 0:
...         print("Found an even number", num)
...         continue
...     print("Found an odd number", num)
...
Found an even number 2
Found an odd number 3
Found an even number 4
Found an odd number 5
Found an even number 6
Found an odd number 7
Found an even number 8
Found an odd number 9
```

## 定义函数

下列代码创建一个可以输出限定数值内的斐波那契数列函数：

```python
>>> def fib(n):    # write Fibonacci series up to n
...     """Print a Fibonacci series up to n."""
...     a, b = 0, 1
...     while a < n:
...         print(a, end=' ')
...         a, b = b, a+b
...     print()
...
>>> # Now call the function we just defined:
... fib(2000)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

函数在 *执行* 时使用函数局部变量符号表，所有函数变量赋值都存在局部符号表中；引用变量时，首先，在局部符号表里查找变量，然后，是外层函数局部符号表，再是全局符号表，最后是内置名称符号表。因此，尽管可以引用全局变量和外层函数的变量，但最好不要在函数内直接赋值（除非是 [`global`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#global) 语句定义的全局变量，或 [`nonlocal`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#nonlocal) 语句定义的外层函数变量）。

在调用函数时会将实际参数（实参）引入到被调用函数的局部符号表中；因此，实参是使用 *按值调用* 来传递的（其中的 *值* 始终是对象的 *引用* 而不是对象的值）。 [1](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#id2) 当一个函数调用另外一个函数时，会为该调用创建一个新的局部符号表。

函数定义在当前符号表中把函数名与函数对象关联在一起。解释器把函数名指向的对象作为用户自定义函数。还可以使用其他名称指向同一个函数对象，并访问访该函数：

```python
>>> fib
<function fib at 10042ed0>
>>> f = fib
>>> f(100)
0 1 1 2 3 5 8 13 21 34 55 89
```

`fib` 不返回值，因此，其他语言不把它当作函数，而是当作过程。事实上，没有 [`return`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#return) 语句的函数也返回值，只不过这个值比较是 `None` （是一个内置名称）。一般来说，解释器不会输出单独的返回值 `None` ，如需查看该值，可以使用 [`print()`](https://docs.python.org/zh-cn/3/library/functions.html#print)：

```python
>>> fib(0)
>>> print(fib(0))
None
```

编写不直接输出斐波那契数列运算结果，而是返回运算结果列表的函数也非常简单：

```python
>>> def fib2(n):  # return Fibonacci series up to n
...     """Return a list containing the Fibonacci series up to n."""
...     result = []
...     a, b = 0, 1
...     while a < n:
...         result.append(a)    # see below
...         a, b = b, a+b
...     return result
...
>>> f100 = fib2(100)    # call it
>>> f100                # write the result
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

本例也新引入了一些 Python 功能：

- [`return`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#return) 语句返回函数的值。`return` 语句不带表达式参数时，返回 `None`。函数执行完毕退出也返回 `None`。
- `result.append(a)` 语句调用了列表对象 `result` 的 *方法* 。方法是“从属于”对象的函数，命名为 `obj.methodname`，`obj` 是对象（也可以是表达式），`methodname` 是对象类型定义的方法名。不同类型定义不同的方法，不同类型的方法名可以相同，且不会引起歧义。（用 *类* 可以自定义对象类型和方法，详见 [类](https://docs.python.org/zh-cn/3/tutorial/classes.html#tut-classes) ）示例中的方法 `append()` 是为列表对象定义的，用于在列表末尾添加新元素。本例中，该方法相当于 `result = result + [a]` ，但更有效。

## 函数定义详解

### 4.7.1. 默认值参数

为参数指定默认值是非常有用的方式。调用函数时，可以使用比定义时更少的参数，例如：

```python
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        ok = input(prompt)
        if ok in ('y', 'ye', 'yes'):
            return True
        if ok in ('n', 'no', 'nop', 'nope'):
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)
```

该函数可以用以下方式调用：

- 只给出必选实参：`ask_ok('Do you really want to quit?')`
- 给出一个可选实参：`ask_ok('OK to overwrite the file?', 2)`
- 给出所有实参：`ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')`

本例还使用了关键字 [`in`](https://docs.python.org/zh-cn/3/reference/expressions.html#in) ，用于确认序列中是否包含某个值。

默认值在 *定义* 作用域里的函数定义中求值，所以：

```python
i = 5

def f(arg=i):
    print(arg)

i = 6
f()
```

上例输出的是 `5`。

**重要警告：** 默认值只计算一次。默认值为列表、字典或类实例等可变对象时，会产生与该规则不同的结果。例如，下面的函数会累积后续调用时传递的参数：

```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))
```

输出结果如下：

```
[1]
[1, 2]
[1, 2, 3]
```

不想在后续调用之间共享默认值时，应以如下方式编写函数：

```
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```



### 4.7.2. 关键字参数

`kwarg=value` 形式的 [关键字参数](https://docs.python.org/zh-cn/3/glossary.html#term-keyword-argument) 也可以用于调用函数。函数示例如下：

```python
def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
    print("-- This parrot wouldn't", action, end=' ')
    print("if you put", voltage, "volts through it.")
    print("-- Lovely plumage, the", type)
    print("-- It's", state, "!")
```

该函数接受一个必选参数（`voltage`）和三个可选参数（`state`, `action` 和 `type`）。该函数可用下列方式调用：

```python
parrot(1000)                                          # 1 positional argument
parrot(voltage=1000)                                  # 1 keyword argument
parrot(voltage=1000000, action='VOOOOOM')             # 2 keyword arguments
parrot(action='VOOOOOM', voltage=1000000)             # 2 keyword arguments
parrot('a million', 'bereft of life', 'jump')         # 3 positional arguments
parrot('a thousand', state='pushing up the daisies')  # 1 positional, 1 keyword
```

以下调用函数的方式都无效：

```python
parrot()                     # required argument missing
parrot(voltage=5.0, 'dead')  # non-keyword argument after a keyword argument
parrot(110, voltage=220)     # duplicate value for the same argument
parrot(actor='John Cleese')  # unknown keyword argument
```

函数调用时，关键字参数必须跟在位置参数后面。所有传递的关键字参数都必须匹配一个函数接受的参数（比如，`actor` 不是函数 `parrot` 的有效参数），关键字参数的顺序并不重要。这也包括必选参数，（比如，`parrot(voltage=1000)` 也有效）。不能对同一个参数多次赋值，下面就是一个因此限制而失败的例子：

```python
>>> def function(a):
...     pass
...
>>> function(0, a=0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: function() got multiple values for argument 'a'
```

最后一个形参为 `**name` 形式时，接收一个字典（详见 [映射类型 --- dict](https://docs.python.org/zh-cn/3/library/stdtypes.html#typesmapping)），该字典包含与函数中已定义形参对应之外的所有关键字参数。`**name` 形参可以与 `*name` 形参（下一小节介绍）组合使用（`*name` 必须在 `**name` 前面）， `*name` 形参接收一个 [元组](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#tut-tuples)，该元组包含形参列表之外的位置参数。例如，可以定义下面这样的函数：

```python
def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw, ":", keywords[kw])
```

该函数可以用如下方式调用：

```
cheeseshop("Limburger", "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")
```

输出结果如下：

```
-- Do you have any Limburger ?
-- I'm sorry, we're all out of Limburger
It's very runny, sir.
It's really very, VERY runny, sir.
----------------------------------------
shopkeeper : Michael Palin
client : John Cleese
sketch : Cheese Shop Sketch
```

注意，关键字参数在输出结果中的顺序与调用函数时的顺序一致。

### 4.7.3. 特殊参数

默认情况下，参数可以按位置或显式关键字传递给 Python 函数。为了让代码易读、高效，最好限制参数的传递方式，这样，开发者只需查看函数定义，即可确定参数项是仅按位置、按位置或关键字，还是仅按关键字传递。

函数定义如下：

```
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
      -----------    ----------     ----------
        |             |                  |
        |        Positional or keyword   |
        |                                - Keyword only
         -- Positional only
```

`/` 和 `*` 是可选的。这些符号表明形参如何把参数值传递给函数：位置、位置或关键字、关键字。关键字形参也叫作命名形参。

#### 4.7.3.1. 位置或关键字参数

函数定义中未使用 `/` 和 `*` 时，参数可以按位置或关键字传递给函数。

#### 4.7.3.2. 仅位置参数

此处再介绍一些细节，特定形参可以标记为 *仅限位置*。*仅限位置* 时，形参的顺序很重要，且这些形参不能用关键字传递。仅限位置形参应放在 `/` （正斜杠）前。`/` 用于在逻辑上分割仅限位置形参与其它形参。如果函数定义中没有 `/`，则表示没有仅限位置形参。

`/` 后可以是 *位置或关键字* 或 *仅限关键字* 形参。

#### 4.7.3.3. 仅限关键字参数

把形参标记为 *仅限关键字*，表明必须以关键字参数形式传递该形参，应在参数列表中第一个 *仅限关键字* 形参前添加 `*`。

#### 4.7.3.4. 函数示例

请看下面的函数定义示例，注意 `/` 和 `*` 标记：

```python
>>> def standard_arg(arg):
...     print(arg)
...
>>> def pos_only_arg(arg, /):
...     print(arg)
...
>>> def kwd_only_arg(*, arg):
...     print(arg)
...
>>> def combined_example(pos_only, /, standard, *, kwd_only):
...     print(pos_only, standard, kwd_only)
```

第一个函数定义 `standard_arg` 是最常见的形式，对调用方式没有任何限制，可以按位置也可以按关键字传递参数：

```python
>>> standard_arg(2)
2

>>> standard_arg(arg=2)
2
```

第二个函数 `pos_only_arg` 的函数定义中有 `/`，仅限使用位置形参：

```python
>>> pos_only_arg(1)
1

>>> pos_only_arg(arg=1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: pos_only_arg() got some positional-only arguments passed as keyword arguments: 'arg'
```

第三个函数 `kwd_only_args` 的函数定义通过 `*` 表明仅限关键字参数：

```python
>>> kwd_only_arg(3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: kwd_only_arg() takes 0 positional arguments but 1 was given

>>> kwd_only_arg(arg=3)
3
```

最后一个函数在同一个函数定义中，使用了全部三种调用惯例：

```python
>>> combined_example(1, 2, 3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: combined_example() takes 2 positional arguments but 3 were given

>>> combined_example(1, 2, kwd_only=3)
1 2 3

>>> combined_example(1, standard=2, kwd_only=3)
1 2 3

>>> combined_example(pos_only=1, standard=2, kwd_only=3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: combined_example() got some positional-only arguments passed as keyword arguments: 'pos_only'
```

下面的函数定义中，`kwds` 把 `name` 当作键，因此，可能与位置参数 `name` 产生潜在冲突：

```python
def foo(name, **kwds):
    return 'name' in kwds
```

调用该函数不可能返回 `True`，因为关键字 `'name'` 总与第一个形参绑定。例如：

```python
>>> foo(1, **{'name': 2})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: foo() got multiple values for argument 'name'
>>>
```

加上 `/` （仅限位置参数）后，就可以了。此时，函数定义把 `name` 当作位置参数，`'name'` 也可以作为关键字参数的键：

```python
def foo(name, /, **kwds):
    return 'name' in kwds
>>> foo(1, **{'name': 2})
True
```

换句话说，仅限位置形参的名称可以在 `**kwds` 中使用，而不产生歧义。

#### 4.7.3.5. 小结

以下用例决定哪些形参可以用于函数定义：

```
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
```

说明：

- 使用仅限位置形参，可以让用户无法使用形参名。形参名没有实际意义时，强制调用函数的实参顺序时，或同时接收位置形参和关键字时，这种方式很有用。
- 当形参名有实际意义，且显式名称可以让函数定义更易理解时，阻止用户依赖传递实参的位置时，才使用关键字。
- 对于 API，使用仅限位置形参，可以防止未来修改形参名时造成破坏性的 API 变动。



### 4.7.4. 任意实参列表

调用函数时，使用任意数量的实参是最少见的选项。这些实参包含在元组中（详见 [元组和序列](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#tut-tuples) ）。在可变数量的实参之前，可能有若干个普通参数：

```
def write_multiple_items(file, separator, *args):
    file.write(separator.join(args))
```

`variadic` 实参用于采集传递给函数的所有剩余实参，因此，它们通常在形参列表的末尾。`*args` 形参后的任何形式参数只能是仅限关键字参数，即只能用作关键字参数，不能用作位置参数：

\>>>

```
>>> def concat(*args, sep="/"):
...     return sep.join(args)
...
>>> concat("earth", "mars", "venus")
'earth/mars/venus'
>>> concat("earth", "mars", "venus", sep=".")
'earth.mars.venus'
```



### 4.7.5. 解包实参列表

函数调用要求独立的位置参数，但实参在列表或元组里时，要执行相反的操作。例如，内置的 [`range()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#range) 函数要求独立的 *start* 和 *stop* 实参。如果这些参数不是独立的，则要在调用函数时，用 `*` 操作符把实参从列表或元组解包出来：

```python
>>> list(range(3, 6))            # normal call with separate arguments
[3, 4, 5]
>>> args = [3, 6]
>>> list(range(*args))            # call with arguments unpacked from a list
[3, 4, 5]
```

同样，字典可以用 `**` 操作符传递关键字参数：

```python
>>> def parrot(voltage, state='a stiff', action='voom'):
...     print("-- This parrot wouldn't", action, end=' ')
...     print("if you put", voltage, "volts through it.", end=' ')
...     print("E's", state, "!")
...
>>> d = {"voltage": "four million", "state": "bleedin' demised", "action": "VOOM"}
>>> parrot(**d)
-- This parrot wouldn't VOOM if you put four million volts through it. E's bleedin' demised !
```



### 4.7.6. Lambda 表达式

[`lambda`](https://docs.python.org/zh-cn/3/reference/expressions.html#lambda) 关键字用于创建小巧的匿名函数。`lambda a, b: a+b` 函数返回两个参数的和。Lambda 函数可用于任何需要函数对象的地方。在语法上，匿名函数只能是单个表达式。在语义上，它只是常规函数定义的语法糖。与嵌套函数定义一样，lambda 函数可以引用包含作用域中的变量：

```python
>>> def make_incrementor(n):
...     return lambda x: x + n
...
>>> f = make_incrementor(42)
>>> f(0)
42
>>> f(1)
43
```

上例用 lambda 表达式返回函数。还可以把匿名函数用作传递的实参：

```python
>>> pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda pair: pair[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```



### 4.7.7. 文档字符串

以下是文档字符串内容和格式的约定。

第一行应为对象用途的简短摘要。为保持简洁，不要在这里显式说明对象名或类型，因为可通过其他方式获取这些信息（除非该名称碰巧是描述函数操作的动词）。这一行应以大写字母开头，以句点结尾。

文档字符串为多行时，第二行应为空白行，在视觉上将摘要与其余描述分开。后面的行可包含若干段落，描述对象的调用约定、副作用等。

Python 解析器不会删除 Python 中多行字符串字面值的缩进，因此，文档处理工具应在必要时删除缩进。这项操作遵循以下约定：文档字符串第一行 *之后* 的第一个非空行决定了整个文档字符串的缩进量（第一行通常与字符串开头的引号相邻，其缩进在字符串中并不明显，因此，不能用第一行的缩进），然后，删除字符串中所有行开头处与此缩进“等价”的空白符。不能有比此缩进更少的行，但如果出现了缩进更少的行，应删除这些行的所有前导空白符。转化制表符后（通常为 8 个空格），应测试空白符的等效性。

下面是多行文档字符串的一个例子：

```
>>> def my_function():
...     """Do nothing, but document it.
...
...     No, really, it doesn't do anything.
...     """
...     pass
...
>>> print(my_function.__doc__)
Do nothing, but document it.

    No, really, it doesn't do anything.
```

### 4.7.8. 函数注解

[函数注解](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#function) 是可选的用户自定义函数类型的元数据完整信息（详见 [**PEP 3107**](https://www.python.org/dev/peps/pep-3107) 和 [**PEP 484**](https://www.python.org/dev/peps/pep-0484) ）。

[标注](https://docs.python.org/zh-cn/3/glossary.html#term-function-annotation) 以字典的形式存放在函数的 `__annotations__` 属性中，并且不会影响函数的任何其他部分。 形参标注的定义方式是在形参名后加冒号，后面跟一个表达式，该表达式会被求值为标注的值。 返回值标注的定义方式是加组合符号 `->`，后面跟一个表达式，该标注位于形参列表和表示 [`def`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#def) 语句结束的冒号之间。 下面的示例有一个必须的参数，一个可选的关键字参数以及返回值都带有相应的标注:

\>>>

```
>>> def f(ham: str, eggs: str = 'eggs') -> str:
...     print("Annotations:", f.__annotations__)
...     print("Arguments:", ham, eggs)
...     return ham + ' and ' + eggs
...
>>> f('spam')
Annotations: {'ham': <class 'str'>, 'return': <class 'str'>, 'eggs': <class 'str'>}
Arguments: spam eggs
'spam and eggs'
```



## 小插曲：编码风格

- 缩进，用 4 个空格，不要用制表符。

  4 个空格是小缩进（更深嵌套）和大缩进（更易阅读）之间的折中方案。制表符会引起混乱，最好别用。

- 换行，一行不超过 79 个字符。

  这样换行的小屏阅读体验更好，还便于在大屏显示器上并排阅读多个代码文件。

- 用空行分隔函数和类，及函数内较大的代码块。

- 最好把注释放到单独一行。

- 使用文档字符串。

- 运算符前后、逗号后要用空格，但不要直接在括号内使用： `a = f(1, 2) + g(3, 4)`。

- 类和函数的命名要一致；按惯例，命名类用 `UpperCamelCase`，命名函数与方法用 `lowercase_with_underscores`。命名方法中第一个参数总是用 `self` (类和方法详见 [初探类](https://docs.python.org/zh-cn/3/tutorial/classes.html#tut-firstclasses))。

- 编写用于国际多语环境的代码时，不要用生僻的编码。Python 默认的 UTF-8 或纯 ASCII 可以胜任各种情况。

- 同理，就算多语阅读、维护代码的可能再小，也不要在标识符中使用非 ASCII 字符。

## 数据结构

### 列表推导式

列表推导式创建列表的方式更简洁。常见的用法为，对序列或可迭代对象中的每个元素应用某种操作，用生成的结果创建新的列表；或用满足特定条件的元素创建子序列。

例如，创建平方值的列表：

```python
>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

注意，这段代码创建（或覆盖）变量 `x`，该变量在循环结束后仍然存在。下述方法可以无副作用地计算平方列表：

```python
squares = list(map(lambda x: x**2, range(10)))
```

或等价于：

```python
squares = [x**2 for x in range(10)]
```

上面这种写法更简洁、易读。

列表推导式的方括号内包含以下内容：一个表达式，后面为一个 `for` 子句，然后，是零个或多个 `for` 或 `if` 子句。结果是由表达式依据 `for` 和 `if` 子句求值计算而得出一个新列表。 举例来说，以下列表推导式将两个列表中不相等的元素组合起来：

```python
>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

等价于：

```python
>>> combs = []
>>> for x in [1,2,3]:
...     for y in [3,1,4]:
...         if x != y:
...             combs.append((x, y))
...
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

注意，上面两段代码中，[`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 和 [`if`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#if) 的顺序相同。

表达式是元组（例如上例的 `(x, y)`）时，必须加上括号：

```python
>>> vec = [-4, -2, 0, 2, 4]
>>> # create a new list with the values doubled
>>> [x*2 for x in vec]
[-8, -4, 0, 4, 8]
>>> # filter the list to exclude negative numbers
>>> [x for x in vec if x >= 0]
[0, 2, 4]
>>> # apply a function to all the elements
>>> [abs(x) for x in vec]
[4, 2, 0, 2, 4]
>>> # call a method on each element
>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
['banana', 'loganberry', 'passion fruit']
>>> # create a list of 2-tuples like (number, square)
>>> [(x, x**2) for x in range(6)]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
>>> # the tuple must be parenthesized, otherwise an error is raised
>>> [x, x**2 for x in range(6)]
  File "<stdin>", line 1, in <module>
    [x, x**2 for x in range(6)]
               ^
SyntaxError: invalid syntax
>>> # flatten a list using a listcomp with two 'for'
>>> vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

列表推导式可以使用复杂的表达式和嵌套函数：

```python
>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

### 嵌套的列表推导式

列表推导式中的初始表达式可以是任何表达式，甚至可以是另一个列表推导式。

下面这个 3x4 矩阵，由 3 个长度为 4 的列表组成：

```python
>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
```

下面的列表推导式可以转置行列：

```python
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

如上节所示，嵌套的列表推导式基于其后的 [`for`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#for) 求值，所以这个例子等价于：

```python
>>> transposed = []
>>> for i in range(4):
...     transposed.append([row[i] for row in matrix])
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

反过来说，也等价于：

```python
>>> transposed = []
>>> for i in range(4):
...     # the following 3 lines implement the nested listcomp
...     transposed_row = []
...     for row in matrix:
...         transposed_row.append(row[i])
...     transposed.append(transposed_row)
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

实际应用中，最好用内置函数替代复杂的流程语句。此时，[`zip()`](https://docs.python.org/zh-cn/3/library/functions.html#zip) 函数更好用：

```python
>>> list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]
```

### 集合

集合是由不重复元素组成的无序容器。基本用法包括成员检测、消除重复元素。集合对象支持合集、交集、差集、对称差分等数学运算。

创建集合用花括号或 [`set()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#set) 函数。注意，创建空集合只能用 `set()`，不能用 `{}`，`{}` 创建的是空字典 

```python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # show that duplicates have been removed
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # fast membership testing
True
>>> 'crabgrass' in basket
False

>>> # Demonstrate set operations on unique letters from two words
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # unique letters in a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # letters in a but not in b
{'r', 'd', 'b'}
>>> a | b                              # letters in a or b or both
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # letters in both a and b
{'a', 'c'}
>>> a ^ b                              # letters in a or b but not both
{'r', 'd', 'b', 'm', 'z', 'l'}
```

### 字典

[`dict()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#dict) 构造函数可以直接用键值对序列创建字典：

```python
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

### 循环的技巧

在字典中循环时，用 `items()` 方法可同时取出键和对应的值：

```python
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
...
gallahad the pure
robin the brave
```

在序列中循环时，用 [`enumerate()`](https://docs.python.org/zh-cn/3/library/functions.html#enumerate) 函数可以同时取出位置索引和对应的值：

```python
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe
```

同时循环两个或多个序列时，用 [`zip()`](https://docs.python.org/zh-cn/3/library/functions.html#zip) 函数可以将其内的元素一一匹配：

```python
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

逆向循环序列时，先正向定位序列，然后调用 [`reversed()`](https://docs.python.org/zh-cn/3/library/functions.html#reversed) 函数：

```python
>>> for i in reversed(range(1, 10, 2)):
...     print(i)
...
9
7
5
3
1
```

按指定顺序循环序列，可以用 [`sorted()`](https://docs.python.org/zh-cn/3/library/functions.html#sorted) 函数，在不改动原序列的基础上，返回一个重新的序列：

```python
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for i in sorted(basket):
...     print(i)
...
apple
apple
banana
orange
orange
pear
```

使用 [`set()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#set) 去除序列中的重复元素。使用 [`sorted()`](https://docs.python.org/zh-cn/3/library/functions.html#sorted) 加 [`set()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#set) 则按排序后的顺序，循环遍历序列中的唯一元素：

```python
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for f in sorted(set(basket)):
...     print(f)
...
apple
banana
orange
pear
```

**一般来说，在循环中修改列表的内容时，创建新列表比较简单，且安全：**

```python
>>> import math
>>> raw_data = [56.2, float('NaN'), 51.7, 55.3, 52.5, float('NaN'), 47.8]
>>> filtered_data = []
>>> for value in raw_data:
...     if not math.isnan(value):
...         filtered_data.append(value)
...
>>> filtered_data
[56.2, 51.7, 55.3, 52.5, 47.8]
```

## 深入条件控制

`while` 和 `if` 条件句不只可以进行比较，还可以使用任意运算符。

比较运算符 `in` 和 `not in` 校验序列里是否存在某个值。运算符 `is` 和 `is not` 比较两个对象是否为同一个对象。所有比较运算符的优先级都一样，且低于数值运算符。

**比较操作支持链式操作。例如，`a < b == c` 校验 `a` 是否小于 `b`，且 `b` 是否等于 `c`。**

**注意，Python 与 C 不同，在表达式内部赋值必须显式使用 [海象运算符](https://docs.python.org/zh-cn/3/faq/design.html#why-can-t-i-use-an-assignment-in-an-expression) `:=`。 这避免了 C 程序中常见的问题：要在表达式中写 `==` 时，却写成了 `=`**

## 模块

```python
import fibo
```

这项操作不直接把 `fibo` 函数定义的名称导入到当前符号表，只导入模块名 `fibo` 。要使用模块名访问函数：

```python
>>> fibo.fib(1000)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
>>> fibo.fib2(100)
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
>>> fibo.__name__
'fibo'
```

如果经常使用某个函数，可以把它赋值给局部变量：

```python
>>> fib = fibo.fib
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

[`import`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#import) 语句有一个变体，可以直接把模块里的名称导入到另一个模块的符号表。例如：

```python
>>> from fibo import fib, fib2
>>> fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

**这种方式会导入所有不以下划线（`_`）开头的名称。大多数情况下，不要用这个功能，这种方式向解释器导入了一批未知的名称，可能会覆盖已经定义的名称。**

模块名后使用 `as` 时，直接把 `as` 后的名称与导入模块绑定。

```python
>>> import fibo as fib
>>> fib.fib(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

[`from`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#from) 中也可以使用这种方式，效果类似：

```python
>>> from fibo import fib as fibonacci
>>> fibonacci(500)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377
```

### [`dir()`](https://docs.python.org/zh-cn/3/library/functions.html#dir) 函数

内置函数 [`dir()`](https://docs.python.org/zh-cn/3/library/functions.html#dir) 用于查找模块定义的名称。返回结果是经过排序的字符串列表：

```
>>> import fibo, sys
>>> dir(fibo)
['__name__', 'fib', 'fib2']
>>> dir(sys)  
['__breakpointhook__', '__displayhook__', '__doc__', '__excepthook__',
 '__interactivehook__', '__loader__', '__name__', '__package__', '__spec__',
 '__stderr__', '__stdin__', '__stdout__', '__unraisablehook__',
 '_clear_type_cache', '_current_frames', '_debugmallocstats', '_framework',
 '_getframe', '_git', '_home', '_xoptions', 'abiflags', 'addaudithook',
 'api_version', 'argv', 'audit', 'base_exec_prefix', 'base_prefix',
 'breakpointhook', 'builtin_module_names', 'byteorder', 'call_tracing',
 'callstats', 'copyright', 'displayhook', 'dont_write_bytecode', 'exc_info',
 'excepthook', 'exec_prefix', 'executable', 'exit', 'flags', 'float_info',
 'float_repr_style', 'get_asyncgen_hooks', 'get_coroutine_origin_tracking_depth',
 'getallocatedblocks', 'getdefaultencoding', 'getdlopenflags',
 'getfilesystemencodeerrors', 'getfilesystemencoding', 'getprofile',
 'getrecursionlimit', 'getrefcount', 'getsizeof', 'getswitchinterval',
 'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
 'intern', 'is_finalizing', 'last_traceback', 'last_type', 'last_value',
 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path', 'path_hooks',
 'path_importer_cache', 'platform', 'prefix', 'ps1', 'ps2', 'pycache_prefix',
 'set_asyncgen_hooks', 'set_coroutine_origin_tracking_depth', 'setdlopenflags',
 'setprofile', 'setrecursionlimit', 'setswitchinterval', 'settrace', 'stderr',
 'stdin', 'stdout', 'thread_info', 'unraisablehook', 'version', 'version_info',
 'warnoptions']
```

没有参数时，[`dir()`](https://docs.python.org/zh-cn/3/library/functions.html#dir) 列出当前定义的名称：

```
>>> a = [1, 2, 3, 4, 5]
>>> import fibo
>>> fib = fibo.fib
>>> dir()
['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

注意，该函数列出所有类型的名称：变量、模块、函数等。

[`dir()`](https://docs.python.org/zh-cn/3/library/functions.html#dir) 不会列出内置函数和变量的名称。这些内容的定义在标准模块 [`builtins`](https://docs.python.org/zh-cn/3/library/builtins.html#module-builtins) 里：

```
>>> import builtins
>>> dir(builtins)  
['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException',
 'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning',
 'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError',
 'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning',
 'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False',
 'FileExistsError', 'FileNotFoundError', 'FloatingPointError',
 'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError',
 'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError',
 'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError',
 'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented',
 'NotImplementedError', 'OSError', 'OverflowError',
 'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError',
 'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning',
 'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError',
 'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError',
 'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError',
 'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning',
 'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__',
 '__debug__', '__doc__', '__import__', '__name__', '__package__', 'abs',
 'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable',
 'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits',
 'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit',
 'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr',
 'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass',
 'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview',
 'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property',
 'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice',
 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars',
 'zip']
```

## 格式化字符串字面值

格式说明符是可选的，写在表达式后面，可以更好地控制格式化值的方式。下例将 pi 舍入到小数点后三位：

```
>>> import math
>>> print(f'The value of pi is approximately {math.pi:.3f}.')
The value of pi is approximately 3.142.
```

在 `':'` 后传递整数，为该字段设置最小字符宽度，常用于列对齐：

```
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
>>> for name, phone in table.items():
...     print(f'{name:10} ==> {phone:10d}')
...
Sjoerd     ==>       4127
Jack       ==>       4098
Dcab       ==>       7678
```

还有一些修饰符可以在格式化前转换值。 `'!a'` 应用 [`ascii()`](https://docs.python.org/zh-cn/3/library/functions.html#ascii) ，`'!s'` 应用 [`str()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#str)，`'!r'` 应用 [`repr()`](https://docs.python.org/zh-cn/3/library/functions.html#repr)：

```
>>> animals = 'eels'
>>> print(f'My hovercraft is full of {animals}.')
My hovercraft is full of eels.
>>> print(f'My hovercraft is full of {animals!r}.')
My hovercraft is full of 'eels'.
```

## 读写文件

通常，文件以 *text mode* 打开，即，从文件中读取或写入字符串时，都以指定编码方式进行编码。如未指定编码格式，默认值与平台相关 (参见 [`open()`](https://docs.python.org/zh-cn/3/library/functions.html#open))。在 mode 中追加的 `'b'` 则以 *binary mode* 打开文件：此时，数据以字节对象的形式进行读写。该模式用于所有不包含文本的文件。

在文本模式下读取文件时，默认把平台特定的行结束符（Unix 上为 `\n`, Windows 上为 `\r\n`）转换为 `\n`。在文本模式下写入数据时，默认把 `\n` 转换回平台特定结束符。这种操作方式在后台修改文件数据对文本文件来说没有问题，但会破坏 `JPEG` 或 `EXE` 等二进制文件中的数据。注意，在读写此类文件时，一定要使用二进制模式。

## 处理异常

[`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 语句的工作原理如下：

- 首先，执行 *try 子句* （[`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 和 [`except`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#except) 关键字之间的（多行）语句）。
- 如果没有触发异常，则跳过 *except 子句*，[`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 语句执行完毕。
- 如果执行 try 子句时发生了异常，则跳过该子句中剩下的部分。如果异常的类型与 [`except`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#except) 关键字后面的异常匹配，则执行 except 子句，然后，继续执行 [`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 语句之后的代码。
- 如果发生的异常不是 except 子句中列示的异常，则将其传递到外部的 [`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 语句中；如果没有找到处理程序，则它是一个 *未处理异常*，语句执行将终止，并显示如上所示的消息。

[`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 语句可以有多个 except 子句，可为不同异常指定相应的处理程序。但最多只会执行一个处理程序。处理程序只处理对应的 try 子句中发生的异常，而不处理同一 `try` 语句内其他处理程序中的异常。except 子句可以用元组命名多个异常，例如：

```python
... except (RuntimeError, TypeError, NameError):
...     pass
```

[`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) ... [`except`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#except) 语句支持可选的 *else 子句*，该子句必须放在所有 except 子句之后。如果 try 子句没有触发异常，但又必须执行一些代码时，这个子句很有用。例如：

```
for arg in sys.argv[1:]:
    try:
        f = open(arg, 'r')
    except OSError:
        print('cannot open', arg)
    else:
        print(arg, 'has', len(f.readlines()), 'lines')
        f.close()
```

使用 `else` 子句比向 [`try`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#try) 子句添加额外的代码要好，可以避免意外捕获非 `try` ... `except` 语句保护的代码触发的异常。

## 作用域和命名空间示例

这个例子演示了如何引用不同作用域和名称空间，以及 [`global`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#global) 和 [`nonlocal`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#nonlocal) 会如何影响变量绑定:

```python
def scope_test():
    def do_local():
        spam = "local spam"

    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal spam"

    def do_global():
        global spam
        spam = "global spam"

    spam = "test spam"
    do_local()
    print("After local assignment:", spam)
    do_nonlocal()
    print("After nonlocal assignment:", spam)
    do_global()
    print("After global assignment:", spam)

scope_test()
print("In global scope:", spam)
```

示例代码的输出是：

```python
After local assignment: test spam
After nonlocal assignment: nonlocal spam
After global assignment: nonlocal spam
In global scope: global spam
```

请注意 *局部* 赋值（这是默认状态）不会改变 *scope_test* 对 *spam* 的绑定。 [`nonlocal`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#nonlocal) 赋值会改变 *scope_test* 对 *spam* 的绑定，而 [`global`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#global) 赋值会改变模块层级的绑定。

您还可以发现在 [`global`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#global) 赋值之前没有 *spam* 的绑定。

# Python数据处理

NumPy库提供多维数组ndarray，其**所有元素类型必须相同**，且**大小固定**，在创建时定义，**使用过程中不可改变**。一般采用如下方式导入NumPy库：

\>>> import numpy as np

为避免函数命名冲突，后续使用Numpy时使用np代替。

## 数组

### 一维数组

np.array(xxx).ndim    #数组维度 

np.array(xxx).size    #数组元素的个数 

np.array(xxx).ndim    #数组的数据类型

### 二维数组

```python
arr = np.array([[1,2],[3,4]])
print(arr.ndim) # 数组维度 2
print(arr.size) # 数组元素总数 = 行数*列数 4
print(arr.shape) # 数组的行数和列数 (2,2)
print(arr.dtype) # 数组元素的类型 int32
```

访问指定行、列的元素，给定行和列两个索引值

```python
arr = np.array([[1,2],[3,4,5,6],[7,8,9]])
print(arr)
# [list([1, 2]) list([3, 4, 5, 6]) list([7, 8, 9])]
print(arr[2][2])
# 9
# 上面这是一维的，不过元素是list对象,访问arr[0,1]报错,不贴error信息
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr)
"""
[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]
"""
print(arr[2,3])
# 12
print(arr[[0,1],[1,2],[2,3]])
# IndexError: too many indices for array
a = arr[[0,2],[1,3]]
print(type(a))
# <class 'numpy.ndarray'>
print(arr[[0,2],[1,3]])
# [ 2 12]
```

- 上例在方括号中给出行切片[0,2]和列切片[1,3]，表示抽取行序号为0、列序号为1，以及行序号为2、列序号为3的元素，**得到一维的ndarray数组**

访问部分行元素，给出行列表即可，**列索引的":"可以省略**

```python
print(arr[[0,1]])
"""
[[1 2 3 4]
 [5 6 7 8]]
"""
print(arr[[0,1],:])
"""
[[1 2 3 4]
 [5 6 7 8]]
"""
```

访问部分列元素，**前面的行索引“:"不能省略**，否则无法识别是对列切片

```python
print(arr[,[0,1]])
# SyntaxError: invalid syntax
print(arr[:,[0,1]])
"""
[[ 1  2]
 [ 5  6]
 [ 9 10]]
"""
print(arr[0:2:1,[0,1]])
# [[1 2]]
print(arr[0:2:1,[0,1]])
"""
[[1 2]
 [5 6]]
"""
print(arr[0:3:1,[0,1]])
"""
[[ 1  2]
 [ 5  6]
 [ 9 10]]
"""
```

- 后面几条尝试了前面提到的start: end: step形式，可以看出end是不包含的，也就是区间是[start,end)

**访问部分行和列数据**

```python
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
print(arr[ [0,2] , 1:3 ]) # 0、2行中1~2列的所有元素
"""
[[ 2  3]
 [10 11]]
"""
```

如果需要抽取指定**某些行中指定列**的所有元素，则需要进行**两层切片**

```python
print(arr[0,1][:,[0,3]])
# IndexError: invalid index to scalar variable.
print(arr[[0,1]][:,[0,3]])
"""
[[1 4]
 [5 8]]
"""
#首先arr[[0,1]]抽取指定0、1行组成的二维ndarray对象，再在此对象上进行切片操作，取所有行中0、3列的元素
```

条件筛选

可以使用布尔型数组筛选访问其他数组的元素。用于筛选的布尔数组，需要具有与访问数组相同的函数或列数。

```python
flag = np.array(['one','two','three'])
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
selector = (flag == 'one') | (flag == 'three')
print(selector)
print(arr[selector])
"""
[ True False  True]
[[ 1  2  3  4]
 [ 9 10 11 12]]
"""
flag = np.array(['one','two','three','four'])
arr = np.array([[1,2,3,4],[5,6,7,8],[9,10,11,12]])
selector = (flag == 'one') | (flag == 'three')
print(selector)
print(arr[:,selector])
"""
[ True False  True False]
[[ 1  3]
 [ 5  7]
 [ 9 11]]
"""
```

#### [2.1.3 创建多维数组的常用方法](https://ashiamd.github.io/docsify-notes/#/study/Python/python数据处理?id=_213-创建多维数组的常用方法)

1. arange()函数

   生成指定头、尾、步长的等差数组，**参数可以是浮点数**

   ```python
   np.arange(4)
   # array([0, 1, 2, 3])
   np.arange(0,4)
   # array([0, 1, 2, 3])
   np.arange(0,5,2)
   # array([0, 2, 4])
   np.arange(0.3,2.2,0.3)
   # array([0.3, 0.6, 0.9, 1.2, 1.5, 1.8, 2.1])
   ```

2. reshape()函数

   将一维数组转换为指定的多维数组

   ```python
   # arr.reshape(m,n) 获取arr转换为m行n列的新数组
   arr1 = np.array([1,2,3,4,5,6,7,8,9])
   arr2 = arr1.reshape(3,3)
   print(arr2)
   """
   [[1 2 3]
    [4 5 6]
    [7 8 9]]
   """
   ```

3. zeros()函数和ones()函数

   zeros()函数和ones()函数生成指定大小全0和全1的数组。

   ```python
   print(np.zeros(5))
   print(np.ones(4))
   """
   [0. 0. 0. 0. 0.]
   [1. 1. 1. 1.]
   """
   print(np.zeros((3,4)))
   print(np.ones((4,3)))
   """
   [[0. 0. 0. 0.]
    [0. 0. 0. 0.]
    [0. 0. 0. 0.]]
   [[1. 1. 1.]
    [1. 1. 1.]
    [1. 1. 1.]
    [1. 1. 1.]]
   """
   ```

二维数组与一维数组运算

```python
arr = np.zeros((2,4))
arr += np.array([1,2,3,4])
print(arr)
"""
[[1. 2. 3. 4.]
 [1. 2. 3. 4.]]
"""
```

选定元素运算

```python
flag = np.array([3,4,2,5])
arr = np.zeros((4,6))
arr[flag > 3] += np.array([1,2,3,4,5,6])
print(flag>3)
print(arr)
"""
[False  True False  True]
[[0. 0. 0. 0. 0. 0.]
 [1. 2. 3. 4. 5. 6.]
 [0. 0. 0. 0. 0. 0.]
 [1. 2. 3. 4. 5. 6.]]
```

#### [2.2.2 函数和矩阵运算](https://ashiamd.github.io/docsify-notes/#/study/Python/python数据处理?id=_222-函数和矩阵运算)

1. 通用函数(ufunc)

   - 常用的一元函数(eg: np.abs(arr)等 )

   函数描述abs、fabs计算整数、浮点数或负数的绝对值sqrt计算各元素的平方根square计算各元素的平方exp计算各元素的指数log、log10自然对数、底数为10的logsign计算各元素的正负号ceil计算各元素的ceiling值，即大于或等于该值的最小整数floor计算各元素的floor值，即小于或等于该值的最大整数cos、cosh、sin、sinh、tan、tanh普通和双曲型三角函数

   - 常用的二元函数

   函数描述add将数据中对应的元素相加subtract从第一个数组中践去第二个数组中的元素multiply数组元素相乘divide数组对应元素相除power对第一个数组中的元素A，根据第二个数组中的相应元素B，计算ABmod元素级的求模运算copysign将第二个数组中的值的符号复制给第一个数组中的值equal,not_equal执行元素级的比较运算，产生布尔型数组

2. 聚焦函数

   - 常用的聚集函数

   函数描述sum求和mean算数平均值min、max最小值和最大值argmin、argmax最小值和最大值的索引cumsum从0开始向前累加各元素cumprod从1开始向前累乘各元素

   - 聚焦函数的使用样例(下面数据看看就好，主要了解参数)

   ```python
   #导入numpy
   import numpy as np
   
   #创建一维数组
   names = np.array(['王微', '肖良英', '方绮雯', '刘旭阳','钱易铭'])
   subjects = np.array(['Math', 'English', 'Python', 'Chinese','Art', 'Database', 'Physics'])
   #创建二维数组
   scores = np.array([[70,85,77,90,82,84,89],[60,64,80,75,80,92,90],[90,93,88,87,86,90,91],[80,82,91,88,83,86,80],[88,72,78,90,91,73,80]])
   
   # 1)统计不同科目的成绩总分
   # 首先利用布尔型数组选择“王微”的所有成绩，然后使用求平均值函数mean()
   scores.sum(axis = 0) # 按列求和
   # array([388, 396, 414, 430, 422, 425, 430])
   
   # 2)求“王微”所有课程成绩的平均分
   scores[names == '王微'].mean()
   # 82.42857142857143
   
   # 3)查询英语考试成绩最高的同学的姓名
   names[ scores[:,subjects == 'English'].argmax() ]
   # argmax()函数能返回特定元素的下标。首先通过列筛选得到由所有学生英语成绩组成的一维数组，接着通过argmax()函数返回一维数组中最高分的索引值，最后利用该索引值在names数组中查找到该学生的姓名
   ```

   > 对于二维数组对象，可以指定聚集函数是在行上操作还是在列上操作。**当参数axis为0时，函数操作的对象是同一列不同行的数组元素；当参数axis为1时，函数操作的对象是同一行不同列的数组元素**。

#### [2.2.3 随机数组生成函数](https://ashiamd.github.io/docsify-notes/#/study/Python/python数据处理?id=_223-随机数组生成函数)

- 常用函数

  函数描述random随机产生[0,1)之间的浮点值randint随机生成给定范围的一组整数uniform随机生成给定范围内服从均匀分布的一组浮点数choice在给定的序列内随机选择元素normal随机生成一组服从给定均值和方差的正态分布随机数

  ```python
  # 生成5*6的二维随机整数，随机数的取值是0或1
  np.random.randint(0,2,size = (5,6))
  """
  array([[0, 0, 0, 0, 1, 0],
         [1, 0, 1, 0, 0, 0],
         [1, 1, 1, 1, 1, 0],
         [1, 0, 1, 1, 0, 0],
         [0, 0, 1, 1, 0, 0]])
  """
  
  # 生成均值为0、方差为1服从正太分布的4*5二维数组
  np.random.normal(0,1,size = (4,5))
  """
  array([[ 0.27348588, -1.06847557, -0.84807463, -0.82102134,  0.31033654],
         [-0.76208868,  0.9522973 ,  0.53462609, -0.07880294,  1.08901491],
         [-1.45998785, -1.72776158, -1.09198982, -0.91469086, -1.2952753 ],
         [ 0.56222164, -1.13944672, -0.64216053, -0.03491689,  0.44231984]])
  """
  ```