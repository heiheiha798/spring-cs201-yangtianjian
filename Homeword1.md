# Homework #1: 拉齐大家Python水平

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12


## 1.题目

### 20742: 泰波拿契數

http://cs101.openjudge.cn/practice/20742/

思路：

用四个变量进行迭代，循环次数为n-2

**代码**：

```python
a=0 
b=1
c=1
d=int(input())-2
for i in range(d):
    e=a+b+c
    a=b
    b=c
    c=e

print (c)
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20214052.png)



### 58A. Chat rooms

http://codeforces.com/problemset/problem/58/A

思路：

将“hello”拆成list，对string依次查找字母，若有一次找不到则pos<5，能全找到则pos=5，根据pos==5的真假进行输出

##### 代码：

```python
![Screenshot 2024-02-19 215456](C:\Users\13071\Desktop\大一下\数算B\Homework\#1\Screenshot 2024-02-19 215456.png)s=input()
list1=["h","e","l","l","o"]
inde=0
pos=0
while(inde!=-1 and pos<5 and inde<=len(s)):
    inde=s.find(list1[pos],inde)
    if (inde==-1):
        break
    pos+=1
    inde+=1
if (pos==5):
    print("YES")
else :
    print ("NO")
```

截图 ：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20215456.png)



### 118A. String Task

http://codeforces.com/problemset/problem/118/A

思路：

设置“aoyeui”字符串，对输入字符串小写化后进行查找，如果不是元音就放到ans字符串末尾

**代码**：

```python
s=input().lower()
yuan ="aoyeui"
ans="."
for ch in range (len(s)):
    if (yuan.find(s[ch])==-1):
        ans+=s[ch]
        ans+="."
ans=ans[:-1]
print (ans)
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20220538.png)



### 22359: Goldbach Conjecture

http://cs101.openjudge.cn/practice/22359/

思路：

定义一个质数判断函数，从2开始到a//2进行循环判断是否全是质数

##### 代码

```python
a=int(input ())
def isprime (n):
    if (n<2):
        return False 
    elif (n<4):
        return True
    elif (n%2==0 or n%3==0):
        return False 
    else :
        for i in range (5,n,6):
            if (n==i or n==i+2):
                return True
        return False
for i in range (2,a//2):
    if (isprime (i) and isprime (a-i)):
        print (i,a-i)
        break
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20221549.png)



### 23563: 多项式时间复杂度

http://cs101.openjudge.cn/practice/23563/

思路：

输入字符串按“+”进行分割，对每一项检查：幂次是否大于零、系数是否是零，对合格的项比大小

##### 代码：

```python
s=input().split("+")
ans=0
for i in s:
    pos=i.find("n")
    if (pos==-1):
        continue
    elif (pos!=0 and int(i[:pos])==0):
        continue
    else :
        ans=max(ans,int(i[pos+2:]))
print ("n^"+str(ans))
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20232317.png)



### 24684: 直播计票

http://cs101.openjudge.cn/practice/24684/

思路：

将输入转化为int类型的数组，用桶来统计票数，最终比大小得出最高票的index

##### 代码：

```python
s = list(map(int,input().split()))
list1 =[0]*100001
for i in s:
    list1[i]+=1
maxi=max(list1)
for i in range (100001):
    if (list1[i]==maxi):
        print (i,end=" ")
```

截图：

![](https://raw.githubusercontent.com/heiheiha798/spring-cs201-yangtianjian/main/img/Screenshot%202024-02-19%20234309.png)



## 2. 学习总结和收获

零基础学习python的第一天，学习了python的基本语法，尝试运用python解决一些简单的问题，学习并逐渐熟悉常见函数如map、split、list、for、while的用法，希望能够使用得更加熟练，算法能够更加简洁
