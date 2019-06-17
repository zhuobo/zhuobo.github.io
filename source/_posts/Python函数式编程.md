---
title: Python函数式编程
date: 2019-06-15 16:05:05
tags: python
categories: python
---

## 一、高阶函数

在说什么事高阶函数之前，首先理解几个概念：

1. 变量可以指向函数，比如python内置的一个函数`min()`,参数为一个list或者tuple，返回值为最小值。那么可以用一个变量指向这个函数名，该变量便指向了函数本身，与原来的函数名`min`一样可以调用：

```python
>>> min
<built-in function min>
>>> f = min
>>> f
<built-in function min>
>>> f([1, 2, 3, 4, 9, 0])
0
```

2. 函数名也是变量，也就是一个函数的函数名可以赋值为其他内容，甚至可这样做，改变内置函数的名字，但是最好不要这么改变，函数名一般来说都是有意义的：

```python
>>> min, max = max, min
>>> max
>>> <built-in function min>
>>> min
>>> <built-in function max>
```

   

**高阶函数**：既然变量可以指向函数，函数的可以接收变量作为参数，那么函数就可以接收函数作为参数，这种以函数为参数的函数称为高阶函数。

```python
# 函数f接收函数g作为参数
>>> def f(x, y, g):
...     return g(x) + g(y)
...
>>> def g(x):
...     return x * x
···
>>> f(2, 3, g)
13
```

### 1. map/reduce

python内置的`map()`、`reduce()`函数，这个两个函数都是高阶函数：

1. map()：`map()`函数接收两个参数，一个是函数，另一个`Iterable`，`map()`函数将传入的函数依次作用到序列的每个元素，并且返回一个`Iterator`。比如：

```python
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1,2,3,4,5,6,7,8,9])
# list函数，将Iterator转换为list
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]


# 将序列里的数转为字符串
>>> list(map(f,[1,2,3,4,5,6,7,8,9]))
['1', '2', '3', '4', '5', '6', '7', '8', '9']
```

2. reduce()：`reduce()`也是把一个函数作用在一个序列上，接收两个参数，一个是函数，另一个是`Iterable`，`reduce()`函数把结果继续和序列的下一个元素做**累积**运算，效果是：

```python
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

利用`map`、`reduce`函数自定义一个将`str`转为`int`的函数：

```python
>>> from functools import reduce
>>> digits = {'1':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9, '0':0}
>>> def str2int(str):
...     def char2num(c):
...             return digits[c]
...     def f(x, y):
...             return x * 10 + y
...     return reduce(f, map(char2num, str))
...
>>> str2int('123')
123
```



### 2. filter

python内置的函数`filter()`也是一个高阶函数，用于过滤序列。与`map`函数类似，`filter`函数接收两个参数，一个是函数，另一个是序列，`filter`将参数的函数作用在序列的每个元素上，并根据返回值是`True`还是`False`决定保留元素在序列，或者将元素删除。**要注意**的是`filter`函数也是返回一是`Iterator`，一个惰性的列表。如删除一个列表的奇数，保留偶数：

```python
>>> def is_even(num):
...     return num % 2 == 0
...
>>> list(filter(is_even, [1,2,3,4,5,6,7,8,9,10]))
[2, 4, 6, 8, 10]
```

### 3. sorted

python内置的排序函数`sorted()`也是一个高阶函数，该函数可以用于对list排序：

```python
>>> sorted([-5, 4, -3, 2, 1])
[-5, -3, 1, 2, 4]
```

由于`sorted()`是一个高阶函数，也可以接收一个函数作为参数，实现自定义的排序。**key**指定的函数将作用在list的每一个元素上，并根据key函数的返回值的结果进行排序。如按照绝对值大小排序：

```python
>>> sorted([-5, 4, -3, 2, 1], key = abs)
[1, 2, -3, 4, -5]
```

如果要反向排序，则可以传入第3个参数`reverse = True`：

```python
>>> sorted([-5, 4, -3, 2, 1], key = abs, reverse = True)
[-5, 4, -3, 2, 1]
```





## 二、返回函数

### 1. 函数作为返回值

高阶函数除了可以接收函数作为函数的参数之外，还可以将函数作为返回值，如下面的一个求和的函数，返回的是一个函数：

```python
>>> def lazy_sum(*args):
...     def sum():
...             ans = 0
...             for num in args:
...                     ans += num
...             return ans
...     return sum
...
>>> f = lazy_sum(1,2,3,4)
>>> f
<function lazy_sum.<locals>.sum at 0x0000026D7D6977B8>
```

用f去接收这个函数，发现f是一个函数，f这个函数在被调用的时候就会执行：

```python
>>> f()
10
```

要注意的是，哪怕传递的参数是一样的，返回的函数对象还是不一样的对象：

```python
>>> f1 = lazy_sum(1,2,3,4)
>>> f2 = lazy_sum(1,2,3,4)
>>> f1
<function lazy_sum.<locals>.sum at 0x0000026D7CC5C1E0>
>>> f2
<function lazy_sum.<locals>.sum at 0x0000026D7D6978C8>
```

### 2. 闭包

在上面的例子中，函数`lazy_sum`内部嵌套了另一个函数`sum`，并且将sum这个函数作为返回值返回。注意到函数`sum`是引用了函数`lazy_sum`的局部变量，并且在`lazy_sum`返回后还在引用`args`。在这里`f`就是一个**闭包**，闭包本质上就是一个函数，它包含了**sum函数**，以及**局部变量args**。闭包使得这些局部变量在lazy_sum执行后依然可以保存在内存中，可以访问。

要**注意**的是，返回一个函数时，被返回的函数没有被立刻执行，而是等到调用才执行：

```python
>>> def count():
...     fs = []
...     for i in range(1, 4):
...             def f():
...                     return i * i
...             fs.append(f)
...     return fs
...
>>> L = count()
>>> f1, f2, f3 = L

