# Homework #A: 图论：遍历，树算及栈

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 20743: 整人的提词本

http://cs101.openjudge.cn/practice/20743/

思路：

每次找到‘）’就往前查找‘（’并进行局部翻转

代码

```python
s=input()
stack=[]
ans=[]
for i in s:
	if i!=')':
		stack.append(i)
	else :
		pos=len(stack)-stack[::-1].index("(")-1
		stack=stack[:pos]+stack[pos+1:][::-1]

print("".join(stack))   
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240421212850133](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421212850133.png)



### 02255: 重建二叉树

http://cs101.openjudge.cn/practice/02255/

思路：

递归建树，然后递归后序

代码

```python
def ercha(qian,zhong):
    if len(qian)<2:
        return qian
    if len(qian)==2:
        return qian[::-1]
    head=qian[0]
    zl=zhong[:zhong.find(head)]
    zr=zhong[zhong.find(head)+1:]
    ql=qian[1:1+len(zl)]
    qr=qian[1+len(zl):]
    return ercha(ql,zl)+ercha(qr,zr)+head
    
while True:
    try:
        s1,s2=input().split()
        print(ercha(s1,s2))
    except EOFError:
        break
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240421213058726](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421213058726.png)



### 01426: Find The Multiple

http://cs101.openjudge.cn/practice/01426/

要求用bfs实现

思路：

类似bfs，从0开始搜索二进制的数查看是否有倍数关系

代码

```python
while True:
    n=int(input())
    if n==0:
        break
    num=1
    while True:
        ans=int(bin(num)[2:])
        if ans%n==0:
            print(ans)
            break
        num+=1
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240421213207684](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421213207684.png)



### 04115: 鸣人和佐助

bfs, http://cs101.openjudge.cn/practice/04115/

思路：

用bfs，第一排序时间，第二排序查克拉，用一个dict存储查克拉的最小时间进行剪枝

代码

```python
m,n,t=map(int,input().split())
list1=[list(input())for _ in range(m)]
bx,ex,by,ey=0,0,0,0
for i in range(m):
	for j in range(n):
		if list1[i][j]=='@':
			bx,by=i,j
			list1[i][j]='*'
		if list1[i][j]=='+':
			ex,ey=i,j
			list1[i][j]='*'

import heapq
dxy=[(0,1),(0,-1),(1,0),(-1,0)]
heap=[(0,t,bx,by)]
dict1={}
finalans=-1
while heap:
	ans,cha,x,y=heapq.heappop(heap)
	if dict1.get((x,y,cha),0)!=0:
		if ans>=dict1[(x,y,cha)]:
			continue
	dict1[(x,y,cha)]=ans
	if x==ex and y==ey:
		finalans=ans
		break
	for i in range(4):
		xx,yy=x+dxy[i][0],y+dxy[i][1]
		if xx>=0 and yy>=0 and xx<m and yy<n:
			if list1[xx][yy]=='#':
				if cha>0:
					heapq.heappush(heap,(ans+1,cha-1,xx,yy))
			else:
				heapq.heappush(heap,(ans+1,cha,xx,yy))
	
print(finalans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240421220115988](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421220115988.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：

还是bfs，换了一个评估函数

代码

```python
m,n,p=map(int,input().split())
list1=[list(input().split())for _ in range(m)]
for i in range(m):
	for j in range(n):
		if list1[i][j].isdigit():
			list1[i][j]=int(list1[i][j])

import heapq
def bfs(list2):
	bx,by,ex,ey=list2[0],list2[1],list2[2],list2[3]
	dxy=[(0,-1),(0,1),(1,0),(-1,0)]
	if list1[bx][by]=='#' or list1[ex][ey]=='#':
		print('NO')
		return 
	heap=[(0,bx,by)]
	used=[[float('inf') for _ in range(n)]for _ in range(m)]
	while heap:
		ans,x,y=heapq.heappop(heap)
		if x==ex and y==ey:
			print(ans)
			return
		if ans>=used[x][y]:
			continue
		used[x][y]=ans
		for i in range(4):
			xx,yy=x+dxy[i][0],y+dxy[i][1]
			if xx>=0 and yy>=0 and xx<m and yy<n:
				if list1[xx][yy]!='#':
					heapq.heappush(heap,(ans+abs(list1[xx][yy]-list1[x][y]),xx,yy))
	print('NO')

for _ in range(p):
	bfs(list(map(int,input().split())))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240421222816018](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421222816018.png)



### 05442: 兔子与星空

Prim, http://cs101.openjudge.cn/practice/05442/

思路：

学习了题解，每次找已使用节点的边中权值最小的，然后添加节点，实际上就是学会dijkstra的实现

代码

```python
n=int(input())
dict1={chr(65+i):{} for i in range(n)}
for _ in range(n-1):
	list1=list(input().split())
	name=list1[0]
	num=int(list1[1])
	for i in range(num):
		name1,weight=list1[i*2+2],int(list1[i*2+3])
		dict1[name][name1]=weight
		dict1[name1][name]=weight
	
used=set()
used.add('A')
import heapq
heap=[(cost,'A',name) for name,cost in dict1['A'].items()]
heapq.heapify(heap)
anslist=[]
while heap:
	cost,fro,to=heapq.heappop(heap)
	if to in used:
		continue
	anslist.append(cost)
	used.add(to)
	for toto,cos in dict1[to].items():
		if toto not in used :
			heapq.heappush(heap,(cos,to,toto))

print(sum(anslist))	
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240421230505739](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240421230505739.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

练习了一些图的题目，有一定熟练度后写起来感觉好很多

还是有题目不会，只能进行面向题解的编程



