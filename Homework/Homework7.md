# Assignment #7: April 月考

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows

Python编程环境：Pycharm

## 1. 题目

### 27706: 逐词倒放

http://cs101.openjudge.cn/practice/27706/

思路：

根据题意写代码

代码

```python
s=list(input().split())[::-1]
print(*s)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170447.jpg)



### 27951: 机器翻译

http://cs101.openjudge.cn/practice/27951/

思路：

用一个list存查找过的数据，然后取后m个

代码

```python
m,n=map(int,input().split())
dict1=[]
list1=list(map(int,input().split()))
ans=0
for i in list1:
    if len(dict1)<m:
        if i not in dict1:
            ans+=1
            dict1.append(i)
    else:
        if i not in dict1[-m:]:
            ans+=1
            dict1.append(i)
print(ans)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170512.jpg)



### 27932: Less or Equal

http://cs101.openjudge.cn/practice/27932/

思路：

排序一下，然后注意特判

代码

```python
n,k=map(int,input().split())
list1=sorted(list(map(int,input().split())))
if k==0:
    if list1[0]==1:
        print(-1)
    else:
        print(list1[0]-1)
elif k==n:
    print(list1[-1])
else:
    if list1[k]==list1[k-1]:
        print(-1)
    else:
        print(list1[k-1])
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170529.jpg)



### 27948: FBI树

http://cs101.openjudge.cn/practice/27948/

思路：

递归建树，然后递归后序

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

def FBI(s):
    if '1' in s and '0' in s:
        return 'F'
    elif '1' not in s:
        return 'B'
    return 'I'

def buildtree(s):
    root=Node(FBI(s))
    if len(s)>1:
        ls=s[:len(s)//2]
        rs=s[len(s)//2:]
        root.left=buildtree(ls)
        root.right=buildtree(rs)
    return root

def post(root):
    if not root:
        return ''
    return post(root.left)+post(root.right)+root.data

n=int(input())
s=input()
print(post(buildtree(s)))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170552.jpg)



### 27925: 小组队列

http://cs101.openjudge.cn/practice/27925/

思路：

用一个list存name，一个list存group，然后找相同group

代码

```python
t=int(input())
list1=[list(map(int,input().split()))for _ in range(t)]
dict1=dict()
for i in range(t):
    for j in list1[i]:
        dict1[j]=i
stack=[]
flag=[]
while True:
    s=input()
    if s=='STOP':
        break
    elif s=='DEQUEUE':
        print(stack.pop(0))
        flag.pop(0)
    else:
        a,b=s.split()
        name=int(b)
        group=dict1[name]
        pos=0
        while pos<len(flag) and flag[pos]!=group:
            pos+=1
        while pos<len(flag) and flag[pos]==group:
            pos+=1
        if pos==len(flag):
            flag.append(group)
            stack.append(name)
        else:
            flag.insert(pos,group)
            stack.insert(pos,name)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170612.jpg)



### 27928: 遍历树

http://cs101.openjudge.cn/practice/27928/

思路：

set取数据，dict建树，两个set找根，递归得答案

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.children=[]

def findroot(set1,set2):
    for i in set1:
        if i not in set2:
            return i
def show (root):
    anslist=[]
    root.children.sort(key=lambda x:x.data)
    pos=0
    while pos<len(root.children) and root.children[pos].data<root.data:
        anslist+=show(root.children[pos])
        pos+=1
    anslist.append(root.data)
    while pos<len(root.children):
        anslist+=show(root.children[pos])
        pos+=1
    return anslist

dict1=dict()
dictfather=dict()
n=int(input())
list1=[]
set1=set()
for _ in range(n):
    list2=list(map(int,input().split()))
    list1.append(list2)
    for i in list2:
        set1.add(i)
for i in set1:
    dict1[i]=Node(i)
for i in range(n):
    list2=list1[i]
    for i in list2[1:]:
        dict1[list2[0]].children.append(dict1[i])
        dictfather[i]=list2[0]
set2=set()
for key,value in dictfather.items():
    set2.add(key)
root=dict1[findroot(set1,set2)]
ans=show(root)
for i in ans:
    print(i)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/47587f88aa4413d361c0034d02be853e7e4173cc/img/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202024-04-03%20170635.jpg)



## 2. 学习总结和收获

月考不是很难，笔试让我汗流浃背了，写代码的同时也要注重概念的理解。
