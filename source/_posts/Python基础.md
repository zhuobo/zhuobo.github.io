---
title: python基础
date: 2019-06-12 06:43:05
tags: python
categories: python
---

## 一、数据类型和变量

### 1.数据类型

1. 整数
2. 浮点数
3. 字符串，字符串使用单引号`''`,或者双引号`""`括起来，如果字符串中包含单引号或者双引号，那么应该转义字符`\`标记；
4. 布尔值，只有`False`、`True`，注意首字母大写，布尔值的运算与或非使用`and`，`or`，`not`表示；
5. 空值，使用`None`表示空值，空值不是`0`，就是表示什么都不是，而`0`是一个整数，有意义的；

### 2. 变量

Python的变量类型是不固定的，被成为动态语言，与之对应的是静态语言。静态语言在定义的时候必须指定数据类型，如果为变量赋值，数据类型不匹配还会报错，java、C++等便是静态语言。

**常量**指的是不能变的变量，也即是值是固定的，Python中一般使用全部大写表示常量，如：`PI = 3.141592653`。但是这只是一种约定俗成，如果非要改变`PI`的值也是可以。

## 二、字符和编码

1. ASCII码：使用**1个字节**存储数据，包括英文字符、数字、一些常见英文符号;

2. GB2312：中国制定的包括中文的编码

3. Unicode：为了将所有语言统一到一个编码，以解决不同地区使用不同编码集出现的乱码问题而制定的编码。Unicode编码通常是2个字节，如果非常生僻的字符可能会用到4个字节。

4. UTF-8：**可编长编码**，Unicode解决了乱码问题，但是会使本来只用一个字节编码的常用英文字母、数字等字符占用存储空间变为两倍。因此UTF-8将常用英文字母、数字编码为1个字节、中文汉字通常为3个字节、很生僻的字符会用到4-6个字节。

   一般来说，在计算机内存中使用Unicode编码，当需要保存到硬盘或者在网络上传输就转换为UTF-8**。**

5. **格式化输出字符串**：Python使用类似于C语言的方式格式化字符串，在字符串的内部，`%s`表示用字符串替换，`%d`表示用整数替换。如果有多个占位符，后面使用 % 运算符再跟上相应的变量，注意要一一对应。

   ```python
   'My name is %s, and I have got %d yuan for my work!' % ('zhuobo', salary)
   ```

   常用的占位符：

   | 占位符 |    替换    |
   | :----: | :--------: |
   |   %s   |   字符串   |
   |   %d   |    整数    |
   |   %f   |   浮点数   |
   |   %x   | 十六进整数 |

   其中，替换的整数或者浮点数还可以指定是否在前面补0，或者占用的位数。

   占用3位：

   ```Python
   '占用3位的整数：%3d' % 5
   #输出：占用3位的整数：  5'
   ```

   前面补0：

   ```python
   '占用3位的整数：%03d' % 5
   #输出：占用3位的整数：005'
   ```

## 三、list和tuple

### 1. list

list（列表）是Python内置的一种数据类型，是有序的集合，可以随机存取其中的元素。

1. list使用中括号将数据括起来，数据之间用`,`分隔;

2. 索引从0开始，最后一个元素是`list[-1]`，也可以是`list[len(list) - 1]`;

3. list是一个有序的可变长度列表，可以使用`append()`追加元素;

   ```python
   list.append('hello')
   ```

4. 使用`insert()`将元素插入到指定的位置，以下表示插入到`0号`位置

   ```python
   list.insert(0, 'hello')
   ```

5. 使用`pop()`删除末尾元素，返回值为被删除元素

   ```python
   # 空参数表示删除末尾元素
   var = list.pop()
   ```

6. 使用`pop(pos)`删除指定位置元素，`pos`为指定的索引，返回值为被删除元素

   ```Python
   # 参数表示指定位置
   var = list.pop(pos)
   ```

7. 一个list可以存储不同类型的元素

   ```python
   list = ['asdf', 12, 13.4]
   ```

8. list里面也可以存储list，如果要获取`alice`，相当于一个二维数组

   ```Python
   >>> list
   ['asdf', 12, 13.4, ['david', 'alice']]
   >>> len(list)
   4
   >>> list[3][1] # 二维数组
   'alice'
   ```

### 2. tuple

tuple是另一种有序列表，使用`()`包裹起来，与list的区别是一旦tuple被初始化就不能修改，因此没有`append()`、`insert()`等修改元素的方法。tuple和list一样可以获取元素，但是不能被重新赋值为另外的元素，tuple不能修改的意义在于可以增强安全性，如果可以，就应该尽量使用tuple而不是list。**要注意的是，定义一个tuple，在定义的时候就必须初始化，也就是确定tuple的元素。**

```python
# 定义一个元组
tuple1 = ('david', 13, 'alice', 14.5)
# 定义一个空的元组
tuple2 = ()
# 定义一个只有1个元素的元组，当这个元素是数值时，为了区别数值，所有一个元素的元组在那个元素后面加一个逗号
tuple3 = (1,) # 这是一个元组1个元素的元组，该元素是数字1
tuple4 = (1) # 这是整数1, 不是元组
```



tuple不变指的是每个索引指向的元素不变，也就是本来tuple的元素是`'a'`,那么就一直是 `'a'`，而不能变成来其他的元素，如果指向的是一个可变的元素，比如list，那么list还是可以改变，这时候改变的是list的元素，而不是tuple的元素。

## 四、条件判断

if_else句式：

```python
if e:
	do something
