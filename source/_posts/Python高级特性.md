---
title: Python高级特性
date: 2019-06-14 22:42:22
tags: python
categories: python
---

## 一、切片

一般来说，获取list或者tuple的元素是一个重复性很高的操作，比如要获取前3个元素，一是可以使用索引获取`list[0]`,`list[1]`,`list[2]`，但是这样无法将操作扩展到N；二是加一个索引，使用for循环：

```Python
names = ['Alice', 'Bob', 'Cidy', 'David', 'Enny', 'File']
for i in range(6):
	print(i, list[i])
```

切片是Python提供的一种获取元素更加简洁的方式，使用切片操作符，可以方便地获取list或者tuple的元素。格式：

[开始索引:结束索引:步长值]

开始索引：索引从0开始，最后一个元素的索引是-1；

结束索引：切片操作到结束索引结束，不包括结束索引；

步长值：步长值默认是1，正整数表示从左向右，负整数表示从右向左；

练习：

1. 复制一个list

   ```Python
   # 获取全部元素
   names = list[:]
   ```

2. 获取前N个元素

   ```python
   names[:5]
   ```

3. 获取后N个元素

   ```python
   names[-5:]
   ```

4. 每隔3个取1个

   ```python
   names[::3]
   ```

5. 获取最后一个元素

   ```
   names[-1]
   ```

6. 自定义trim函数，删除字符串的收尾空格

   ```Python
   def trim(string):
       if len(string) != 0:
           while string[:1] == ' ':
               string = string[1:]
           while string[-1:] == ' ':
               string = string[:-1]
       return string
   
   s = trim('  asdfsadf  ')
   print(s)
   ```

**注意**：

1. tuple是一种不可变的list，tuple也可以进行切片操作，结果还是tuple；
2. 字符串也可以进行切片操作，结果还是字符串；

## 二、迭代

在Python中，迭代通过`for xxx in xxx`完成，无论是有下标的list、tuple还是没有下标的dict、字符串都可以进行迭代。

1. 迭代list

   ```Python
   for name in names:
       print(name)
   ```

2. 迭代dict

   ```python
   # 1. 默认是迭代key
   for k in dict：
   	print(k)
       
   # 2. 迭代value
   for v in dict.values():
       print(v)
       
   # 3. 同时迭代key、value
   for k, v in dict.items():
   	print(k, v)
   ```

3. 迭代字符串

   ```python
   str = 'abcdefg'
   for ch in str:
   	print(ch)
   ```

## 三、列表生成式

1. Python内置的用来生成list列表的的生成式，比如生成`n^2`：

    ```python
    [x * x for x in range(1, 11)]

    # [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    ```

2. 列表生成器还可以搭配判断语句：

    ```python
    [x * x for x in range(1, 11) if x % 2 == 0]

    # [4, 16, 36, 64, 100]
    ```

3. 双层循环，生成全排列：
	
	```Python
	[m + n for n in 'abc' for m in '123']
	
	# ['1a', '2a', '3a', '1b', '2b', '3b', '1c', '2c', '3c']
	```
	
4. 使用两个变量生成list：

    ```Python
    dict = {'aaa': '1', 'bbb': '2', 'ccc': '3'}
    [k + '=' + v for k,v in dict.items()]
    
    # ['aaa=1', 'bbb=2', 'ccc=3']
    ```

## 四、生成器

列表生成式可以生成列表，但是列表需要占用大空间、容量有限；生成器（generator）可以在循环的过程中推算出后续的元素，不必建立完整的列表。创建生成器和创建列表生成式的方法一样，只是`[]`改为`()`。

```Python
# l是一个list， g是一个generator

>>> l = [x * x for x in range(1, 11)]
>>> g = (x * x for x in range(1, 11))
```

要获取generator的下一个返回值，可以使用`next(g)`，每次调用`next(g)`就计算出下一个元素的值，直到没有更多的元素，抛出 `StopIteration`异常。

```python
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
36
>>> next(g)
49
>>> next(g)
64
>>> next(g)
81
>>> next(g)
100
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

**注意：**generator是一个可迭代的对象，也就是可以使用`for xxx in xxx`迭代generator，并且会自动迭代到最后一个元素，而不用关心`StopIteration`异常。

```Python
>>> for x in g:
...     print(x)
... 
1
4
9
16
25
36
49
64
81
100
```

如果是定义一个函数，输出斐波那契数列，函数的定义应该如下：

```python 
>>> def fib(max):
...     n, a, b = 0, 1, 1
...     while n < max:
...             print(b)
...             a, b = b, a + b
...             n += 1
...     return 'none'
```

把函数中的`print(b)`换为`yield(b)`,这个函数便成为了一个生成器（generator），这是定义生成器的另一种方法。如下定义，`g`便是一个生成器。这个生成器的逻辑是：在循环中不断调用`yield()`,遇到`yield()`就中断并返回，下次再从上次返回的`yield`语句处继续执行。

```Python
>>> def fib(max):
...     n, a, b = 0, 1, 1
...     while n < max:
...             yield(b)
...             a, b = b, a + b
...             n += 1
...     return 'none'
... 
>>> g = fib(5)
>>> g
<generator object fib at 0x7f9e6c715a98>
```



## 五、迭代器

可以直接作用于`for`循环的有两种数据类型，一是集合数据类型，如`list`、`tuple`、`str`、`dict`、`set`；二`是generator`，如生成器，含有`yield`语句的generator Function。这些可以直接作用于`for`循环的数据类型称为可迭代对象：`Iterable`。可以使用`isinstance()`判断一个对象否是`Iterable`：

```Python
>>> from collections import Iterable
>>> isinstance(22,Iterable)
False
>>> isinstance('asdf', Iterable)
True
>>> isinstance(['a', 'b', 'c'], Iterable)
True
>>> isinstance((x * x for x in range(10)), Iterable)
True
```

**生成器**不但是可以直接作用于for循环的`Iterable`，而且可以被`next()`函数不断调用，返回下一个值，直到抛出异常`StopIteration`表示没有下一个值了，无法继续返回。这种可以被`next()`不断调用并返回下一个值的对象称为`迭代器`：`Iterator`。可以使用`isinstance()`判断一个对象是否是迭代器：

```Python
>>> from collections import Iterator
>>> isinstance([], Iterator)
False
>>> isinstance((), Iterator)
False
>>> isinstance((x*x for x in range(10)), Iterator)
True
>>> isinstance('asdf', Iterator)
False
```

由此可以知道，生成器都是`Iterator`，而且也是`Iterable`；`list`、`tuple`、`str`、`dict`、`set`是`Iterable`，但不是`Iterator`。可以使用`iter()`函数把`list`、`tuple`、`str`、`dict`、`set`等`Iterable`转换为`Iterator`。

```Python
>>> isinstance(iter([]),Iterator)
True
>>> isinstance(iter(()),Iterator)
True
>>> isinstance(iter({}),Iterator)
True
>>> isinstance(iter('asdf'),Iterator)
True
```

