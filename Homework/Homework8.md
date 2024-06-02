# Assignment #8: 图论：概念、遍历，及 树算

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 19943: 图的拉普拉斯矩阵

matrices, http://cs101.openjudge.cn/practice/19943/

思路：

一个二维list进行操作

代码

```python
n,m=map(int,input().split())
graph=[[0 for _ in range(n)]for _ in range(n)]
for _ in range(m):
	a,b=map(int,input().split())
	graph[a][a]+=1
	graph[b][b]+=1
	graph[a][b]-=1
	graph[b][a]-=1

for i in graph:
	print(*i)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H8/08.04.2024_12.14.44_REC.png)



### 18160: 最大连通域面积

matrix/dfs similar, http://cs101.openjudge.cn/practice/18160

思路：

bfs搜索得到连通面积，取最大值

代码

```python
def bfs(x,y):
	if x<0 or x>=n or y< 0 or y>=m or lake[x][y]=='.':
		return 0
	lake[x][y]='.'
	return 1+\
	bfs(x+1,y)+\
	bfs(x-1,y)+\
	bfs(x,y-1)+\
	bfs(x,y+1)+\
	bfs(x+1,y+1)+\
	bfs(x+1,y-1)+\
	bfs(x-1,y+1)+\
	bfs(x-1,y-1)

lake=[]

for _ in range(int(input())):
	n,m=map(int,input().split())
	lake=[list(input()) for _ in range(n)]
	ans=0
	for i in range(n):
		for j in range(m):
			if lake[i][j]=='W':
				ans=max(ans,bfs(i,j))
	print(ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/08.04.2024_12.20.33_REC.png)



### sy383: 最大权值连通块

https://sunnywhy.com/sfbj/10/3/383

思路：

用二维list存储连通状态，bfs求值取最大

代码

```python
n,m=map(int,input().split())
value=list(map(int,input().split()))
tu=[[-1 for _ in range(n)]for _ in range(n)]
for i in range(n):
	tu[i][i]=value[i]

for  _ in range(m):
	a,b=map(int,input().split())
	tu[a][b]=value[b]
	tu[b][a]=value[a]

ans=0
stack=[False for _ in range(n)]

def bfs(x):
	ans=0
	list1=[]
	for i in range(n):
		if i!=x and not stack[i] and tu[x][i]!=-1:
			list1.append(i)
			stack[i]=True
			ans+=tu[x][i]
	for i in list1:
		ans+=bfs(i)
	return ans

for i in range(n):
	if not stack[i]:
		stack[i]=True
		ans=max(ans,bfs(i)+value[i])
print(ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H8/08.04.2024_12.32.09_REC.png)



### 03441: 4 Values whose Sum is 0

data structure/binary search, http://cs101.openjudge.cn/practice/03441

思路：

询问GPT获得了哈希表算法，可以把暴力n^4优化为n^2

代码

```python
def fourSumCount(A, B, C, D):
    dict1 = dict()
    for num1 in A:
        for num2 in B:
            dict1[num1 + num2] = dict1.get(num1 + num2, 0) + 1
    
    res = 0
    for num3 in C:
        for num4 in D:
            target = -(num3 + num4)
            if target in dict1:
                res += dict1[target]
    
    return res

n = int(input())
A, B, C, D = [], [], [], []
for _ in range(n):
    a, b, c, d = map(int, input().split())
    A.append(a)
    B.append(b)
    C.append(c)
    D.append(d)

print(fourSumCount(A, B, C, D))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H8/08.04.2024_12.49.25_REC.png)



### 04089: 电话号码

trie, http://cs101.openjudge.cn/practice/04089/

思路：

对数据排序，难更新set，对每个电话号码查找前缀

代码

```python
for _ in range(int(input())):
	n=int(input())
	list1=sorted(list(input()for _ in range(n)))
	set1=set()
	ok=True
	for i in list1:
		if not ok:
			break
		for length in range(1,len(i)+1):
			if i[:length] in set1:
				ok=False
				break
		set1.add(i)
	print('YES' if ok else 'NO')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/08.04.2024_13.13.56_REC.png)



### 04082: 树的镜面映射

http://cs101.openjudge.cn/practice/04082/

思路：

用stack建二叉树，然后转化成children树，最后bfs输出

代码

```python
from collections import defaultdict
class Node:
    def __init__(self,data,left=None,right=None):
        self.data=data
        self.left=left
        self.right=right

def buildtree(list1):
    stack=[]
    for i in list1:
        if i[-1]=='0':
            a=Node(i[0],1,1)
        else :
            a=Node(i[0])
        if stack:
            for i in range(len(stack)-1,-1,-1):
                if stack[i].left==1:
                    stack[i].left=a
                    break
                elif stack[i].right==1:
                    stack[i].right=a
                    break
        stack.append(a)
    return stack[0]

def buildtree1(root,height):
    dic=defaultdict(list)
    while True:
        dic[height].insert(0,root.data)
        if not(root.left==None or root.left.data=='$'):
            for key,value in buildtree1(root.left,height+1).items():
                dic[key]=value+dic[key]
        if root.right==None or root.right.data=='$':
            break
        root=root.right
    return dic
        
n=int(input())
for key,value in buildtree1(buildtree(list(input().split())),1).items():
    print(" ".join(value),end=" ")
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/08.04.2024_13.15.03_REC.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

期中周任务繁重，没有额外练习，补上了每日选做。



