#### $$Sort$$

```python
list.sort(key=None, reverse=False)
```

$$key 是一个比较函数，默认对对象中的每个元素进行字典序比较，可以替换为自定义函数。$$

$$reverse是排序规则，reverse = True 降序， reverse = False 升序（默认）$$

```python
#比如说对一个元素是list的list进行排序：

data = [[3, 4], [1, 2], [3, 1], [2, 3]]
data.sort(key=lambda x: (x[0], -x[1]))

#表示用来比较的元素是先第一个元素，后第二个元素的相反数，也就是第一个升序，第二个降序
```

$$当比较方法十分复杂的时候我们可以在sort外部定义一个比较函数$$

```python
def custom_sort_key(item):
    unit_weights = {'K': 1, 'M': 2, 'B': 3} 
    number_part, unit_part = item[:-1], item[-1]
    unit_weight = unit_weights.get(unit_part, 0)
    return (unit_weight, float(number_part))

data = ["1.23B", "5.67K", "10.5M", "3.21B"]
data.sort(key=custom_sort_key)
```



#### $$Kadane算法 \,—\, 查找最大子数组$$

#### $$Kadane\;Pro$$

```python
# OJ-20744:土豪购物
def kadane(list1):
	localmax=globalmax=list1[0]
	for i in list1[1:]:
		localmax=max(localmax+i,i)
		globalmax=max(globalmax,localmax)
	return globalmax

def tuhaokadane(list1):
	ans=kadane(list1)
	leftmax=[]
	rightmax=[]

	sum1=0
	for i in list1:
		sum1=max(sum1+i,i)
		leftmax.append(sum1)
	
	sum1=0
	for i in list1[::-1]:
		sum1=max(sum1+i,i)
		rightmax.append(sum1)

	rightmax.reverse()
	for i in range(1,len(list1)-1):
		ans=max(ans,(leftmax[i-1]+rightmax[i+1]))

	return ans

list1=list(map(int,input().split(',')))
if max(list1)<=0:
	print(max(list1))
else :
	print(tuhaokadane(list1))
```



#### $$DFS爆栈时考虑二分或不用递归$$

```python
# OJ-04135
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



#### $$list直接赋值的时候是赋了一个引用，copy\,(\;)\,是浅拷贝$$

#### $$一维数组可以copy，多维数组需要deepcopy$$

```python
from copy import deepcopy
list2=deepcopy(list1)
```



#### $$关于zip$$

##### $$当你有两个或更多个可迭代对象（例如列表、元组或其他序列）时,zip函数可以将它们逐个元素地配对。$$

##### $$它返回一个迭代器,该迭代器生成元组,每个元组包含来自每个可迭代对象的相应位置的元素。$$

```python
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']

zipped = zip(list1, list2)

for pair in zipped:
    print(pair)

# 输出：
(1, 'a')
(2, 'b')
(3, 'c')
```

##### $$在代码中，zip(list1, list2)将list1和list2中的元素逐一配对，生成一个由元组组成的迭代器zipped。$$

##### $$然后，我们可以通过迭代zipped来访问这些配对。每次迭代，我们得到一个包含来自list1和list2对应位置的元素的元组。$$

##### $$需要注意的是，如果可迭代对象的长度不同,zip函数将会以最短的长度为准，多余的元素将被忽略。$$



#### $$bisect好用$$

##### $$nlog(n)复杂度求逆序数$$

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

##### $$补充一点关于insert、bisect.bisect\_left、bisect.insert\_left等等的参数传递规则:$$

```python
list.insert(index,item)
bisect.bisect_left(list,item)
bisect.insort_left(list,item)
```

##### $$请注意，如果写:$$

```python
from bisect import *
```

##### $$将会得到类似C++中类似$$

```C++
using namespace bisect
```

##### $$的效果，在写小代码量程序时非常方便$$



#### $$关于heapq$$

##### $$默认是最小堆,但实际上把值都取负就是最大堆,pop的时候再取一下负就行了$$

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



#### $$关于deque$$

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

#我偏好的写法（虽然劣）：
n,k=map(int,input().split())
list1=list(map(int,input().split()))

import heapq
heap=[(-list1[0],0)]
ans=[]
pos=1
while pos<=n:
	val,pos1=heapq.heappop(heap)
	while pos-pos1>k and heap:
		val,pos1=heapq.heappop(heap)
	if pos>=k:
		ans.append(-val)
	heapq.heappush(heap,(val,pos1))
	if pos==n:
		break
	heapq.heappush(heap,(-list1[pos],pos))
	pos+=1

print(*ans)
```