else:
	do something
```

if_elif_else句式：

```python
if e1:
	do something
elif e2:
	do something
elif e3:
	do something
else:
	do something
```

## 五、循环

### 1. `for x in ...`循环

`for x in ...`循环用于遍历list或者tuple，迭代取出存储其中的元素。	

```python
# for 循环

names = ['zhuobo', 'david', 'alice']
for name in names:
    print('Hello,', name)
```

```python
# list函数生成list，range函数生成生成序列
sum = 0
mylist = list(range(101))
for num in mylist:
    sum += num

print(sum)
```

使用索引的for循环，使用函数`enumerate()`，事实上是将list组装成了类似于[('aaa', 1), ('bbb', 2), ('ccc', 3)]这种形式。

```Python
names = ['zhuobo', 'david', 'alice']

for index, name in enumerate(names):
    print(index,':', name)
```



### 2.`while`循环

当循环条件不再满足，跳出循环

```Python
# 100以内的奇数的和
n = 1
sum = 0
while n < 100:
    sum += n
    n += 2

print(sum)
```

### 3. break、continue

`break`控制直接跳出循环、`continue	`控制跳过本轮循环，继续下一轮循环，这两个语句一般都会配合判断语句使用，符合条件就执行。



## 六、dict、set

### 1. dict

1. Python内置了对字典的支持，字典在其他语言中成为`Map`，也就是使用`键-值`对存储的数据结构。

	```Python
    d = {'Alice': 80, 'Zhuobo':90, 'David':88}

    print('Alice:', d['Alice'])
	```

2. 字典中的**键**不允许重复，可以使用`in`判断是键是否在字典中：

   ```Python
   >>> 'zhuobo' in d
   False
   >>> 'Zhuobo' in d
   True
   ```

3. 除了使用索引的方法获取值，字典还提供了`get()`方法获取值，如果`key`不存在就返回`None`，也可以返回指定的值。

   ```Python
   >>> d.get('zhuobo', -1)
   -1
   >>> d.get('Zhuobo')
   90
   ```

要注意的是，作为key的数据必须是不可变的，比如数字、字符串等，但是list是可变的数据，因此不可作为字典的键。这是因为字典使用哈希算法根据键获取值，而如果键是可变的，哈希的结果会变化。



### 2. set

创建一个set，需要提供一个list，set的元素不重复，没有顺序。

```python
>>> set(['zhuob', 12, 14.2, 12, 12])
{12, 14.2, 'zhuob'}
```

1. `add(key)`方法添加数据

   ```Python
   >>> mySet.add('Alice')
   >>> mySet
   {'Alice', 12, 14.2, 'zhuob'}
   ```

2. remove(key)方法删除数据

   ```Python
   >>> mySet.remove(12)
   >>> mySet
   {'Alice', 14.2, 'zhuob'}
   ```

3. 并集、交集

   ```Python
   >>> s1 = set([1, 2, 3])
   >>> s2 = set([4, 2, 3])
   >>> s1 & s2
   {2, 3}
   >>> s1 | s2
   {1, 2, 3, 4}
   ```

4. 集合也不可存储可变数据，因为这样无法判断两个数据是否相等，无法去重。