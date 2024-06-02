---
typora-copy-images-to: ./
---

# Homework #9: 图论：遍历，及 树算

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 04081: 树的转换

http://cs101.openjudge.cn/dsapre/04081/

思路：

层序历遍，如果有子节点就递归建树，直到全部建完，最后求高度

代码

```python
from collections import defaultdict

class Node:
    def __init__(self,data):
        self.data=data
        self.children=[]

class Tree:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None
               
s=input()
dic=defaultdict(list)
dic[0].append(Node(0))
sumd=0
pos=1
for i in s:
    if i=='d':
        sumd+=1
        a=Node(pos)
        dic[sumd-1][-1].children.append(a)
        dic[sumd].append(a)
        pos+=1
    else:
        sumd-=1    
oldmax=max(dic.keys())

def buildtree(a):
    root=Tree(a.data)
    b=a.children[0]
    if b.children:
        root.left=buildtree(b)
    else :
        root.left=Tree(b.data)
    stack=[root,root.left]
    for i in a.children[1:]:
        if i.children:
            stack.append(buildtree(i))
        else :
            stack.append(Tree(i.data))
        stack[-2].right=stack[-1]
    return root

root=buildtree(dic[0][0])

def findmax(root):
    return max(0 if root.left==None else findmax(root.left),(0 if root.right==None else findmax(root.right)))+1

newmax=findmax(root)-1

print(f"{oldmax} => {newmax}")
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240414182508520.png)



### 08581: 扩展二叉树

http://cs101.openjudge.cn/dsapre/08581/

思路：

递归建树，节点满载了就pop出去，最后递归得到答案

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

def mid(root):
    return ("" if root.left==None else mid(root.left))+(""if root.data=='.'else root.data)+(""if root.right==None else mid(root.right))

def post(root):
    return ("" if root.left==None else post(root.left))+(""if root.right==None else post(root.right))+(""if root.data=='.'else root.data)

s=input()
root=Node(s[0])
stack=[root]
for i in range(1,len(s)):
    a=Node(s[i])
    if stack[-1].left==None :
        stack[-1].left=a
        if a.data!='.':
            stack.append(a)
    else :
        stack[-1].right=a
        stack.pop()
        if s[i]!='.':
            stack.append(a)

print(mid(root))
print(post(root))

#ABD..EF..G..C..
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240414182605658.png)



### 22067: 快速堆猪

http://cs101.openjudge.cn/practice/22067/

思路：

维护一个min数组，对应pop和append就行，类似dp

代码

```python
list1=[]
dp=[]
while True:
    try:
        s=input().strip()
        if len(s)>3:
            s,n=s.split()
        if list1 and s=='pop':
            list1.pop()
            dp.pop()
        elif s=='push':
            list1.append(int(n))
            if dp:
                dp.append(min(dp[-1],int(n)))
            else :
                dp.append(int(n))
        elif s=='min' and list1:
            print (dp[-1])
    except EOFError:
        break
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240414182755757.png)



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：

bfs+回溯，最终全部走满就+1

代码

```python
for _ in range(int(input())):
	n,m,x,y=map(int,input().split())
	dxy=[(-2,-1),(-2,1),(-1,2),(-1,-2),(1,2),(1,-2),(2,1),(2,-1)]
	grid=[[False for _ in range(m)]for _ in range(n)]
	grid[x][y]=True
	ans=0
	def bfs(x,y,sum1):
		if sum1==m*n:
			global ans
			ans+=1
			return
		ok=True
		for i in range(8):
			dx,dy=dxy[i][0],dxy[i][1]
			x1=x+dx
			y1=y+dy
			if 0<=x1 and x1<n and 0<=y1 and y1<m and not grid[x1][y1]:
				grid[x1][y1]=True
				bfs(x1,y1,sum1+1)
				grid[x1][y1]=False
	bfs(x,y,1)
	print(ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240414185650874.png)



### 28046: 词梯

bfs, http://cs101.openjudge.cn/practice/28046/

思路：

从a开始向外查找，不重复使用，同时用heapq，找到目标就exit

代码

```python
def check(a,b):
	sum1=0
	for i in range(4):
		if a[i]!=b[i]:
			sum1+=1
	return sum1==1

n=int(input())
list1=sorted(list(set((input() for _ in range(n)))))
used=[False for _ in range(n)]
a,b=input().split()
used[list1.index(a)]=True

import heapq
heap=[(1,[a])]
while heap:
	length,list2=heapq.heappop(heap)
	for i in range(n):
		if not used[i] and check(list1[i],list2[-1]):
			if list1[i]==b:
				list2+=[b]
				print(*list2)
				exit()
			used[i]=True
			heapq.heappush(heap,(length+1,list2+[list1[i]]))
print('NO')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240414185738925.png)



### 28050: 骑士周游

dfs, http://cs101.openjudge.cn/practice/28050/

思路：

建图，然后从起始点开始启发式搜索，对于fail的情况非常慢，不知道怎么优化

代码

```python
class vertex:
	def __init__(self,x,y):
		self.x=x
		self.y=y
		self.children=[]

	def addchildren(self,vertex1):
		self.children.append(vertex1)

class graph:
	def __init__(self,n):
		self.vertexlist={}
		for i in range(n):
			for j in range(n):
				self.vertexlist[(i,j)]=vertex(i,j)
	
	def addedge(self,vertex1,vertex2):
		self.vertexlist[vertex1].addchildren(vertex2)
	
	def buildgraph(self,n):
		dxy=[(-2,-1),(-2,1),(-1,2),(-1,-2),(1,2),(1,-2),(2,1),(2,-1)]
		for key in self.vertexlist:
			for i in range(8):
				x1,y1=key[0]+dxy[i][0],key[1]+dxy[i][1]
				if x1>=0 and y1>=0 and x1<n and y1<n:
					self.addedge(key,(x1,y1))

n=int(input())
x,y=map(int,input().split())
tu=graph(n)
tu.buildgraph(n)
used={}
for i in range(n):
	for j in range(n):
		used[(i,j)]=False
used[(x,y)]=True

def nbsort(tu,list1):
	list2=[]
	for i in list1:
		sum1=0
		for j in tu.vertexlist[i].children:
			if not used[j]:
				sum1+=1
		list2.append((sum1,i))
	list2.sort(key=lambda x:x[0])
	list3=[x[1] for x in list2]
	return list3
	
def dfs(tu,vertex1,depth):
	if depth==n*n:
		print('success')
		exit()
	list1=tu.vertexlist[vertex1].children
	list1=nbsort(tu,list1)
	for i in list1:
		if not used[i]:
			used[i]=True
			dfs(tu,i,depth+1)
			used[i]=False

dfs(tu,(x,y),1)
print('fail')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240602222329087.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

还没有熟练图的写法，所以本周作业写得有点曲折，骑士周游从零开始学图，非常拷打（😔）。

期中季即将结束，要恶补一下了（😓）。