#### $$前缀和$$

```python
# OJ-20453:和为k的子数组个数
list1=list(map(int,input().split()))
k=int(input())

pre=[list1[0]]
for i in range(1,len(list1)):
	pre.append(pre[-1]+list1[i])

ans=pre.count(k)
for i in range(len(list1)):
	ans+=pre[:i].count(pre[i]-k)

print(ans)
```



#### $$dict.get(value,type)是defaultlist的很好替代$$



#### $$异或与(XOR)$$

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



#### $$DP替代线段树$$

```python
# OJ-23568:幸福的寒假生活
def change(a):
	mon,day=map(int,a.split('.'))
	return 31*(mon-1)+day-6

n=int(input())
list1=[]
dp=[0 for _ in range(46)]
for _ in range(n):
	a,b,c=input().split()
	a,b,c=change(a),change(b),int(c)
	if b>45:
		continue
	list1.append((a,b,c))
n=len(list1)
for i in range(1,46):
	dp[i]=dp[i-1]
	for j in list1:
		st,en,va=j[0],j[1],j[2]
		if en==i:
			dp[i]=max(dp[i],dp[st-1]+va)
print(dp[45])
```



#### $$ AVL$$

```python
class Node:
	def __init__(self,data):
		self.data=data
		self.left=None
		self.right=None

def getheight(root):
	if not root:
		return 0
	return 1+max(getheight(root.left),getheight(root.right))

def getbalance(root):
	return getheight(root.left)-getheight(root.right)

def LLrotate(root):
	a=root.left
	if a.right==None:
		root.left=None
	else:
		root.left=a.right
	a.right=root
	return a
	
def RRrotate(root):
	a=root.right
	if a.left==None:
		root.right=None
	else:
		root.right=a.left
	a.left=root
	return a

def pre(root):
	if not root:
		return []
	return [root.data]+pre(root.left)+pre(root.right)

class AVL:
	def __init__(self):
		self.root=None

	def insert(self,data):
		if not self.root:
			self.root=Node(data)
		else:
			self.root=self._insert(self.root,data)

	def _insert(self,root,data):
		if not root:
			return Node(data)
		elif data<root.data:
			root.left=self._insert(root.left,data)
		else:
			root.right=self._insert(root.right,data)
		
		balance=getbalance(root)

		if balance>1:
			if data<root.left.data:#LL
				root=LLrotate(root)
			else:#LR
				root.left=RRrotate(root.left)
				root=LLrotate(root)
		
		if balance<-1:
			if data>root.right.data:#RR
				root=RRrotate(root)
			else:#RL
				root.right=LLrotate(root.right)
				root=RRrotate(root)

		return root	

n=int(input())
list1=list(map(int,input().split()))
Tree=AVL()
for i in list1:
	Tree.insert(i)
print(*pre(Tree.root))
```



#### $$骑士周游$$

##### $$记得启发式搜索就行$$



#### $$二进制$$

```python
二进制：bin(x)->0byyy
数值之间转换：int(bin(x)[2:])
```



#### $$KMP$$

```python
# 前缀数组
def findpre(s):
	pre=[0 for _ in range(len(s))]
	j=0
	for i in range(1,len(s)):
		while j>0 and s[i]!=s[j]:
			j=pre[j-1]
		if s[i]==s[j]:
			j+=1
		pre[i]=j
	return pre
	
# next数组
next=[-1]+pre[:-1]
```

