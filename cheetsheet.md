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

 

##### #2

反斜杠可以用来转义，使用 **r** 可以让反斜杠不发生转义。 如 ：

```python
r"this is a line with \n" 
```

则 **\n** 会显示，并不是换行。

 

##### #3

Python 中的字符串有两种索引方式，从左往右以 **0** 开始，从右往左以 **-1** 开始。

字符串可以用 **+** 运算符连接在一起，用 ***** 运算符重复。

python中的范围从start开始，取不到end，step默认为1

Python 字符串不能被改变，向一个索引位置赋值，比如 **word[0] = 'm'** 会导致错误

string的截取的语法格式如下：**变量[头下标:尾下标:步长]**——>可以用string[:-1]去掉一个string的末尾

 

##### #4

**print** 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end="xxx"**

 

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

 

##### #6

python中可以同时为多个变量赋值：

```python
a = b = c = 1
```

也可以为多个对象指定多个变量：

```python
a, b, c = 1, 2, "runoob"
```

 

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

 

##### #9

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 **()** 里，元素之间用逗号隔开，元组中的元素类型也可以不相同。

构造包含 0 个或 1 个元素的元组比较特殊，所以有一些额外的语法规则：

```python
tup1 = ()    # 空元组
tup2 = (20,) # 一个元素，需要在元素后添加逗号
```

 

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

 

##### #11

字典（dictionary）是Python中一种无序的对象集合，用 **{ }** 标识，是一个无序的 **键(key) : 值(value)** 的集合，键(key)必须使用不可变类型。在同一个字典中，键(key)必须是唯一的。

```python
tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}
```

构造函数 dict() 可以直接从键值对序列中构建字典如下：

```python
>>> dict([('Runoob', 1), ('Google', 2), ('Taobao', 3)])
{'Runoob': 1, 'Google': 2, 'Taobao': 3}
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
>>> dict(Runoob=1, Google=2, Taobao=3)
{'Runoob': 1, 'Google': 2, 'Taobao': 3}
```



##### #12

海象运算符’：=‘在表达式中同时进行赋值和返回赋值的值

```python
# 传统写法
n = 10
if n > 5:
    print(n)

# 使用海象运算符
if (n := 10) > 5:
    print(n)
```



##### #13

位运算符：

```python
a = 60            # 60 = 0011 1100 
b = 13            # 13 = 0000 1101 
c = 0
 
c = a & b        # 12 = 0000 1100
 
c = a | b        # 61 = 0011 1101 
 
c = a ^ b        # 49 = 0011 0001
 
c = ~a           # -61 = 1100 0011
 
c = a << 2       # 240 = 1111 0000
 
c = a >> 2       # 15 = 0000 1111
```



##### #14

身份运算符is、is not：

**x is y**, 类似 **id(x) == id(y)** ，比较对象内存地址

is 与 == 区别：

is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。

```python
>>>a = [1, 2, 3]
>>> b = a
>>> b is a 
True
>>> b == a
True
>>> b = a[:]
>>> b is a
False
>>> b == a
True
```



##### #15

数字相关的数学函数：

```
abs()	#内置函数，任意数据类型
fabs()	#math模块导入，浮点数绝对值
ceil()	#上取整
floor()	#下取整
log(真数,底数)
max()/min()	#参数可以为序列
round()	#返回浮点数四舍五入值
```



##### #16

随机数函数：

```
random()	#随机生成一个[0,1)范围内实数
random.choice(range(n))	#[0,n-1]随机整数
shuffle(lst)	#序列所有元素随机排列
uniform(x,y)	#随机生成[x,y]内实数
```



##### #17

字符串格式化符号：

| %c   | 格式化字符及其ASCII码                |
| ---- | ------------------------------------ |
| %s   | 格式化字符串                         |
| %d   | 格式化整数                           |
| %u   | 格式化无符号整型                     |
| %o   | 格式化无符号八进制数                 |
| %x   | 格式化无符号十六进制数               |
| %X   | 格式化无符号十六进制数（大写）       |
| %f   | 格式化浮点数字，可指定小数点后的精度 |
| %e   | 用科学计数法格式化浮点数             |
| %E   | 作用同%e，用科学计数法格式化浮点数   |
| %g   | %f和%e的简写                         |
| %G   | %f 和 %E 的简写                      |
| %p   | 用十六进制数格式化变量的地址         |



##### #18

**f-string**:

以 **f** 开头，后面跟着字符串，字符串中的表达式用大括号 {} 包起来:

```python
>>> name = 'Runoob'
>>> f'Hello {name}'  # 替换变量
'Hello Runoob'
>>> f'{1+2}'         # 使用表达式
'3'

>>> w = {'name': 'Runoob', 'url': 'www.runoob.com'}
>>> f'{w["name"]}: {w["url"]}'
'Runoob: www.runoob.com'
```



##### #19

Python 的字符串内建函数:

