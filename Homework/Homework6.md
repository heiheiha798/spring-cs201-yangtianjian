# Homework #6: "树"算：Huffman,BinHeap,BST,AVL,DisjointSet

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 22275: 二叉搜索树的遍历

http://cs101.openjudge.cn/practice/22275/

思路：

左小右大建树，递归后序

代码

```python
class Node:
	def __init__(self,data):
		self.data=data
		self.left=None
		self.right=None

def buildtree(root,a):
	if a<root.data:
		if root.left==None:
			root.left=Node(a)
		else:
			root.left=buildtree(root.left,a)
	else:
		if root.right==None:
			root.right=Node(a)
		else :
			root.right=buildtree(root.right,a)
	return root

def post(root):
	return (" "if root.left==None else post(root.left))+" "+(" "if root.right==None else post(root.right))+" "+str(root.data)

n=int(input())
list1=list(map(int,input().split()))
root=Node(list1[0])
for i in list1[1:]:
	buildtree(root,i)
print(*post(root).split())
```

代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-03-24 225946](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/d66ffa1bd12e58710ba30501a4aeddad7233757d/img/Screenshot%202024-03-24%20225946.jpg)



### 05455: 二叉搜索树的层次遍历

http://cs101.openjudge.cn/practice/05455/

思路：

左小右大建树，用dict按层存二叉树的数据，最后输出

代码

```python
from collections import defaultdict
dic=defaultdict(list)

class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

def buildtree(root,a):
    if a<root.data:
        root.left=(Node(a) if root.left==None else buildtree(root.left,a))
    if a>root.data :
        root.right=(Node(a) if root.right==None else buildtree(root.right,a))
    return root

def zhouyou(root,depth):
    dic[depth].append(root.data)
    if root.left!=None:
        zhouyou(root.left,depth+1)
    if root.right != None:
        zhouyou(root.right,depth+1)

list1=list(map(int,input().split()))
root = Node(list1[0])
for i in range(1,len(list1)):
    root=buildtree(root,list1[i])
zhouyou(root,0)
ans=[]
for key,value in dic.items():
    ans+=value
print(*ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![Screenshot 2024-03-24 230044](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/d66ffa1bd12e58710ba30501a4aeddad7233757d/img/Screenshot%202024-03-24%20230044.jpg)



### 04078: 实现堆结构

http://cs101.openjudge.cn/practice/04078/

练习自己写个BinHeap。当然机考时候，如果遇到这样题目，直接import heapq。手搓栈、队列、堆、AVL等，考试前需要搓个遍。

思路：

运用heapq即可，最大堆把数据取反就行

代码

```python
import heapq
heap=[]
for _ in range(int(input())):
	type=list(map(int,input().split()))
	if type[0]==1:
		heapq.heappush(heap,type[1])
	else:
		print(heapq.heappop(heap))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/c2f2773ad02f79612f5357bc1d3aa541d6efb23b/img/Screenshot%202024-03-24%20230126.jpg)



### 22161: 哈夫曼编码树

http://cs101.openjudge.cn/practice/22161/

思路：

根据题目的描述进行模拟，先对输入数据进行处理，排序，然后建树，对查询输入，字符串直接到字典中找编码，是编码的话比较麻烦，需要递归处理

代码

```python
class Node:
	def __init__(self,data,name):
		self.data=data
		self.left=None
		self.right=None
		self.name=name

from collections import defaultdict
n=int(input())
list1=[]
dict1=defaultdict(str)
for _ in range(n):
	s,val=input().split()
	list1.append(Node(int(val),s))
list1.sort(key=lambda x:(x.data,x.name),reverse=True)

while len(list1)>1:
	left=list1.pop()
	right=list1.pop()
	for i in left.name:
		dict1[i]='0'+dict1[i]
	for i in right.name:
		dict1[i]='1'+dict1[i]
	root=Node(left.data+right.data,left.name+right.name)
	root.left=left
	root.right=right
	list1.append(root)
	list1.sort(key=lambda x:(x.data,x.name),reverse=True)
root=list1[0]

def jiemi (S,root,nowroot):
	if S[0]=='0':
		if len(nowroot.left.name)==1:
			return nowroot.left.name+("" if len(S)==1 else jiemi(S[1:],root,root))
		else:
			return jiemi(S[1:],root,nowroot.left)
	else:
		if len(nowroot.right.name)==1:
			return nowroot.right.name+("" if len(S)==1 else jiemi(S[1:],root,root))
		else:
			return jiemi(S[1:],root,nowroot.right)

while True:
	try:
		s=input()
		ans=''
		if s.isdigit():
			ans=jiemi(s,root,root)
		else :
			for i in s:
				ans+=dict1[i]
		print(ans)
	except EOFError:
		break
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/c2f2773ad02f79612f5357bc1d3aa541d6efb23b/img/Screenshot%202024-03-24%20230751.jpg)



### 晴问9.5: 平衡二叉树的建立

https://sunnywhy.com/sfbj/9/5/359

思路：

学习样例代码，首先建搜索树，递归从下往上检查balance，进行相应旋转，最后前序输出

代码

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

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![Screenshot 2024-03-25 001718](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/c2f2773ad02f79612f5357bc1d3aa541d6efb23b/img/Screenshot%202024-03-25%20091509.jpg)



### 02524: 宗教信仰

http://cs101.openjudge.cn/practice/02524/

思路：

用father数组存各个编号的父子关系，最后看有多少个独立祖先，其实也可以再用一个数组存辈分

代码

```python
pos=0
while True:
    pos+=1
    n,m=map(int,input().split())
    if n==0:
        break
    father=[x for x in range(n+1)]
    for _ in range(m):
        a,b=map(int,input().split())
        while father[a]!=a:
            a=father[a]
        while father[b]!=b:
            b=father[b]
        father[b]=a
    list1=[False]*(n+1)
    sum=0
    for i in range(1,n+1):
        s=i
        while father[s]!=s:
            if list1[s]:
                break
            list1[s]=True
            s=father[s]
        if father[s]==s and list1[s]==False:
            sum+=1
            list1[s]=True
    print(f"Case {pos}: {sum}")
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/c2f2773ad02f79612f5357bc1d3aa541d6efb23b/img/Screenshot%202024-03-24%20230751.jpg)



## 2. 学习总结和收获

这周做了一下数算pre每日选做，看到英文头痛，中文题写得七七八八，很有收获，基本数据结构和常用函数都有一定熟练度。还会遇到dp想不出转移方程、算法太low导致超时等等问题，还需要多学习样例代码。

AVL完全不会，只能理解性抄写/(ㄒoㄒ)/~~