##### $$值得注意的是:$$

```python
pos%(pos - pre[pos-1]) == 0 and pre[pos-1]!=0
```

##### $$是判定字符串前缀是否含有周期的条件$$



#### $$表达式树——shunting\,-\,yard$$





#### 中序表达式转后序表达式

```python
pre={'+':1,'-':1,'*':2,'/':2}
for _ in range(int(input())):
    expr=input()
    ans=[]; ops=[]
    for char in expr:
        if char.isdigit() or char=='.':
            ans.append(char)
        elif char=='(':
            ops.append(char)
        elif char==')':
            while ops and ops[-1]!='(':
                ans.append(ops.pop())
            ops.pop()
        else:
            while ops and ops[-1]!='(' and pre[ops[-1]]>=pre[char]:
                ans.append(ops.pop())
            ops.append(char)
    while ops:
        ans.append(ops.pop())
    print(''.join(ans))
```

#### 最大全0子矩阵

```python
for row in ma:
    stack=[]
    for i in range(n):
        h[i]=h[i]+1 if row[i]==0 else 0
        while stack and h[stack[-1]]>h[i]:
            y=h[stack.pop()]
            w=i if not stack else i-stack[-1]-1
            ans=max(ans,y*w)
        stack.append(i)
    while stack:
        y=h[stack.pop()]
        w=n if not stack else n-stack[-1]-1
        ans=max(ans,y*w)
print(ans)
```

#### 求逆序对数

```python
from bisect import *
a=[]
rev=0
for _ in range(n):
    num=int(input())
    rev+=bisect_left(a,num)
    insort_left(a,num)
ans=n*(n-1)//2-rev
```

```python
def merge_sort(a):
    if len(a)<=1:
        return a,0
    mid=len(a)//2
    l,l_cnt=merge_sort(a[:mid])
    r,r_cnt=merge_sort(a[mid:])
    merged,merge_cnt=merge(l,r)
    return merged,l_cnt+r_cnt+merge_cnt
def merge(l,r):
    merged=[]
    l_idx,r_idx=0,0
    inverse_cnt=0
    while l_idx<len(l) and r_idx<len(r):
        if l[l_idx]<=r[r_idx]:
            merged.append(l[l_idx])
            l_idx+=1
        else:
            merged.append(r[r_idx])
            r_idx+=1
            inverse_cnt+=len(l)-l_idx
    merged.extend(l[l_idx:])
    merged.extend(r[r_idx:])
    return merged,inverse_cnt
```



#### 解析括号嵌套表达式

```python
def parse(s):
    node=Node(s[0])
    if len(s)==1:
        return node
    s=s[2:-1]; t=0; last=-1
    for i in range(len(s)):
        if s[i]=='(': t+=1
        elif s[i]==')': t-=1
        elif s[i]==',' and t==0:
            node.children.append(parse(s[last+1:i]))
            last=i
    node.children.append(parse(s[last+1:]))
    return node
```

#### 并查集

```python
class UnionFind:
    def __init__(self,n):
        self.p=list(range(n))
        self.h=[0]*n
    def find(self,x):
        if self.p[x]!=x:
            self.p[x]=self.find(self.p[x])
        return self.p[x]
    def union(self,x,y):
        rootx=self.find(x)
        rooty=self.find(y)
        if rootx!=rooty:
            if self.h[rootx]<self.h[rooty]:
                self.p[rootx]=rooty
            elif self.h[rootx]>self.h[rooty]:
                self.p[rooty]=rootx
            else:
                self.p[rooty]=rootx
                self.h[rootx]+=1
```

#### 

Trie字典树







#### dijkstra

