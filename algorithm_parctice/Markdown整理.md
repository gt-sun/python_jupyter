[TOC]

# 算法

## 排序算法

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


