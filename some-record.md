### 学习知识点整理：

##### #1

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠\来实现多行语句，例如：

```python
total = item_one + \
        item_two + \
        item_three
```

在 [], {}, 或 () 中的多行语句，不需要使用反斜杠\，例如：

```python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

+++

##### #2

反斜杠可以用来转义，使用 **r** 可以让反斜杠不发生转义。 如 ：

```python
r"this is a line with \n" 
```

则 **\n** 会显示，并不是换行。

+++

##### #3

Python 中的字符串有两种索引方式，从左往右以 **0** 开始，从右往左以 **-1** 开始。

字符串可以用 **+** 运算符连接在一起，用 ***** 运算符重复。

python中的范围从start开始，取不到end，step默认为1

Python 字符串不能被改变，向一个索引位置赋值，比如 **word[0] = 'm'** 会导致错误

string的截取的语法格式如下：**变量[头下标:尾下标:步长]**——>可以用string[:-1]去掉一个string的末尾

+++

##### #4

**print** 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end="xxx"**

+++

##### #5

在 python 用 **import** 或者 **from...import** 来导入相应的模块。

将整个模块(somemodule)导入，格式为： 

```python
import somemodule
```

从某个模块中导入某个函数,格式为：

```python
 from somemodule import somefunction
```

导入多个函数,格式为： 

```python
from somemodule import firstfunc, secondfunc,thirdfunc
```

将某个模块中的全部函数导入，格式为：

```python
from somemodule import 
```

+++

##### #6

python中可以同时为多个变量赋值：

```python
a = b = c = 1
```

也可以为多个对象指定多个变量：

```python
a, b, c = 1, 2, "runoob"
```

+++

##### #7

Python3 中常见的数据类型有：Number（数字）、String（字符串）、bool（布尔类型）、List（列表）、Tuple（元组）、Set（集合）、Dictionary（字典）

不可变数据（3 个）：Number（数字）、String（字符串）、Tuple（元组）

可变数据（3 个）：List（列表）、Dictionary（字典）、Set（集合）

内置的 type() 函数可以用来查询变量所指的对象类型，如：

```python
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```

+++

##### #8

List(列表)是写在方括号 **[]** 之间、用逗号分隔开的元素列表，索引值以 **0** 为开始值，**-1** 为从末尾的开始位置，加号 **+** 是列表连接运算符，星号 ***** 是重复操作，列表中的元素可以改变，如：

```python
>>> a = [1, 2, 3, 4, 5, 6]
>>> a[0] = 9
>>> a[2:5] = [13, 14, 15]
>>> a
[9, 2, 13, 14, 15, 6]
>>> a[2:5] = []   # 将对应的元素值设置为 [] 
>>> a
[9, 2, 6]
```

+++

##### #9

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 **()** 里，元素之间用逗号隔开，元组中的元素类型也可以不相同。

构造包含 0 个或 1 个元素的元组比较特殊，所以有一些额外的语法规则：

```python
tup1 = ()    # 空元组
tup2 = (20,) # 一个元素，需要在元素后添加逗号
```

+++

##### #10

Python 中的集合（Set）是一种无序、可变的数据类型，用于存储唯一的元素，可以进行交集、并集、差集等常见的集合操作。

在 Python 中，集合使用大括号 **{}** 表示，元素之间用逗号 , 分隔，创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典

```python
sites = {'Google', 'Taobao', 'Runoob', 'Facebook', 'Zhihu', 'Baidu'}

print(sites)   # 输出集合，重复的元素被自动去掉

# 成员测试
if 'Runoob' in sites :
    print('Runoob 在集合中')
else :
    print('Runoob 不在集合中')


# set可以进行集合运算
a = set('abracadabra')
b = set('alacazam')

print(a)

print(a - b)     # a 和 b 的差集

print(a | b)     # a 和 b 的并集

print(a & b)     # a 和 b 的交集

print(a ^ b)     # a 和 b 中不同时存在的元素
```

输出结果：

```python
{'Zhihu', 'Baidu', 'Taobao', 'Runoob', 'Google', 'Facebook'}
Runoob 在集合中
{'b', 'c', 'a', 'r', 'd'}
{'r', 'b', 'd'}
{'b', 'c', 'a', 'z', 'm', 'r', 'l', 'd'}
{'c', 'a'}
{'z', 'b', 'm', 'r', 'l', 'd'}
```

+++

##### #11

字典（dictionary）是Python中一种无序的对象集合，用 **{ }** 标识，是一个无序的 **键(key) : 值(value)** 的集合，键(key)必须使用不可变类型。在同一个字典中，键(key)必须是唯一的。