```python
# 1.使用vis集合
def dijkstra(start,end):
    heap=[(0,start,[start])]
    vis=set()
    while heap:
        (cost,u,path)=heappop(heap)
        if u in vis: continue
        vis.add(u)
        if u==end: return (cost,path)
        for v in graph[u]:
            if v not in vis:
                heappush(heap,(cost+graph[u][v],v,path+[v]))
# 2.使用dist数组
import heapq
def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    priority_queue = [(0, start)]
    while priority_queue:
        current_distance, current_node = heapq.heappop(priority_queue)
        if current_distance > distances[current_node]:
            continue
        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))
    return distances
```

#### kruskal

```python
uf=UnionFind(n)
edges.sort()
ans=0
for w,u,v in edges:
    if uf.union(u,v):
        ans+=w
print(ans)
```

#### prim

```python
vis=[0]*n
q=[(0,0)]
ans=0
while q:
    w,u=heappop(q)
    if vis[u]:
        continue
    ans+=w
    vis[u]=1
    for v in range(n):
        if not vis[v] and graph[u][v]!=-1:
            heappush(q,(graph[u][v],v))
print(ans)
```

#### 拓扑排序

```python
from collections import deque
def topo_sort(graph):
    in_degree={u:0 for u in graph}
    for u in graph:
        for v in graph[u]:
            in_degree[v]+=1
    q=deque([u for u in in_degree if in_degree[u]==0])
    topo_order=[]
    while q:
        u=q.popleft()
        topo_order.append(u)
        for v in graph[u]:
            in_degree[v]-=1
            if in_degree[v]==0:
                q.append(v)
    if len(topo_order)!=len(graph):
        return []  
    return topo_order
```



### 工具

int(str,n)	将字符串`str`转换为`n`进制的整数。

for key,value in dict.items()	遍历字典的键值对。

for index,value in enumerate(list)	枚举列表，提供元素及其索引。

dict.get(key,default) 	从字典中获取键对应的值，如果键不存在，则返回默认值`default`。

list(zip(a,b))	将两个列表元素一一配对，生成元组的列表。

math.pow(m,n)	计算`m`的`n`次幂。

math.log(m,n)	计算以`n`为底的`m`的对数。

lrucache	

```py
from functools import lru_cache
@lru_cache(maxsize=None)
```

bisect

```python
import bisect
# 创建一个有序列表
sorted_list = [1, 3, 4, 4, 5, 7]
# 使用bisect_left查找插入点
position = bisect.bisect_left(sorted_list, 4)
print(position)  # 输出: 2
# 使用bisect_right查找插入点
position = bisect.bisect_right(sorted_list, 4)
print(position)  # 输出: 4
# 使用insort_left插入元素
bisect.insort_left(sorted_list, 4)
print(sorted_list)  # 输出: [1, 3, 4, 4, 4, 5, 7]
# 使用insort_right插入元素
bisect.insort_right(sorted_list, 4)
print(sorted_list)  # 输出: [1, 3, 4, 4, 4, 4, 5, 7]
```

字符串

1. `str.lstrip() / str.rstrip()`: 移除字符串左侧/右侧的空白字符。

2. `str.find(sub)`: 返回子字符串`sub`在字符串中首次出现的索引，如果未找到，则返回-1。

3. `str.replace(old, new)`: 将字符串中的`old`子字符串替换为`new`。

4. `str.startswith(prefix) / str.endswith(suffix)`: 检查字符串是否以`prefix`开头或以`suffix`结尾。

5. `str.isalpha() / str.isdigit() / str.isalnum()`: 检查字符串是否全部由字母/数字/字母和数字组成。

   6.`str.title()`：每个单词首字母大写。

counter：计数

```python
from collections import Counter
# 创建一个Counter对象
count = Counter(['apple', 'banana', 'apple', 'orange', 'banana', 'apple'])
# 输出Counter对象
print(count)  # 输出: Counter({'apple': 3, 'banana': 2, 'orange': 1})
# 访问单个元素的计数
print(count['apple'])  # 输出: 3
# 访问不存在的元素返回0
print(count['grape'])  # 输出: 0
# 添加元素
count.update(['grape', 'apple'])
print(count)  # 输出: Counter({'apple': 4, 'banana': 2, 'orange': 1, 'grape': 1})
```

