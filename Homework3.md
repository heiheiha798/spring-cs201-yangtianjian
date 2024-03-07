# Homework #3: March月考

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows

Python编程环境：python pycharm

## 1. 题目

**02945: 拦截导弹**

http://cs101.openjudge.cn/practice/02945/

思路：

寻找最长有序非上升子序列

##### 代码

```python
list1=[]
n=int(input())
list2=list(map(int,input().split()))
for i in list2:
    if not list1:
        list1.append(i)
    else :
        for j in range(len(list1)):
            if i>list1[j]:
                list1[j]=i
                break
            if j==len(list1)-1:
                list1.append(i)
                break
print(len(list1))
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20174420.jpg)

**04147:汉诺塔问题(Tower of Hanoi)**

http://cs101.openjudge.cn/practice/04147

思路：

设置起点，终点，经过点进行递归，要挪第n大的到终点就得把上面（n-1）个移到经过点，最后还要移回去

##### 代码

```python
n,a,b,c=input().split()
n=int(n)

def hano(x,st,mi,fi,id):
    if x==1:
        print (str(id)+":"+st+"->"+fi)
    else :
        hano(x-1,st,fi,mi,id-1)
        hano(1,st,mi,fi,id)
        hano(x-1,mi,st,fi,x-1)

hano(n,a,b,c,n)
```

代码运行截图 ==（至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20174612.jpg)



**03253: 约瑟夫问题No.2**

http://cs101.openjudge.cn/practice/03253

思路：

模拟报数，对第x个进行循环，对应一个编号list进行pop

##### 代码

```python
while True:
    n,p,m=map(int,input().split())
    if n==0:
        break
    list1=[x for x in range(n+1)]
    ans=[]
    while len(list1)>1:
        p -= 1
        p=(p+m-1)%n+1
        n-=1
        ans.append(list1.pop(p))
    print(",".join(map(str,ans)))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20174758.jpg)



**21554:排队做实验 (greedy)v0.2**

http://cs101.openjudge.cn/practice/21554

思路：

让耗时最短的最先打水

##### 代码

```python
n=int(input())
list1=list(map(int,input().split()))
list2=[(i+1,list1[i]) for i in range(n)]

list2.sort(key=lambda x:x[1])
ans=[]
for a,b in list2:
    ans.append(a)
print (" ".join(map(str,ans)))

sum=0

for i in range(n):
    sum+=(n-1-i)*list2[i][1]

print("%.2f"%(sum/n))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20174905.jpg)



**19963:买学区房**

http://cs101.openjudge.cn/practice/19963

思路：

按照题目的意思，先对输入进行处理，然后计算性价比和两个中位数，对每组数据进行比较

##### 代码

```python
n=int(input())
s=list(input().split())
price=list(map(int,input().split()))
dis=[]
for i in s:
    a=int(i[1:i.find(",")])
    b=int(i[i.find(",")+1:-1])
    dis.append(a+b)
xingjiabi=[dis[i]/price[i] for i in range(n)]
xingjiabi1=sorted(xingjiabi)
price1=sorted(price)
if n%2==0:
    xing1=(xingjiabi1[n//2]+xingjiabi1[n//2-1])/2
    jia1=(price1[n//2]+price1[n//2-1])/2
else :
    xing1=xingjiabi1[n//2]
    jia1=price1[n//2]

sum=0
for i in range(n):
    if price[i]<jia1 and xingjiabi[i]>xing1:
        sum+=1
print(sum)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20175313.jpgg)



**27300: 模型整理**

http://cs101.openjudge.cn/practice/27300

思路：

输入数据放到一个字典里，然后自定义一个cmp函数进行sort

##### 代码

```python
from collections import defaultdict
import sys
from functools import cmp_to_key

dic=defaultdict(list)
names=[]

for _ in range(int(input())):
    s=input()
    pos=s.find("-")
    name=s[:pos]
    a=s[pos+1:]
    dic[name].append(a)
    if name not in names:
        names.append(name)

def comp(s1,s2):
    if s1[-1]==s2[-1]:
        if float(s1[:-1])>float(s2[:-1]):
            return 1
        else :
            return -1
    else :
        if s1[-1]=='B':
            return 1
        else :
            return -1

names.sort()
for i in names:
    print(i+": ",end="")
    ans=dic[i].copy()
    ans.sort(key=cmp_to_key(comp))
    print(", ".join(ans))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://github.com/heiheiha798/spring-cs201-yangtianjian/blob/2ddacca0a438a91b903679f2ff5797ce5b19c788/img/Screenshot%202024-03-06%20175636.jpg)



## 2. 学习总结和收获

这次作业是在机房进行的月考，需要多多适应电脑的键盘和PyCharm的使用。因为平时用的是VSCode，用PyCharm很不习惯。

做题目感觉对各种数据结构还是不熟悉，考试的时候后会出一些非法操作报错。以及对各种库和函数不够熟悉，考试的时候不知道sort怎么使用自定义的比较函数，最终求助搜索引擎（汗）。

2024spring每日选做感觉问题不大，等我做完各种水课的pre和论文就尽量坐一坐其他题库的题目，之前做了一点，但是数图完全不会，故而搁置了。

关于最后一题的sort，学习了一下之后我觉得可以不用重定义一个cmp函数，可以

```python
def compare(s):
    if s[-1]=='B':
        return float(s[:-1])*1000
    else :
        return float(s[:-1])
```

然后

```python
    ans.sort(key=compare)
```

即可，考试时候把我绕晕了（QAQ）
