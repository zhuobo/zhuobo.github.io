---
title: python函数
date: 2019-06-13 06:43:05
tags: python
categories: python
---

## 一、定义函数

使用 `def`语句定义函数，依次写出函数名、括号、参数、冒号，然后在缩进的代码块中写函数体，使用`return`语句返回。当没有`return`语句时，默认提供一个，但是返回的是`None`，返回None可以简写为`return`。

```Python
def sum_of_two(x, y):
    return x + y
```

1. 空函数，如果一个函数什么都不做，或者暂时还没想好怎么实现，只是希望程序可以跑起来，那么可以使用`pass`语句。

   ```python
   def nop():
   	pass
   ```

2. 多个返回值

   Python可以提供多个返回值，事实上返回的是一个tuple

   ```Python
   def function(x, y):
       return x, y
   ```

## 二、函数的参数

### 1. 位置参数

在调用函数时，根据函数定义的参数位置来传递参数，参数的个数不能少、**位置要一一对应**。

### 2. 默认参数

定义函数时给出某些参数的默认值，在调用的的时候不用传递这些有默认值的参数，这些参数将会使用默认值，当然也可以传递。要注意的是，无论是定义函数，还是调用函数默认参数都必须在位置参数之后。

```Python
def pow(x, n = 2):
    ans = 1
    while n >= 1:
        ans *= x
        n -= 1
    return ans

print(pow(3)) # 9
print(pow(3, 3)) # 27
```

默认参数应该指向一个不可变的对象，比如整数、字符串、None等，如果默认参数指向了可变的对象比如list，一旦list在函数中被改变，那么下次调用函数使用的默认参数就是被改变了的参数，出现不希望出现的情况。

### 3. 可变参数

可变参数指的是传递的参数个数可以变化，可以是任意个参数。在没有使用可变参数的方法定义函数的时候，又希望函数的参数可以变化，那么就可以将函数参数定义为一个list。

```python
def calc(nums):
    ans = 0
    for num in nums:
        ans += num * num;
    return ans
```

这样在调用的时候也可以传递多个参数，但是要先组装为一个list

```Python
print(calc([1, 2, 3]))
```

在上面定义函数的基础上，参数前加一个`*`,表示是可变参数，函数体都不需要改变即可直接在调用函数的时候传递任意个参数。

```Python
def calc(*nums):
    ans = 0
    for num in nums:
        ans += num * num;

    return ans
```

调用函数，不需要组装list，或者tuple

```Python
print(calc(1, 2, 3))
```

函数定义为可变参数之后，如果参数又是一个list，那么还是可以直接传递list作为参数，只需要在list名之前加一个`*`，这就表示把list的元素变为可变参数传递到函数，并且在调用的时候自动组装成一个`tuple，`*list`表示把list的元素作为可变参数传递给函数。

```Python
list = (1, 2, 3, 4)
print(calc(*list))
```

### 4. 关键字参数

可变参数允许传递任意个参数，在函数调用的时候自动组装为一个tuple，二关键字参数也是允许传递任意个**含有参数名的参数**，在函数调用的时候自动组装为一个`dict`。

```Python
def show_user(username, age, **kw):
    print('username:', username, 'age:', age, 'other:',kw)

show_user('zhuobo', 19)

show_user('zhuobo', 19, city = '广州')

show_user('zhuobo', 19, city = '广州', gender = 'male')
```

输出：

```Python
username: zhuobo age: 19 other: {}
username: zhuobo age: 19 other: {'city': '广州'}
username: zhuobo age: 19 other: {'city': '广州', 'gender': 'male'}
```

当然在传递关键字参数的时候，也可以先组装一个dict，然后使用类似于`*list`的方式传递关键字参数。`**kw`表示把dict的所有`key-value`用关键字参数传递到函数的`**kw`参数，`**kw`将获取一份dict的拷贝。

```Python
dict = {'city':'广州', 'gender':'male'}
show_user('zhuobo', 19, **dict)
```

### 5. 命名关键字参数

对于关键子参数，调用函数的时候可以传递任意个关键字参数，如果要限制关键字参数的名字，就需要**命名关键字参数**。和关键字参数`**kw`所不同的是，需要在位置参数和命名关键字参数之间写一个`*`作为分隔符号。如：要限制输入的关键字参数只能是`city`，`hobby`，那么就可以这么定义函数：

```Python
def show_user(username, age, *, city, hobby):
    print(username, age, city, hobby)
```

调用方式：

```Python
show_user('zhuobo', 19, city = 'Guangzhou', hobby = 'football')
```

输出：

```Python
zhuobo 19 Guangzhou football
```

如果是函数已经定义了一个可变参数，那么就无需使用`*`作为分隔符，直接写命名关键字参数即可

```Python
def show_user(username, age, *args, city, hobby):
    print(username, age, city, hobby)
```

**注意**：

1. 函数调用时必须，命名关键字参数必须写上参数名，否则解释器无法区分位置参数和命名关键字参数；
2. 如果没有可变参数，位置参数和命名关键字参数之间必须使用`*`作为分隔符；



