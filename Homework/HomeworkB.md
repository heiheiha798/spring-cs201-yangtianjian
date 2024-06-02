# Homework #B: 图论和树算

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 28170: 算鹰

dfs, http://cs101.openjudge.cn/practice/28170/

思路：

bfs

代码

```python
list1=[list(input()) for _ in range(10)]
from collections import deque
ans=0
used=[[False for _ in range(10)]for _ in range(10)]
dxy=[(0,1),(0,-1),(1,0),(-1,0)]
for i in range(10):
	for j in range(10):
		if list1[i][j]=='.' and not used[i][j]:
			ans+=1
			queue=deque()
			queue.append((i,j))
			while queue:
				x,y=queue.popleft()
				if used[x][y]:
					continue
				used[x][y]=True
				for k in range(4):
					xx,yy=x+dxy[k][0],y+dxy[k][1]
					if xx>=0 and yy>=0 and xx<10 and yy<10 and list1[xx][yy]=='.':
						queue.append((xx,yy))
print(ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240428163430233](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163430233.png)



### 02754: 八皇后

dfs, http://cs101.openjudge.cn/practice/02754/

思路：

搜索加回溯

代码

```python
zuo=[False]*15
you=[False]*15
col=[False]*8
list1=[]

def bahuang(row,ans):
    if row==8:
        list1.append(ans)
        return
    for i in range(8):
        if col[i] or zuo[row+i] or you[row-i+7]:
            continue
        else:
            col[i],zuo[row+i],you[row-i+7]=True,True,True
            bahuang(row+1,ans+str(i+1))
            col[i],zuo[row+i],you[row-i+7]=False,False,False

bahuang(0,"")
for _ in range(int(input())):
    print (list1[int(input())-1])
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240428163521815](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163521815.png)



### 03151: Pots

bfs, http://cs101.openjudge.cn/2024sp_routine/03151/

思路：

bfs

代码

```python
a,b,c=map(int,input().split())
import heapq
heap=[]
heap.append((0,0,0,[]))
used=set()
while heap:
	step,x,y,state=heapq.heappop(heap)
	if (x,y) in used:
		continue
	used.add((x,y))
	if x==c or y==c:
		print(step)
		for i in state:
			print(i)
		exit()
	step+=1
	if x!=0:
		heapq.heappush(heap,(step,0,y,state.copy()+['DROP(1)']))
	if y!=0:
		heapq.heappush(heap,(step,x,0,state.copy()+['DROP(2)']))
	if x!=a:
		heapq.heappush(heap,(step,a,y,state.copy()+['FILL(1)']))
	if y!=b:
		heapq.heappush(heap,(step,x,b,state.copy()+['FILL(2)']))
	if x!=a and y!=0:
		if x+y>a:
			heapq.heappush(heap,(step,a,x+y-a,state.copy()+['POUR(2,1)']))
		else:
			heapq.heappush(heap,(step,x+y,0,state.copy()+['POUR(2,1)']))
	if y!=b and x!=0:
		if x+y>b:
			heapq.heappush(heap,(step,x+y-b,b,state.copy()+['POUR(1,2)']))
		else:
			heapq.heappush(heap,(step,0,x+y,state.copy()+['POUR(1,2)']))
print('impossible')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428163543961](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163543961.png)



### 05907: 二叉树的操作

http://cs101.openjudge.cn/dsapre/05907/

思路：

建树然后分类操作

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

def findleft(root):
    while root and root.left:
        root=root.left
    return (root.data if root else -1)

for _ in range(int(input())):
    n,m=map(int,input().split())
    stack=[Node(x) for x in range(n)]
    for _ in range(n):
        a,b,c=map(int,input().split())
        if b!=-1:
            stack[a].left=stack[b]
        if c!=-1:
            stack[a].right=stack[c]
    root=stack[0]
    for _ in range(m):
        s=list(map(int,input().split()))
        if s[0]==2:
            print(findleft(stack[s[1]]))
        else:
            x,y=s[1],s[2]
            for node in stack:
                if node.left and node.left.data in[x,y]:
                    node.left=stack[y] if node.left.data==x else stack[x]
                if node.right and node.right.data in[x,y]:
                    node.right=stack[y] if node.right.data==x else stack[x]
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428163755408](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163755408.png)



### 18250: 冰阔落 I

Disjoint set, http://cs101.openjudge.cn/practice/18250/

思路：

并查集

代码

```python
def findfather(x):
		if father[x]==x:
			return x
		father[x]=findfather(father[x])
		return father[x]

while True:
	try:
		n,m=map(int,input().split())
		father=[x for x in range(n+1)]

		for _ in range(m):
			x,y=map(int,input().split())
			if findfather(x)==findfather(y):
				print('Yes')
			else:
				print('No')
				father[findfather(y)]=findfather(x)

		for i in range(1,n+1):
			a=findfather(i)

		ans=sorted(list(set(father[1:])))
		print(len(ans))
		print(*ans)

	except EOFError:
		break
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428163838879](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163838879.png)



### 05443: 兔子与樱花

http://cs101.openjudge.cn/practice/05443/

思路：

用多维数组，输入数据后进行处理，同时记录下走过的路径点

代码

```python
p=int (input())
dic={input():i+1 for i in range(p)}
redic={b:a for a,b in dic.items()}
inf=0xfffffffff
grid=[[inf]*(p+1) for _ in range(p+1)]
dp=[[[inf,[0]] for _ in range(p+1)] for _ in range(p+1)]

for _ in range(int(input())):
    s1,s2,distance=input().split()
    id1,id2=dic[s1],dic[s2]
    grid[id1][id2]=grid[id2][id1]=int(distance)

def findway(id1,mid,id2,state,total): 
    if grid[mid][id2]!=inf:
        total+=grid[mid][id2]
        state.append(id2)
        list2=dp[id1][id2]
        list3=dp[id2][id1]
        if total<list2[0]:
            list2[0]=list3[0]=total
            list2[1]=state
            # print(dp[id1][id2])
            stateco=state.copy()
            stateco.reverse()
            list3[1]=stateco
        return
    for i in range(1,p+1):
        if grid[mid][i]!=inf and i not in state:
            stateco=state.copy()
            stateco.append(i)
            findway(id1,i,id2,stateco,total+grid[mid][i])
            
for _ in range(int(input())):
    s1,s2=input().split()
    id1,id2=dic[s1],dic[s2]
    if s1==s2:
        print(s1)
    else :
        findway(id1,id1,id2,[id1],0)
        list0=dp[id1][id2]
        list1=list0[1]
        for i in range (len(list1)-1):
            print(redic[list1[i]]+f"->({grid[list1[i]][list1[i+1]]})->",end="")
        print(redic[list1[-1]])
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240428163909239](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240428163909239.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

都是做过的题，也没有额外练习题目，享受五一假期中



