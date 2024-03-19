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

**#2.Kadane算法**

```python
# openjudge 02766
def kadane(list1):
    globalmax,localmax = list1[0],list1[0]
    for i in list1[1:]:
        localmax=max(localmax+i,i)
        globalmax=max(globalmax,localmax)
    return globalmax

n=int(input())
inp=[]
grid=[]
while True:
    for i in list(map(int,input().split())):
        inp.append(i)
    if len(inp)==n*n:
        for i in range(0,n*n,n):
            grid.append(inp[i:i+n])
        maxi=0
        for height in range(1,n+1):
            for i in range(0,n-height+1):
                temp=[0]*n
                for j in range(height):
                    for k in range(n):
                        temp[k]+=grid[i+j][k]
                maxi=max(kadane(temp),maxi)
        print(maxi)
        break
```

**#3.二叉树建树（已较为熟练，不详述）**

**#4.有些dfs爆栈可以考虑二分**（OJ04135）

```python
n,m = map(int, input().split())
list1=list(int(input()) for _ in range(n))

def check(x):
    total=0
    pos=0
    y=0
    while True:
        total+=list1[pos]
        pos+=1
        if pos==n:
            return (True if y<=m-1 else False)
        elif total+list1[pos]>x:
            total=0
            y+=1
    
lo=max(list1)
hi=sum(list1)+1
ans=0
while lo<hi:
    mid=(lo+hi)//2
    if check(mid):
        ans=mid
        hi=mid
    else :
        lo=mid+1

print(ans)

#注意lo=mid+1但是hi=mid
```

**#5.dic.items()返回键值对，dic只有键**

**#6.list直接赋值的时候是赋了一个引用，copy（）是浅拷贝**

如果想单纯地复制数值，使用

```python
from copy import deepcopy
list2=deepcopy(list1)
```

有时候反应不过来debug真的会想亖

```python
# OJ-20644:统计全为 1 的正方形子矩阵
from copy import deepcopy

hang,lie=map(int,input().split())
list1=[[0]*(lie+1)]
for _ in range(hang):
    s=input()
    list2=[0]
    for i in s:
        list2.append(int(i))
    list1.append(list2)
dp=deepcopy(list1)
hang1=hang-1
lie1=lie-1
while hang1>0 and lie1>0:
    for i in range(1,hang1+1):
        if list1[i][lie1]==1 and dp[i+1][lie1+1]>=1:
            x=0
            y=0
            while lie1+y<=lie and list1[i][lie1+y]==1:
                y+=1
            while i+x<=hang and list1[i+x][lie1]==1:
                x+=1
            dp[i][lie1]=min(x,min(y,1+dp[i+1][lie1+1]))
    for i in range(1,lie1+1):
        if list1[hang1][i]==1 and dp[hang1+1][i+1]>=1:
            x=0
            y=0
            while i+y<=lie and list1[hang1][i+y]==1:
                y+=1
            while x+hang1<=hang and list1[hang1+x][i]==1:
                x+=1
            dp[hang1][i]=min(x,min(y,1+dp[hang1+1][i+1]))
    hang1-=1
    lie1-=1
ans=0
for i in dp:
    ans+=sum(i)
print(ans)
```

**#7.关于zip**

当你有两个或更多个可迭代对象（例如列表、元组或其他序列）时，`zip` 函数可以将它们逐个元素地配对。它返回一个迭代器，该迭代器生成元组，每个元组包含来自每个可迭代对象的相应位置的元素。

下面是 `zip` 函数的基本用法示例：

```python
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']

zipped = zip(list1, list2)

for pair in zipped:
    print(pair)
```

这将输出：

```python
(1, 'a')
(2, 'b')
(3, 'c')
```

在代码中，`zip(list1, list2)` 将 `list1` 和 `list2` 中的元素逐一配对，生成一个由元组组成的迭代器 `zipped`。然后，我们可以通过迭代 `zipped` 来访问这些配对。每次迭代，我们得到一个包含来自 `list1` 和 `list2` 对应位置的元素的元组。

