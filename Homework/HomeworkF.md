# Homework #F: All-Killed 满分

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 22485: 升空的焰火，从侧面看

http://cs101.openjudge.cn/practice/22485/

思路：

建树然后bfs

代码

```python
class Node:
	def __init__(self,data):
		self.data=data
		self.left=None
		self.right=None

ansdict={}
def bfs(root,depth):
	if not root :
		return
	ansdict[depth]=ansdict.get(depth,[])+[root.data]
	bfs(root.left,depth+1)
	bfs(root.right,depth+1)

n=int(input())
stack=[Node(x) for x in range(n+1)]
for i in range(n):
	a,b=map(int,input().split())
	if a!=-1:
		stack[i+1].left=stack[a]
	if b!=-1:
		stack[i+1].right=stack[b]
root=stack[1]
bfs(root,1)
ans=[ansdict[x][-1] for x in ansdict.keys()]
print(*ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240520185852003](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520185926275.png)



### 28203:【模板】单调栈

http://cs101.openjudge.cn/practice/28203/

思路：

学习了模板

代码

```python
n=int(input())
list1=list(map(int,input().split()))
ans=[0 for _ in range(n)]
stack=[]
for i in range(n):
	if not stack or list1[i]<=stack[-1][1]:
		pass
	else:
		while stack and list1[i]>stack[-1][1]:
			pos,_=stack.pop()
			ans[pos]=i+1
	stack.append((i,list1[i]))
print(*ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240520191549479](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520191549479.png)



### 09202: 舰队、海域出击！

http://cs101.openjudge.cn/practice/09202/

思路：

检查图里面有没有环

代码

```python
for _ in range(int(input())):
	n,m=map(int,input().split())
	dict1={x:[] for x in range(1,n+1)}
	for _ in range(m):
		x,y=map(int,input().split())
		dict1[x].append(y)
	hasloop=False
	used=[False for _ in range(n+1)]
	for key in dict1:
		if hasloop:break
		if not used[key]:
			stack=[(key,[])]
			used[key]=True
			while stack:
				if hasloop:break
				name,state=stack.pop()
				for i in dict1[name]:
					if i in state:
						hasloop=True
						break
					if not used[i]:
						used[i]=True
						stack.append((i,state.copy()+[name]))
	
	
	print('Yes' if hasloop else 'No')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240520191628414](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520191628414.png)



### 04135: 月度开销

http://cs101.openjudge.cn/practice/04135/

思路：

二分法历遍

代码

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
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240520191723925](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520191723925.png)



### 07735: 道路

http://cs101.openjudge.cn/practice/07735/

思路：

dfs，找出满足toll的最小length

代码

```python
class edge:
	def __init__(self,name,length,toll):
		self.name=name
		self.length=length
		self.toll=toll

coin=int(input())
n=int(input())
citi=[[] for _ in range(n+1)]
for _ in range(int(input())):
	sourse,destination,length,toll=map(int,input().split())
	citi[sourse].append(edge(destination,length,toll))

import heapq
ans=float('inf')
heap=[(-1,0,0,[])]
while heap:
	pos,sumlen,sumtoll,state=heapq.heappop(heap)
	pos=-pos
	if sumlen >=ans or sumtoll>coin:
		continue
	if pos==n:
		ans=sumlen
	for item in citi[pos]:
		if item.name not in state:
			heapq.heappush(heap,(-item.name,sumlen+item.length,sumtoll+item.toll,state.copy()+[pos]))

print(-1 if ans==float('inf') else ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240520191808949](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520191808949.png)



### 01182: 食物链

http://cs101.openjudge.cn/practice/01182/

思路：

如果a和b是同类，肯定有a_n与b_n连通，即有相同的根节点；如果a吃b，肯定有a_n与b_2n连通

代码

```python
def findfather(x,father):
	if father[x]!=x:
		father[x]=findfather(father[x],father)
	return father[x]

def union(n,statement):
	father=[x for x in range(3*n+1)]
	ans=0
	for type,x,y in statement:
		if x>n or y>n or type==2 and x==y:
			ans+=1
			continue
		if findfather(x,father)==findfather(y,father) and type==2:
			ans+=1
		elif findfather(x,father)==findfather(y+n,father) and type==1:
			ans+=1
		elif findfather(y,father)==findfather(x+n,father) :
			ans+=1
		else:
			if type==1:
				father[findfather(y,father)]=findfather(x,father)
				father[findfather(y+n,father)]=findfather(x+n,father)
				father[findfather(y+2*n,father)]=findfather(x+2*n,father)
			else:
				father[findfather(y+n,father)]=findfather(x,father)
				father[findfather(y+2*n,father)]=findfather(x+n,father)
				father[findfather(y,father)]=findfather(x+2*n,father)
	
	print(ans)

n,k=map(int,input().split())
list1=[list(map(int,input().split())) for _ in range(k)]
union(n,list1)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240520191848771](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240520191848771.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

基本上都是做过的题

在复习笔试做笔试题
