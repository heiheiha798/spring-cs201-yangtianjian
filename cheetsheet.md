**#1.python中sort的使用：**

```python
list.sort(key=None, reverse=False)
```

key — 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。

reverse — 排序规则，**reverse = True** 降序， **reverse = False** 升序（默认）

```python
#比如说对一个元素是list的list进行排序：

data = [[3, 4], [1, 2], [3, 1], [2, 3]]

data.sort(key=lambda x: (x[0], -x[1]))

#表示用来比较的元素是先第一个元素，后第二个元素的相反数，也就是第一个升序，第二个降序
```

当比较方法十分复杂的时候我们可以在sort外部定义一个比较函数

```python
def custom_sort_key(item):
    unit_weights = {'K': 1, 'M': 2, 'B': 3} 
    number_part, unit_part = item[:-1], item[-1]
    unit_weight = unit_weights.get(unit_part, 0)
    return (unit_weight, float(number_part))

data = ["1.23B", "5.67K", "10.5M", "3.21B"]
data.sort(key=custom_sort_key)
```



**#2.Kadane算法**

```python
# openjudge 02766
def kadane(list1):
    globalmax,localmax = list1[0],list1[0]
    for i in list1[1:]:
        localmax=max(localmax+i,i)
        globalmax=max(globalmax,localmax)
    return globalmax

n=int(input())
inp=[]
grid=[]
while True:
    for i in list(map(int,input().split())):
        inp.append(i)
    if len(inp)==n*n:
        for i in range(0,n*n,n):
            grid.append(inp[i:i+n])
        maxi=0
        for height in range(1,n+1):
            for i in range(0,n-height+1):
                temp=[0]*n
                for j in range(height):
                    for k in range(n):
                        temp[k]+=grid[i+j][k]
                maxi=max(kadane(temp),maxi)
        print(maxi)
        break
```

**#3.二叉树建树(未懂)**

```python
class Node:
    def __init__(self, x, depth):
        self.x = x
        self.depth = depth
        self.lchild = None
        self.rchild = None

    def preorder_traversal(self):
        nodes = [self.x]
        if self.lchild and self.lchild.x != '*':
            nodes += self.lchild.preorder_traversal()
        if self.rchild and self.rchild.x != '*':
            nodes += self.rchild.preorder_traversal()
        return nodes

    def inorder_traversal(self):
        nodes = []
        if self.lchild and self.lchild.x != '*':
            nodes += self.lchild.inorder_traversal()
        nodes.append(self.x)
        if self.rchild and self.rchild.x != '*':
            nodes += self.rchild.inorder_traversal()
        return nodes

    def postorder_traversal(self):
        nodes = []
        if self.lchild and self.lchild.x != '*':
            nodes += self.lchild.postorder_traversal()
        if self.rchild and self.rchild.x != '*':
            nodes += self.rchild.postorder_traversal()
        nodes.append(self.x)
        return nodes


def build_tree():
    n = int(input())
    for _ in range(n):
        tree = []
        stack = []
        while True:
            s = input()
            if s == '0':
                break
            depth = len(s) - 1
            node = Node(s[-1], depth)
            tree.append(node)

            # Finding the parent for the current node
            while stack and tree[stack[-1]].depth >= depth:
                stack.pop()
            if stack:  # There is a parent
                parent = tree[stack[-1]]
                if not parent.lchild:
                    parent.lchild = node
                else:
                    parent.rchild = node
            stack.append(len(tree) - 1)

        # Now tree[0] is the root of the tree
        yield tree[0]


# Read each tree and perform traversals
for root in build_tree():
    print("".join(root.preorder_traversal()))
    print("".join(root.postorder_traversal()))
    print("".join(root.inorder_traversal()))
    print()
```


**#4.有些dfs爆栈可以考虑二分**（OJ04135）

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

#注意lo=mid+1但是hi=mid
```

**#5.dic.items()返回键值对，dic只有键**

**#6.list直接赋值的时候是赋了一个引用**

**#7.关于zip**

当你有两个或更多个可迭代对象（例如列表、元组或其他序列）时，`zip` 函数可以将它们逐个元素地配对。它返回一个迭代器，该迭代器生成元组，每个元组包含来自每个可迭代对象的相应位置的元素。

下面是 `zip` 函数的基本用法示例：

```python
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']

zipped = zip(list1, list2)

for pair in zipped:
    print(pair)
```

这将输出：

```python
(1, 'a')
(2, 'b')
(3, 'c')
```

在代码中，`zip(list1, list2)` 将 `list1` 和 `list2` 中的元素逐一配对，生成一个由元组组成的迭代器 `zipped`。然后，我们可以通过迭代 `zipped` 来访问这些配对。每次迭代，我们得到一个包含来自 `list1` 和 `list2` 对应位置的元素的元组。

需要注意的是，如果可迭代对象的长度不同，`zip` 函数将会以最短的长度为准，多余的元素将被忽略。