需要注意的是，如果可迭代对象的长度不同，`zip` 函数将会以最短的长度为准，多余的元素将被忽略。

**#8.bisect好用**

**nlog(n)复杂度求逆序数**

```python
import bisect
n=int(input())
lsit1=list(map(int,input().split()))
list1=[]
ans=0
for i in lsit1:
    pos=bisect.bisect_left(list1,i)
    ans+=pos
    list1.insert(pos,i)
print(ans)
```

补充一点关于insert、bisect.bisect_left、bisect.insort_left等等的参数传递规则：

```python
list.insort(index,item)
bisect.bisect_left(list,item)
bisect.insort_left(list,item)
```

请注意，如果写：

```python
from bisect import *
```

将会得到类似C++中类似

```C++
using namespace bisect
```

的效果，在写小代码量程序时非常方便

**#9.关于heapq**

**默认是最小堆，但实际上把值都取负就是最大堆，pop的时候再取一下负就行了**

```python
# OJ-06648:Sequence
import heapq

def g(list2,list1,n):
    heap=[(list1[0]+list2[i],0,i)for i in range(n)]
    ans=[]
    while n>0:
        ans1,pos1,pos2=heapq.heappop(heap)
        ans.append(ans1)
        if pos1+1<n:
            heapq.heappush(heap,(list1[pos1+1]+list2[pos2],pos1+1,pos2))
        n-=1
    return sorted(ans)

def f(list1,n,m):
    mini=list1[0]
    for i in range(1,m):
        mini=g(mini,list1[i],n)
    return mini

for _ in range(int(input())):
    m,n=map(int,input().split())
    list1=[sorted(list(map(int,input().split())))for _ in range(m)]
    result=f(list1,n,m)
    print(*result)
```

**#10.关于deque**

**把我虐惨的题目都要好好记下来**

```python
# OJ-26978:滑动窗口最大值
from collections import deque

n, k = map(int, input().split())
list1 = list(map(int, input().split()))
deque = deque()  # 用于存储索引
ans = []

for i in range(n):
    # 移除所有在当前滑动窗口之外的索引
    while deque and deque[0] < i - k + 1:
        deque.popleft()
    # 移除所有小于当前元素的值，因为它们不会成为最大值
    while deque and list1[i] >= list1[deque[-1]]:
        deque.pop()
    deque.append(i)
    # 当前窗口形成后，添加当前窗口的最大值到结果中
    if i >= k - 1:
        ans.append(list1[deque[0]])

print(*ans)
```

反思：

1.不要单纯把数值和序号挂钩进行暴力sort，可以把数值条件放到if中判断，只维护一个序号数组

2.耗时太久就GPT吧，别自己写了，真浪费时间败坏心情，难受😫

**#11.笨笨脑瓜永远想不出的前缀和，妙**

```python
# OJ-20453:和为k的子数组个数
def subarray_sum(nums, k):
    count = 0
    sums = 0
    d = dict()
    d[0] = 1

    for i in range(len(nums)):
        sums += nums[i]
        count += d.get(sums - k, 0)
        d[sums] = d.get(sums, 0) + 1

    return count

nums = list(map(int, input().split()))
k = int(input().strip())
print(subarray_sum(nums, k))
```

并且学到了get的用法，这是个defaultlist（0）的很好替代

**#12异或与XOR**

XOR：一真一假为True，对数值进行XOR运算时实际上是在2进制下做运算，符号为^，有意思的是a^a=0

```python
# OJ-20626:对子数列做XOR运算
list1=list(map(int,input().split()))
sum1=list1[0]
stack=[sum1]
for i in range(1,len(list1)):
    sum1^=list1[i]
    stack.append(sum1)
    
for _ in range(10000):
    a,b=map(int,input().split())
    if a==0:
        print(stack[b])
    else :
        print(stack[b]^stack[a-1])
# 对前缀和很好的应用
```