>>> f1
<function count.<locals>.f at 0x0000026D7D697A60>
>>> f2
<function count.<locals>.f at 0x0000026D7D697AE8>
>>> f3
<function count.<locals>.f at 0x0000026D7D697B70>

# 输出都是9，这是因为当三个函数全部被返回的时候，局部变量i已经是3了，调用f便全都返回9
>>> f3()
9
>>> f1()
9
>>> f2()
9
```



## 三、匿名函数

在将函数作为参数的时候，不需要专门定义一个函数，可以使用匿名函数代替。

```python
l = list(map(lambda x: x*x, [1,2,3,4,5,6,7,8,9]))
print(l)

# 事实上，lambda x: x*x就相当于
def fun(x):
    return x * x
```

关键字`lambda`表示匿名函数，`:`前的x表示匿名函数的参数，`:`后的表达式表示返回值。匿名函数的限制就是只能有一个表达式，不用写`return`语句

匿名函数也是一个函数对象可以赋值给另一个变量，可以作为函数的返回值，当然可以作为参数：

```python
# 匿名函数赋值给变量
>>> f = lambda x: x * x * x
>>> f
<function <lambda> at 0x00000277D38DC1E0>
>>> f(2)
8

# 匿名函数作为函数返回值
>>> def fun(x):
...     return lambda: x * x
...
>>> f = fun(3)
>>> f()
9
```

## 四、装饰器

装饰器本质上是一个python函数或者类，用于为已经存在的对象获取函数添加额外的功能。如一个函数`foo`:

```python
def foo():
	print('i am foo')
```

如果希望在函数调用后打印日志，可以这样在函数体添加日志代码：

```python
def foo():
	print('i am foo')
	logging.warning('foo is running')
```

但是这样做的问题就是，如果很多函数都需要在执行的时候打印日志，那么就要为每个函数添加日志代码，装饰器可以很好的解决这个问题。

```python
def log(func):
    def wrapper(*args, **wk):
        print('%s() is running' % func.__name__)
        return func(*args, **wk)
	return wrapper
```

​	`log()`函数就是一个装饰器，该装饰器接收一个函数作为参数，并返回一个函数，这样借助python的@语法，就可以将装饰器定义在函数，在这里`@log`相当于执行了赋值语句 `foo = log(foo)`。

```python
@log
def foo():
	print('i am foo')
```

调用`foo()`函数，不仅执行`foo()`函数本身，还会在foo函数执行之前打印日志：

```python
>>> @log
... def foo():
...     print('i am foo')
...
>>> foo()
foo() is running
i am foo
```

