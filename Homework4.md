# Homework #4: 排序、栈、队列和树

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 05902: 双端队列

http://cs101.openjudge.cn/practice/05902/

思路：

用list模拟deque

代码

```python
for _ in range(int(input())):
    list1=[]
    for _ in range(int(input())):
        a,b=map(int,input().split())
        if a==1:
            list1.append(b)
        else:
            if b==0:
                list1.pop(0)
            else :
                list1.pop()
    if list1:
        print(*list1)
    else :
        
        print("NULL")
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/f1990c39261a99b979ad9f996ed713b524d80775/img/Screenshot%202024-03-11%20114456.jpg)


### 02694: 波兰表达式

http://cs101.openjudge.cn/practice/02694/

思路：

递归，算符就继续读入，数字有两个就返回进行运算

代码

```python
import math

list1=list(input().split())

pos=-1

def read():
    global pos
    pos+=1
    if list1[pos]=='+':
        return read()+read()
    elif list1[pos]=='-':
        return read()-read()
    elif list1[pos]=='*':
        return read()*read()
    elif list1[pos]=='/':
        return read()/read()
    else :
        return float(list1[pos])

print("%f\n"%(read()))
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/f1990c39261a99b979ad9f996ed713b524d80775/img/Screenshot%202024-03-11%20114712.jpg)



### 24591: 中序表达式转后序表达式

http://cs101.openjudge.cn/practice/24591/

思路：

用栈储存运算符和数字，设定算符优先等级，每个括号结束就pop到答案list中，遇到运算符高级跟在低级后面就pop直到低级跟在高级后，最终输出。关于float读入，因为数据对4.0—>4这种类型没有要求，只要按字符串读入即可。

代码

```python
def deng(s):
    if s in "*/":
        return 2
    elif s in "+-":
        return 1
    else :
        return 0

for _ in range(int(input())):
    s=input().strip()
    num,ops=[],[]
    pos=0
    while pos<len(s
        if s[pos].isdigit():
            a=s[pos]
            pos+=1
            while pos<len(s) and (s[pos].isdigit() or s[pos]=='.'):
                a+=s[pos]
                pos+=1
            num.append(a)
        elif s[pos]=='(':
            ops.append(s[pos])
            pos+=1
        elif s[pos] in "+-*/":
            while ops and deng(s[pos])<=deng(ops[-1]):
                num.append(ops.pop())
            ops.append(s[pos])
            pos+=1
        elif s[pos]==')':
            while ops[-1]!='(':
                num.append(ops.pop())
            ops.pop()
            pos+=1
    while ops:
        num.append(ops.pop())
    print(" ".join(num))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/f1990c39261a99b979ad9f996ed713b524d80775/img/Screenshot%202024-03-11%20115423.jpg)



### 22068: 合法出栈序列

http://cs101.openjudge.cn/practice/22068/

思路：

对每一对进行入栈出栈模拟，如果入了给定字符串的头子母就pop，否则一直append到stack中，最后检查stack[::-1]是否对应给定字符串剩下的部分

代码

```python
s0=input()
while True:
    try:
        s=input()
        list1,stack=list(s0),[]
        pos=0
        if len(s)!=len(s0):
            print("NO")
            continue
        while list1:
            if stack and stack[-1]==s[pos]:
                stack.pop()
                pos+=1
            else:
                stack.append(list1.pop(0))
        while stack and stack[-1]==s[pos]:
            pos+=1
            stack.pop()
        if not stack and pos==len(s):
            print("YES")
        else :
            print ("NO")
    except EOFError:
        break
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/f15e050269d4a75448ea2bfe26fb3078908bca43/img/Screenshot%202024-03-11%20115634.jpg)



### 06646: 二叉树的深度

http://cs101.openjudge.cn/practice/06646/

思路：

定义Node类，写一个建树的函数，对给定数据进行建树，最后写一个递归函数查找最大深度

代码

```python
class Node:
    def __init__(self,value):
        self.value=value
        self.left=None
        self.right=None

def buildtree(nodes):
    root=Node(1)
    treedic={1:root}
    for idx,(left,right) in enumerate(nodes,start=1):
        if left!=-1:
            treedic[idx].left=Node(left)
            treedic[left]=treedic[idx].left
        if right!=-1:
            treedic[idx].right=Node(right)
            treedic[right]=treedic[idx].right
    return root

def findmax(root):
    if not root:
        return 0
    return max(findmax(root.left),findmax(root.right))+1
    
n=int(input())
nodes=(tuple(map(int,input().split())) for _ in range(n))
tree=buildtree(nodes)
maxi=findmax(tree)
print(maxi)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-03-11%20115742.jpg)



### 02299: Ultra-QuickSort

http://cs101.openjudge.cn/practice/02299/

思路：

模拟排序，因为数据量很大，不能冒泡，用nlogn的mergesort进行递归，同时中间还要记录交换次数，这题主要参考题解，自己写了一遍

代码

```python
def mergesort(list1):
    if len(list1)==1:
        return list1,0
    mid=len(list1)//2
    left,levalue=mergesort(list1[:mid])
    right,rivalue=mergesort(list1[mid:])
    merlist,mevalue=merge(left,right)
    return merlist,levalue+rivalue+mevalue

def merge(list1,list2):
    ans=[]
    i=j=0
    total=0
    while i<len(list1) and j<len(list2):
        if list1[i]<list2[j]:
            ans.append(list1[i])
            i+=1
        else :
            ans.append(list2[j])
            j+=1
            total+=len(list1)-i
    ans+=list1[i:]+list2[j:]
    return ans,total
while True:
	n=int(input())
	if n==0:
		break
	list1=[int(input()) for _ in range(n)]
	finallist,ans=mergesort(list1)
	print(ans)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-03-11%20115954.jpg)



## 2. 学习总结和收获

本周把数算pre每日选做做了一下，跳了树图相关的题目。但是每日选做有了树的题目，被迫学习，自己又写了文本二叉树，有点感觉，但是其他课事情略多，周末再做做树的题目。

做题的时候感觉对各种数据类型和语法仍有缺漏，有时一道题提交十几遍还是报错，换一个高效的算法或数据结构就AC了，要多多积累。