permutations：全排列

```python
from itertools import permutations
# 创建一个可迭代对象的排列
perm = permutations([1, 2, 3])
# 打印所有排列
for p in perm:
    print(p)
# 输出: (1, 2, 3)，(1, 3, 2)，(2, 1, 3)，(2, 3, 1)，(3, 1, 2)，(3, 2, 1)
```

combinations：组合

```python
from itertools import combinations
# 创建一个可迭代对象的组合
comb = combinations([1, 2, 3], 2)
# 打印所有组合
for c in comb:
    print(c)
# 输出: (1, 2)，(1, 3)，(2, 3)
```

reduce：累次运算

```python
from functools import reduce
# 使用reduce计算列表元素的乘积
product = reduce(lambda x, y: x * y, [1, 2, 3, 4])
print(product)  # 输出: 24
```

product：笛卡尔积

```python
from itertools import product
# 创建两个可迭代对象的笛卡尔积
prod = product([1, 2], ['a', 'b'])
# 打印所有笛卡尔积对
for p in prod:
    print(p)
# 输出: (1, 'a')，(1, 'b')，(2, 'a')，(2, 'b')
```





## 字符串

### 1. 大小写转换

```python
text: str
text.upper() # 变全大写
text.lower() # 变全小写
text.capitalize() # 首字母大写
text.title() # 单个字母大写
text.swapcase() # 大小写转换
s[idx].isdigit() # 判断是否为整
s.isnumeric() # 判断是否为数字（包含汉字、阿拉伯数字等）更广泛
```

补充：需要十分注意的一点事，当我们将str转化为list时（如‘sfda’：转化为的是['s', 'f', 'd', 'a']，而不是[‘sfda’]）

### 2. 索引技巧

#### 2.1 列表:

`list.index()`

```python
# 返回第一个匹配元素的索引，如果找不到该元素则会引发 ValueError 异常

list.index(element, start, end)

my_list = [10, 20, 30, 40, 50, 30]
index = my_list.index(30)
print(index)  # 输出：2

index = my_list.index(30, 3)
print(index)  # 输出：5
```



#### 2.2 字符串：

`str.find()` 和 `str.index()`

```python
my_string = "Hello, world!"
index = my_string.find("world")
print(index)  # 输出：7

index = my_string.find("Python")
print(index)  # 输出：-1

my_string = "Hello, world!"
index = my_string.index("world")
print(index)  # 输出：7

index = my_string.index("Python")  # 引发 ValueError
```

#### 2.3 字典:

```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
exists = 'b' in my_dict
print(exists)  # 输出：True

exists = 'd' in my_dict
print(exists)  # 输出：False

keys_list = list(my_dict.keys())
index = keys_list.index('b')
print(index)  # 输出：1

# 直接查找字典中的键
index = list(my_dict).index('b')
print(index)  # 输出：1

dict.get(key, default=None)
# 返回指定键的值，如果值不在字典中返回default值
dict.setdefault(key, default=None)
# 和get()类似, 但如果键不存在于字典中，将会添加键并将值设为default
```

#### 2.4 集合

```python
set1 = {1, 2, 3}
set2 = {3, 4, 5}

# 并集
union_set = set1 | set2
print("并集:", union_set)  # 输出：{1, 2, 3, 4, 5}

# 交集
intersection_set = set1 & set2
print("交集:", intersection_set)  # 输出：{3}

# 差集
difference_set = set1 - set2
print("差集:", difference_set)  # 输出：{1, 2}

# 对称差集
symmetric_difference_set = set1 ^ set2
print("对称差集:", symmetric_difference_set)  # 输出：{1, 2, 4, 5}
```

## import相关

```python
# pylint: skip-file
import heapq
from collections import defaultdict
from collections import dequeue
import bisect
from functools import lru_cache
@lru_cache(maxsize=None)
import sys
sys.setrecursionlimit(1<<32)
import math
math.ceil()  # 函数进行向上取整
math.floor() # 函数进行向下取整。
math.isqrt() # 开方取整
exit()
```



