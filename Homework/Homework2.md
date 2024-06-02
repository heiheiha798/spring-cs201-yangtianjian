# Homework #2: 编程练习

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 27653: Fraction类

http://cs101.openjudge.cn/2024sp_routine/27653/

思路：

定义一个class，设置init、add、str等函数，附加求最大公因数函数，即可完成分数运算

##### 代码

```python
def gcd(m, n):
    if n == 0:
        return m
    else:
        return gcd(n, m % n)
    
class Fraction:
    def __init__(self,fenzi,fenmu):
        self.fenzi=fenzi
        self.fenmu=fenmu
        
    def __str__(self):
        return str(self.fenzi)+"/"+str(self.fenmu)

    def __add__(self,otherself):
        newfenzi=self.fenzi*otherself.fenmu+self.fenmu*otherself.fenzi
        newfenmu=self.fenmu*otherself.fenmu
        yinzi=gcd(newfenzi,newfenmu)
        return Fraction(newfenzi//yinzi,newfenmu//yinzi)

    def show(self):
        print(self.fenzi,"/",self.fenmu)

a=list(map(int,input().split()))

fenshu1=Fraction(a[0],a[1])
fenshu2=Fraction(a[2],a[3])

ans=fenshu1+fenshu2

print(ans)
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20132301.png)



### 04110: 圣诞老人的礼物-Santa Clau’s Gifts

greedy/dp, http://cs101.openjudge.cn/practice/04110

思路：

分数背包问题，只要根据性价比进行排序即可

##### 代码

```python
n, w = map(int, input().split())
candies = []

for _ in range (n):
    value,weight=map(int,input().split())
    candies.append((value/weight,value,weight))

candies.sort(reverse=True)
sum=0

for ave,value,weight in candies:
    if w>=weight:
        sum+=value
        w-=weight
    else :
        sum+=w*ave
        break    

print("%.1f"%sum)
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20132433.png)



### 18182: 打怪兽

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：

对输入数据根据“时间升序，伤害降序排列”，相同时间取前m个，如果b<=0则输出，退出循环，如果循环结束b>0则输出“alive”

##### 代码

```python
case=int(input())

for _ in range(case):
    n,m,b=map(int,input().split())
    list1=[tuple(map(int,input().split())) for _ in range(n)]
    list1.sort(key=lambda item: (item[0],-item[1]))
    
    time=list1[0][0]
    sum=m
    for item in list1:
        if item[0]==time:
            if sum==0:
                continue
            else :
                sum-=1
                b-=item[1]
                if b<=0:
                    print(time)
                    break
        else :
            time=item[0]
            sum=m-1
            b-=item[1]
            if b<=0:
                print(time)
                break
    if b>0:
        print("alive")
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20132635.png)



### 230B. T-primes

binary search/implementation/math/number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：

根据题给数据范围写一个埃筛，一个T-prime一定是质数的平方，那么只有平方根是整数且是质数的数才是T-prime

##### 代码

```python
list1 =[False]*2+[True]*1000000
pos=2
 
while pos<len(list1):
    if list1[pos]==True:
        for i in range(pos*2,len(list1),pos):
            list1[i]=False
    pos+=1
 
n=input()
list2=list(map(int,input().split()))
 
for i in list2:
    a=int(i**0.5)
    if (a**2)!=i:
        print("NO")
    elif list1[a]==True:
        print("YES")
    else :
        print ("NO")
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20132713.png)



### 1364A. XXXXX

brute force/data structures/number theory/two pointers, 1200, https://codeforces.com/problemset/problem/1364/A

思路：

对数列整体求和，如果不被x整除答案就是n，如果被整除，则比较首位能被x整除的连续项的大小，取小

##### 代码

```python
for _ in range (case):
    n,x=map(int,input().split())
    list1=list(map(int,input().split()))
    
    total=sum(list1)
    
    if total%x!=0:
        print(n)
    else :
        left=0
        right=-1
        while left<n and list1[left]%x==0:
            left+=1
        while right>=-n and list1[right]%x==0:
            right -=1
        if left==n:
            print (-1)
        else :
            print (n-min(left+1,-right))
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20133118.png)



### 18176: 2050年成绩计算

http://cs101.openjudge.cn/practice/18176/

思路：

同样写一个埃筛，和T-prime一样的判别方法，如果没有T-prime就输出0，有就算平均数

##### 代码

```python
prime=[False]*2+[True]*9999

for i in range(2,10001):
    if prime[i]==True:
        j=i*2
        while j<10001:
            prime[j]=False
            j+=i

def isTprime(n):
    a=int(n**0.5)
    if a**2==n and prime[a]==True:
        return True
    else:
        return False

m,n=map(int,input().split())

for _ in range(m):
    list1=list(map(int,input().split()))
    sum=0
    for i in list1:
        if isTprime(i):
            sum+=i
    if sum==0:
        print(0)
    else :
        print("%.2f"%(sum/len(list1)))
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-24%20133342.png)



## 2. 学习总结和收获

本周作业对我来说很有难度，主要难点在于：

1.算法效率太低：如XXXXX这道题，我写了一个O（n^2）的算法，枚举每一个子列并求和，导致超时，花了很长时间，最后不得不求助ChatGPT

2.对于python中数据结构掌握不熟练：在做背包题目时想用struct来储存输入数据，但是python中可以直接把一组数据列为一个元组储存到列表中，这可以大大简化代码

3.对python中函数的参数设置不了解：比如sort（key，reverse），可以设置一个匿名函数lambda来进行简化输入，还需要做题学习

总之，python学习道阻且长，希望能多多AC







