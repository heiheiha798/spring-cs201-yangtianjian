# Homework #5: "树"算：概念、表示、解析、遍历

2024 spring, Complied by ==杨天健、信息科学技术学院==

**编程环境**

操作系统：Windows 10 22H2

Python编程环境：VScode    python 3.8.12

## 1. 题目

### 27638: 求二叉树的高度和叶子数目

http://cs101.openjudge.cn/practice/27638/

思路：

建树，出现-1，-1数据就是一个叶节点，然后用递归求出高度

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

sumleaf=0
n=int(input())
stack=[Node(i) for i in range(n)]
ok=[True]*n
for i in range(n):
    a,b=map(int,input().split())
    if a==-1 and b==-1:
        sumleaf+=1
    if a!=-1:
        stack[i].left=stack[a]
        ok[a]=False
    if b!=-1:
        stack[i].right=stack[b]
        ok[b]=False

rootpos=0

for i in range(n):
    if ok[i]:
        rootpos=i
        break
root=stack[rootpos]
def findmax(root):
    return 1+max((0 if root.left==None else findmax(root.left)),(0 if root.right==None else findmax(root.right)))

print(f"{findmax(root)-1} {sumleaf}")
```

代码运行截图 ==（至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20114456.jpg)



### 24729: 括号嵌套树

http://cs101.openjudge.cn/practice/24729/

思路：

类似计算表达式的处理方法，遇到“(”就加一层，遇到")"就把当前的全部设置成子节点，然后清空，回到上一层。前序后序用递归就行。

代码

```python
class Node:
    def __init__ (self,data):
        self.data=data
        self.children=[]

def pre(root):
    ans=root.data
    if root.children:
        for i in root.children:
            ans+=pre(i)
    return ans

def post(root):
    ans=""
    if root.children:
        for i in root.children:
            ans+=post(i)
    ans+=root.data
    return ans

from collections import defaultdict        
s= input()
dic=defaultdict(list)
depth=0
#buildtree
for i in s:
    if i == '(':
        depth+=1
    elif i==',':
        pass
    elif i==')':
        dic[depth-1][-1].children+=dic[depth]
        dic[depth].clear()
        depth-=1
    else :
        dic[depth].append(Node(i))
root=dic[0][0]
print(pre(root))
print(post(root))
```

代码运行截图 ==（至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20114712.jpg)



### 02775: 文件结构“图”

http://cs101.openjudge.cn/practice/02775/

思路：

同样是先建树，根据"]"进行深度加减，然后注意要排序，进行一个递归输出就行。

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.children=[]

prefix='|     '
def show(root,depth):
    print(prefix*depth+root.data)
    dlist=[]
    flist=[]
    for i in root.children:
        if i.data[0]=='f':
            flist.append(i)
        else :
            dlist.append(i)
    if dlist:
        for i in dlist:
            show(i,depth+1)
    if flist:
        for i in sorted(flist,key=lambda x :x.data):
            print(prefix*depth+i.data)
case=1
root=Node('ROOT')
stack=[root]
while True:
    s=input()
    if s=='#':
        break
    elif s=='*':
        print(f'DATA SET {case}:')
        case+=1
        show(root,0)
        print()
        root=Node('ROOT')
        stack=[root]
    else:
        if s[0]==']':
            stack.pop()
        else:
            a=Node(s)
            stack[-1].children.append(a)
            if s[0]=='d':
                stack.append(a)
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20115423.jpg)



### 25140: 根据后序表达式建立队列表达式

http://cs101.openjudge.cn/practice/25140/

思路：

需要区分大小写，不能把islower写成lower（汗，检查了好久），用stack进行建树，然后用defaultdict进行转换，然后输出就行。

代码

```python
class Node:
    def __init__(self,data):
        self.data=data
        self.left=None
        self.right=None

from collections import defaultdict
dic=defaultdict(list)    

def buildtree(s):
    stack=[]
    for i in s:
        if i.islower():
            stack.append(Node(i))
        else :
            a=Node(i)
            a.right=stack.pop()
            a.left=stack.pop()
            stack.append(a)
    return stack[0]

def dfs(root,depth):
    dic[depth].append(root.data)
    if root.right!=None:
        dfs(root.right,depth+1)
    if root.left!=None:
        dfs(root.left,depth+1)        

for _ in range(int(input())):
    s=input()
    root=buildtree(s)
    dic.clear()
    dfs(root,0)
    ans=[]
    for i in range(len(dic) - 1, -1, -1):
        ans += dic[i]
    print("".join(ans))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20115634.jpg)



### 24750: 根据二叉树中后序序列建树

http://cs101.openjudge.cn/practice/24750/

思路：

对中序后序进行递归处理建树，然后递归输出前序就结束了，值得注意的是对zhong和hou为空的处理，我印象中是有必要的，但是否有更简单的方法我没有细想，因为数算的精髓在复用（狗头）。

代码

```python
class Node:
    def __init__ (self ,data):
        self.data=data
        self.left=None
        self.right=None

def buildtree(zhong,hou):
    root=Node(hou[-1])
    if len(hou)==1:
        return root
    pos=zhong.find(hou[-1])
    if pos==len(zhong)-1:
        root.left=buildtree(zhong[:-1],hou[:-1])
    elif pos==0:
        root.right=buildtree(zhong[1:],hou[:-1])
    else :
        zl=zhong[:pos]
        zr=zhong[pos+1:]
        hl=hou[:len(zl)]
        hr=hou[len(zl):-1]
        root.left=buildtree(zl,hl)
        root.right=buildtree(zr,hr)
    return root

def pre(root):
    return root.data+(""if root.left==None else pre(root.left))+(""if root.right==None else pre(root.right))


zhong=input()
hou=input()
root=buildtree(zhong,hou)
print(pre(root))
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20115742.jpg)



### 22158: 根据二叉树前中序序列建树

http://cs101.openjudge.cn/practice/22158/

思路：

对前序中序进行递归处理建树，然后递归输出后序就结束了。

代码

```python
class Node:
    def __init__ (self ,data):
        self.data=data
        self.left=None
        self.right=None

def buildtree(qian,zhong):
    root=Node(qian[0])
    if len(qian)==1:
        return root
    pos=zhong.find(qian[0])
    if pos==len(zhong)-1:
        root.left=buildtree(qian[1:],zhong[:pos])
    elif pos==0:
        root.right=buildtree(qian[1:],zhong[1:])
    else :
        zl=zhong[:pos]
        zr=zhong[pos+1:]
        ql=qian[1:len(zl)+1]
        qr=qian[len(zl)+1:]
        root.left=buildtree(ql,zl)
        root.right=buildtree(qr,zr)
    return root

def post(root):
    return (""if root.left==None else post(root.left))+(""if root.right==None else post(root.right))+root.data

while True:
    try:
        qian=input()
        zhong=input()
        root=buildtree(qian,zhong)
        print(post(root))
    except EOFError:
        break
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](C:/Users/13071/Desktop/24-1/DSA-B/Homework/H4/Screenshot%202024-03-11%20115954.jpg)



## 2. 学习总结和收获

本周做树的题目感觉基本思路都不变，class Node和buildtree写了好多遍，果然数算的精髓在于复用。

但在做题的时候时常会遇到一直出错该不对的情况，大致可以分为：

1.算法不对，但是没有意识到，基本靠问GPT解决

2.看错题，或数据的属性写着写着忘记了，自动脑补了一个错误的，找错找不出应该先读题

3.没有利用题目的特点直接暴力计算，需要多多积累，学习样例代码

4.暂时性遗忘还有那些奇妙错误，更多有待补充