#### bisect

1. $$bisect.bisect_left(a, x, lo=0, hi=len(a))$$
   - 在列表`a`中查找元素`x`的插入点，使得插入后仍保持排序。
   - 返回插入点的索引，插入点位于`a`中所有等于`x`的元素之前。
2. $$bisect.bisect_right(a, x, lo=0, hi=len(a))$$ 或 $$bisect.bisect(a, x, lo=0, hi=len(a))$$
   - 类似于`bisect_left`，但插入点位于`a`中所有等于`x`的元素之后。
3. $$bisect.insort_left(a, x, lo=0, hi=len(a))$$
   - 在`a`中查找`x`的插入点并插入`x`，保持列表`a`的有序。
   - 插入点位于`a`中所有等于`x`的元素之前。
4. $$bisect.insort_right(a, x, lo=0, hi=len(a))$$ 或 $$bisect.insort(a, x, lo=0, hi=len(a))$$
   - 类似于`insort_left`，但插入点位于`a`中所有等于`x`的元素之后。

###### 示例代码

```
python复制代码import bisect

a = [1, 2, 4, 4, 5]

# 查找插入点
print(bisect.bisect_left(a, 4))  # 输出: 2
print(bisect.bisect_right(a, 4)) # 输出: 4

# 插入元素
bisect.insort_left(a, 3)
print(a)  # 输出: [1, 2, 3, 4, 4, 5]

bisect.insort_right(a, 4)
print(a)  # 输出: [1, 2, 3, 4, 4, 4, 5]
```



## 转换

#### 进制

```python
b = bin(item)  # 2进制 0b1111
o = oct(item)  # 8进制 0o1111
h = hex(item)  # 16进制 0xffff
```

#### ASCII

```python
ord(char) -> ASCII_value
chr(ascii_value) -> char
```

#### print保留小数

```python
print("%.6f" % x)
print("{:.6f}".format(result))
# 当输出内容很多时：
print('\n'.join(map(str, ans)))
```

## 算法

### 1.埃氏筛

```python
n = int(input())
prime = [0]*2 + [1]*(n-1)
for i in range(n+1):
    if prime[i]:
        for j in range(i*i, n+1, i):
            prime[j] = 0
```

### 2.强联通子图

Kosaraju's算法可以分为以下几个步骤：

1. $$第一次DFS$$：对图进行一次DFS，并记录每个顶点的完成时间（即DFS从该顶点返回的时间）。
2. $$转置图$$：将图中所有边的方向反转，得到转置图。
3. $$第二次DFS$$：根据第一次DFS记录的完成时间的逆序，对转置图进行DFS。每次DFS遍历到的所有顶点构成一个强连通分量。

### 详细步骤

1. $$第一次DFS$$：
   - 初始化一个栈用于记录DFS完成时间顺序。
   - 对图中的每个顶点执行DFS，如果顶点尚未被访问过，则从该顶点开始DFS。
   - DFS过程中，当一个顶点的所有邻居都被访问过后，将该顶点压入栈中。
2. $$转置图$$：
   - 创建一个新的图，边的方向与原图相反。
3. $$第二次DFS$$：
   - 初始化一个新的访问标记数组。
   - 根据栈中的顺序（即第一步中记录的完成时间的逆序）对转置图进行DFS。
   - 每次从栈中弹出一个顶点，如果该顶点尚未被访问过，则从该顶点开始DFS，每次DFS遍历到的所有顶点构成一个强连通分量。

### 示例代码

以下是Kosaraju's算法的Python实现：

