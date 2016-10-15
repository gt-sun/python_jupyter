[TOC]

# 算法

## 排序算法

### 选择排序

#### 思路

1. 在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
2. 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
3. 以此类推，直到所有元素均排序完毕。

#### 代码

```python
def select_sort(ary):
    n = len(ary)
    for i in range(n):
        min = i #最小值的索引
        for j in range(i+1, n):
            if ary[j] < ary[min]:
                min = j
        ary[min],ary[i] = ary[i],ary[min]
    return ary

```

### 冒泡排序

#### 思路


1. 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
2. 对第 0 个到第 n-1 个数据做同样的工作。这时，最大的数就 “浮” 到了数组最后的位置上。
3. 针对所有的元素重复以上的步骤，除了最后一个。
4. 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。


#### 代码

```python
def bubble_sort(x):
    n = len(x)
    for i in range(n):
        for j in range(1, n-i): #这里从1开始，因为下面是j-1
            if x[j-1] > x[j]:
                x[j-1],x[j] = x[j],x[j-1]
    return x
```

优化 1：某一趟遍历如果没有数据交换，则说明已经排好序了，因此不用再进行迭代了。
用一个标记记录这个状态即可。

```python
def bubble_sort(x):
    n = len(x)
    for i in range(n):
        flag = 1
        for j in range(1, n-i): #这里从1开始，因为下面是j-1
            if x[j-1] > x[j]:
                x[j-1],x[j] = x[j],x[j-1]
            flag = 0
        if flag:
            break
    return x
```

优化2： 录某次遍历时最后发生数据交换的位置，这个位置之后的数据显然已经有序了。因此通过记录最后发生数据交换的位置就可以确定下次循环的范围了。

```python
def bubble_sort(ary):
    n = len(ary)
    k = n                           #k 为循环的范围，初始值 n
    for i in range(n):
        flag = 1
        for j in range(1,k):        #只遍历到最后交换的位置即可
             if  ary[j-1] > ary[j] :
                ary[j-1],ary[j] = ary[j],ary[j-1]
                k = j               #记录最后交换的位置
                flag = 0
        if flag :
            break
    return ary
```

### 快速排序

#### 思路

首先选一个值，将其他值与之比较，小的放左边大的放右边；然后递归排序左边和右边的。

#### 代码

- 代码1

```python
def qsort(x):
    if len(x) <= 1:
        return x
    else:
        p = x[0]
        return qsort([i for in x[1:] if i < p] + [p] + [i for i in x[1:] if i >= p])
```

### 插入排序

#### 思路

想象成扑克牌，手里的牌是排好序的（列表左边），桌上的牌是待排序的（列表右边）。

#### 代码

- 代码1 使用2个for循环

```python
def insert_sort(x):
    n = len(x)
    for i in range(1, n):
        for j in range(i, 0, -1):
            if x[j-1] > x[j] and j > 0:
                x[j],x[j-1] = x[j-1],x[j]
    return x

```

- 代码2 使用for-while循环

```python
def insert_sort(seq):
    for i in range(len(seq)):   
        j = i
        if seq[j-1] == seq[j]: continue
        while j > 0 and seq[j-1] > seq[j]:
            seq[j],seq[j-1] = seq[j-1],seq[j]
            j -= 1
    return seq
```


## 查找算法

### 二分查找

#### 思路

目标t在列表x中，那么t肯定满足x[l] <= t <= x[u]，（l=0，u=n-1，n = len(x)）。当l == u时，x[l] == t == x[u]，就能找到t。

#### 代码

- 代码1

```python
def foo(x, t):
    n = len(x)
    l = -1
    u = n

    if l > u:
        return -1  #不存在
    while l + 1 != u:
        m = (l + u) // 2
        if x[m] < t:
            l = m
        else:
            u = m
    p = u  #p为t的位置索引
    if p > n or x[p] != t:
        p = -1
    return p
```


