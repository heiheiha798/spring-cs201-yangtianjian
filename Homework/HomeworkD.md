# Homework #D: May月考

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/

思路：

list暴力

代码

```python
l,m=map(int,input().split())
list1=[1 for _ in range(l+1)]
for _ in range(m):
	a,b=map(int,input().split())
	for i in range(a,b+1):
		list1[i]=0
print(sum(list1))
```

代码运行截图 ==（至少包含有"Accepted"）==

![image-20240508172224622](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172224622.png)



### 20449: 是否被5整除

http://cs101.openjudge.cn/practice/20449/

思路：

int转化字符串

代码

```python
s=input()
ans=''
for i in range(1,len(s)+1):
	s1=s[:i]
	if int(s1,2)%5==0:
		ans+='1'
	else:
		ans+='0'
print(ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172153244.png)



### 01258: Agri-Net

http://cs101.openjudge.cn/practice/01258/

思路：

dijkstra，考试忘记try，except了

代码

```python
while True:
	try:
		n=int(input())
	except EOFError:
		break
	list0=[]
	while len(list0)<n*n:
		list0+=list(map(int,input().split()))
	list1=[list0[i*n:i*n+n] for i in range(n)]
	ans=0
	stack=list1[0]
	used=[False for _ in range(n)]
	used[0]=True
	pos=0
	while False in used:
		for i in range(n):
			stack[i]=min(stack[i],list1[pos][i])
		mini=float('inf')
		for i in range(n):
			if not used[i] and stack[i]<=mini:
				mini=stack[i]
				pos=i
		used[pos]=True
		ans+=mini
	print(ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240508172343184](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172343184.png)



### 27635: 判断无向图是否连通有无回路(同23163)

http://cs101.openjudge.cn/practice/27635/

思路：

connected使用bfs看能否都走到，loop用bfs看能否重复走

代码

```python
n,m=map(int,input().split())
list1=[[-1 for _ in range(n)]for _ in range(n)]
for _ in range(m):
	a,b=map(int,input().split())
	list1[a][b]=1
	list1[b][a]=1

used=[False for _ in range(n)]
from collections import deque
queue=deque()
queue.append(0)
used[0]=True
while queue:
	a=queue.popleft()
	for i in range(n):
		if list1[a][i]==1 and not used[i]:
			queue.append(i)
			used[i]=True
print('connected:'+('no' if False in used else 'yes'))

for i in range(n):
	if 1 in list1[i]:
		queue1=deque()
		used=[False for _ in range(n)]
		queue1.append((i,i))
		while queue1:
			pos,father=queue1.popleft()
			if used[pos]:
				print('loop:yes')
				exit()
			used[pos]=True
			for j in range(n):
				if list1[pos][j]==1 and j != father:
					queue1.append((j,pos))
print('loop:no')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240508172451531](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172451531.png)



### 27947: 动态中位数

http://cs101.openjudge.cn/practice/27947/

思路：

用一个最大堆存小值，最小堆存大值，输出中位数

代码

```python
n,m=map(int,input().split())
list1=[[-1 for _ in range(n)]for _ in range(n)]
for _ in range(m):
	a,b=map(int,input().split())
	list1[a][b]=1
	list1[b][a]=1

used=[False for _ in range(n)]
from collections import deque
queue=deque()
queue.append(0)
used[0]=True
while queue:
	a=queue.popleft()
	for i in range(n):
		if list1[a][i]==1 and not used[i]:
			queue.append(i)
			used[i]=True
print('connected:'+('no' if False in used else 'yes'))

for i in range(n):
	if 1 in list1[i]:
		queue1=deque()
		used=[False for _ in range(n)]
		queue1.append((i,i))
		while queue1:
			pos,father=queue1.popleft()
			if used[pos]:
				print('loop:yes')
				exit()
			used[pos]=True
			for j in range(n):
				if list1[pos][j]==1 and j != father:
					queue1.append((j,pos))
print('loop:no')
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240508172548996](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172548996.png)



### 28190: 奶牛排队

http://cs101.openjudge.cn/practice/28190/

思路：

对每个点依次操作，如果遇到小于start就结算，遇到大于next就换next，遇到中间值check一下

代码

```python
n=int(input())
list1=[int(input()) for _ in range(n)]

ans=0
for i in range(n-1):
	start=list1[i]
	next=list1[i+1]
	if start >= next:
		continue
	pos=i+2
	while pos<n:
		a=list1[pos]
		if a<=start:
			break
		elif a>next:
			next=a
		else:
			ok=True
			if max(list1[pos:])>next:
				pos1=pos
				while list1[pos1]<=next:
					pos1+=1
				list2=list1[i:pos1+1]
				if min(list2)==start and list2.count(start)==1:
					ok=False
					next=list1[pos1]
					pos=pos1
			if ok:
				break
		pos+=1
	if pos-i>ans:
		ans=max(ans,pos-i)
		# state=list1[i:pos]
print(ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![image-20240508172616056](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/image-20240508172616056.png)



## 2. 学习总结和收获

==如果作业题目简单，有否额外练习题目，比如：OJ“2024spring每日选做”、CF、LeetCode、洛谷等网站题目。==

本次考试发现很多知识遗忘，try，except没想到，heap也没有第一时间反应过来。

要注意增加程序的鲁棒性，被第五题WA卡了好久。

要多写点题目，最后一分钟写好heap发现样例过不了，考后才发现a打成1了，难绷。

期中都差不多结束了，是不是能回到每天两题的日子了（狗头保命



