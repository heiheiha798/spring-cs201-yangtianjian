**#1.python中sort的使用：**

```python
list.sort(key=None, reverse=False)
```

key — 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。

reverse — 排序规则，**reverse = True** 降序， **reverse = False** 升序（默认）

```python
#比如说对一个元素是list的list进行排序：

data = [[3, 4], [1, 2], [3, 1], [2, 3]]

data.sort(key=lambda x: (x[0], -x[1]))

#表示用来比较的元素是先第一个元素，后第二个元素的相反数，也就是第一个升序，第二个降序
```

当比较方法十分复杂的时候我们可以在sort外部定义一个比较函数

```python
def custom_sort_key(item):
    unit_weights = {'K': 1, 'M': 2, 'B': 3} 
    number_part, unit_part = item[:-1], item[-1]
    unit_weight = unit_weights.get(unit_part, 0)
    return (unit_weight, float(number_part))

data = ["1.23B", "5.67K", "10.5M", "3.21B"]
data.sort(key=custom_sort_key)
```