```python
from collections import defaultdict

class Graph:
    def __init__(self, vertices):
        self.graph = defaultdict(list)
        self.V = vertices

    def addEdge(self, u, v):
        self.graph[u].append(v)

    def _dfs(self, v, visited, stack):
        visited[v] = True
        for neighbour in self.graph[v]:
            if not visited[neighbour]:
                self._dfs(neighbour, visited, stack)
        stack.append(v)

    def _transpose(self):
        g = Graph(self.V)
        for i in self.graph:
            for j in self.graph[i]:
                g.addEdge(j, i)
        return g

    def _fillOrder(self, v, visited, stack):
        visited[v] = True
        for neighbour in self.graph[v]:
            if not visited[neighbour]:
                self._fillOrder(neighbour, visited, stack)
        stack.append(v)

    def _dfsUtil(self, v, visited):
        visited[v] = True
        print(v, end=' ')
        for neighbour in self.graph[v]:
            if not visited[neighbour]:
                self._dfsUtil(neighbour, visited)

    def printSCCs(self):
        stack = []
        visited = [False] * self.V

        for i in range(self.V):
            if not visited[i]:
                self._fillOrder(i, visited, stack)

        gr = self._transpose()

        visited = [False] * self.V

        while stack:
            i = stack.pop()
            if not visited[i]:
                gr._dfsUtil(i, visited)
                print("")

# 示例使用
g = Graph(5)
g.addEdge(1, 0)
g.addEdge(0, 2)
g.addEdge(2, 1)
g.addEdge(0, 3)
g.addEdge(3, 4)

print("Strongly Connected Components:")
g.printSCCs()
```

### 代码说明

1. $$Graph类$$：
   - 初始化图的邻接表表示。
   - `addEdge`方法用于添加图的边。
2. $$\_dfs和_fillOrder$$：
   - `_dfs`方法用于深度优先搜索，并在顶点完成时将其添加到栈中。
   - `_fillOrder`方法用于填充栈，记录顶点的完成顺序。
3. $$_transpose$$：
   - `_transpose`方法用于生成转置图。
4. $$_dfsUtil$$：
   - `_dfsUtil`方法用于在转置图上进行DFS。
5. $$printSCCs$$：
   - `printSCCs`方法结合上述方法实现Kosaraju's算法，用于打印强连通分量。

在这个实现中，我们首先对原图进行一次DFS，并记录每个顶点的完成时间顺序。然后我们构造转置图，并根据完成时间顺序的逆序对转置图进行DFS，找到所有强连通分量。

### 二分查找

```python
# hi:不可行最小值， lo:可行最大值
lo, hi, ans = 0, max(lst), 0
while lo + 1 < hi:
    mid = (lo + hi) // 2
    # print(lo, hi, mid)
    if check(mid): # 返回True，是因为num>m，是确定不合适
        ans = mid
        lo = mid # 所以lo可以置为 mid + 1。
    else:
        hi = mid
#print(lo)
print(ans)
```

# 数据结构

### 单调栈

* 栈中元素单调。若题目时间复杂度降不下来，可以考虑。（==Tough预定==知识点）

### 并查集

```python
P = list(range(N))
def p(x):
    if P[x] == x:
        return x
    else:
        P[x] = p(P[x])
        return P[x]
    
def union(x, y):
    px, py = p(x), p(y)
    if px==py:
        return True
    else:
        if <不合法>:  # 根据题意，有时可略
            return False
        else:
            P[px] = py
            return True
```

### 8.Trie

1. $$插入（Insert）$$：

   ```python
   class TrieNode:
       def __init__(self):
           self.children = {}
           self.is_end_of_word = False
   
   class Trie:
       def __init__(self):
           self.root = TrieNode()
   
       def insert(self, word):
           node = self.root
           for char in word:
               if char not in node.children:
                   node.children[char] = TrieNode()
               node = node.children[char]
           node.is_end_of_word = True
   ```

2. $$查找（Search）$$：

   ```python
   def search(self, word):
       node = self.root
       for char in word:
           if char not in node.children:
               return False
           node = node.children[char]
       return node.is_end_of_word
   ```

3. $$前缀查询（StartsWith）$$：

   ```python
   def starts_with(self, prefix):
       node = self.root
       for char in prefix:
           if char not in node.children:
               return False
           node = node.children[char]
       return True
   ```