| 1    | capitalize()将字符串的第一个字符转换为大写                   |
| ---- | ------------------------------------------------------------ |
| 2    | **count(str, beg= 0,end=len(string))**返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数 |
| 3    | **endswith(suffix, beg=0, end=len(string))** 检查字符串是否以 suffix 结束，如果 beg 或者 end 指定则检查指定的范围内是否以 suffix 结束，如果是，返回 True,否则返回 False。 |
| 4    | **find(str, beg=0, end=len(string))** 检测 str 是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1 |
| 5    | **isalnum()** 如果字符串至少有一个字符并且所有字符都是字母或数字则返 回 True，否则返回 False |
| 6    | **isalpha()** 如果字符串至少有一个字符并且所有字符都是字母或中文字则返回 True, 否则返回 False |
| 7    | **isdigit()**如果字符串只包含数字则返回 True 否则返回 False.. |
| 8    | **islower()**如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False |
| 9    | **isupper()**如果字符串中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False |
| 10   | **str.join(item)**以指定字符串作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串 |
| 11   | **len(string)**返回字符串长度                                |
| 12   | **lower()** 转换字符串中所有大写字符为小写.                  |
| 13   | **replace(old,new,max)**把 将字符串中的 old 替换成 new,如果 max 指定，则替换不超过 max 次。 |
| 14   | **split(str="", num=string.count(str))**以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num+1 个子字符串 |
| 15   | **swapcase()** 将字符串中大写转换为小写，小写转换为大写      |



##### #20

嵌套列表：

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

列表比较需要引入 **operator** 模块的 **eq** 方法：

```python
import operator

a = [1, 2]
b = [2, 3]

operator.eq(a,b)	#False
```

python中包含以下方法：

| 1    | list.append(obj) 在列表末尾添加新的对象                      |
| ---- | ------------------------------------------------------------ |
| 2    | **list.count(obj)**统计某个元素在列表中出现的次数            |
| 3    | **list.extend(seq)**在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| 4    | **list.index(obj)**从列表中找出某个值第一个匹配项的索引位置  |
| 5    | **list.insert(index, obj)**将对象插入列表                    |
| 6    | **list.pop(index=-1)**移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| 7    | **list.remove(obj)**移除列表中某个值的第一个匹配项           |
| 8    | **list.reverse()**反向列表中元素                             |
| 9    | **list.sort( key=None, reverse=False))**对原列表进行排序     |
| 10   | **list.clear()**清空列表                                     |
| 11   | **list.copy()**复制列表                                      |



##### #21

元组：元组使用小括号 ( )

创建空元组：

```python
tup1 = ()
```

只有一个元素：

```python
tuple=(element1,)	#貌似和前面重复了QAQ
```

元组不可以根据索引进行修改，但是可以对整个元组进行连接、删除等



##### #22

字典：

```python
d = {key1 : value1, key2 : value2, key3 : value3 }
```

键必须是唯一的、不可变的，可以用数字，字符串或元组充当，而用列表就不行。值不必唯一，不必不可变

添加键值对：

```python
tinydict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
 
tinydict['Age'] = 8               # 更新 Age
tinydict['School'] = "菜鸟教程"  # 添加信息
```

删除字典：

```python
tinydict = {'Name': 'Runoob', 'Age': 7, 'Class': 'First'}
 
del tinydict['Name'] # 删除键 'Name'
tinydict.clear()     # 清空字典
del tinydict         # 删除字典
```

不允许同一个键出现两次。创建时如果同一个键被赋值两次，只有后一个值会被记住

Python字典包含了以下内置方法：

| 1    | dict.clear()删除字典内所有元素                               |
| ---- | ------------------------------------------------------------ |
| 2    | **dict.copy()** 返回一个字典的浅复制                         |
| 3    | **dict.get(key, default=None)** 返回指定键的值，如果键不在字典中返回 default 设置的默认值 |
| 4    | **key in dict** 如果键在字典dict里返回true，否则返回false    |
| 5    | **popitem()**返回并删除字典中的最后一对键和值。              |



##### #23

集合（set）是一个无序的不重复元素序列，可以进行交集、并集、差集等操作，可以使用大括号 **{ }** 创建集合，元素之间用逗号 **,** 分隔， 或者也可以使用 **set()** 函数创建集合

```python
set1 = {1, 2, 3, 4}            # 直接使用大括号创建集合
set2 = set([4, 5, 6, 7])      # 使用 set() 函数从列表创建集合
```

创建一个空集合必须用 **set()** 而不是 **{ }**，因为 **{ }** 是用来创建一个空字典

```python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # 这里演示的是去重功能
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # 快速判断元素是否在集合内
True
>>> 'crabgrass' in basket
False

>>> # 下面展示两个集合间的运算.

>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # 集合a中包含而集合b中不包含的元素
{'r', 'd', 'b'}
>>> a | b                              # 集合a或b中包含的所有元素
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # 集合a和b中都包含了的元素
{'a', 'c'}
>>> a ^ b                              # 不同时包含于a和b的元素
{'r', 'd', 'b', 'm', 'z', 'l'}
```

添加元素：

```python
set1.add(item)	#random添加
```

移除元素：

```python
set1.remove(item)	#元素不存在会报错
set1.discard(item)	#元素不存在可以删除空气
set1.pop()	#随机删除一个元素
```



##### #24

循环语句：

for…else 在循环结束后执行一段代码：

```python
for item in iterable:
    # 循环主体
else:
    # 循环结束后执行的代码
```

如果在循环过程中遇到了 break 语句，则会中断循环，此时不会执行 else 子句

空语句pass不做任何事情，一般用作占位语句，有点类似continue

