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

1. 从数列中挑出一个元素作为基准数。
2. 分区过程，将比基准数大的放到右边，小于或等于它的数都放到左边。
3. 再对左右区间递归执行第二步，直至各区间只有一个数。

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

- 代码2

```python
def quick_sort(ary):
    return qsort(ary,0,len(ary)-1)

def qsort(ary,left,right):
    #快排函数，ary为待排序数组，left为待排序的左边界，right为右边界
    if left >= right : return ary
    key = ary[left]     #取最左边的为基准数
    lp = left           #左指针
    rp = right          #右指针
    while lp < rp :
        while ary[rp] >= key and lp < rp :
            rp -= 1
        while ary[lp] <= key and lp < rp :
            lp += 1
        ary[lp],ary[rp] = ary[rp],ary[lp]
    ary[left],ary[lp] = ary[lp],ary[left] #这里右边不用写，因为左边和右边的数组下一次递归时就是一个完整的ary
    qsort(ary,left,lp-1)
    qsort(ary,rp+1,right)
    return ary
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

### 堆排序 HeapSort

堆排序在 top K 问题中使用比较频繁。堆排序是采用二叉堆的数据结构来实现的，虽然实质上还是一维数组。二叉堆是一个近似完全二叉树 。

二叉堆具有以下性质：

1. 父节点的键值总是大于或等于（小于或等于）任何一个子节点的键值。
2. 每个节点的左右子树都是一个二叉堆（都是最大堆或最小堆）。

#### 思路

1. 构造最大堆（Build_Max_Heap）：若数组下标范围为 0~n，考虑到单独一个元素是大根堆，则从下标n/2开始的元素均为大根堆。于是只要从n/2-1开始，向前依次构造大根堆，这样就能保证，构造到某个节点时，它的左右子树都已经是大根堆。
2. 堆排序（HeapSort）：由于堆是用数组模拟的。得到一个大根堆后，数组内部并不是有序的。因此需要将堆化数组有序化。思想是移除根节点，并做最大堆调整的递归运算。第一次将heap[0]与heap[n-1]交换，再对heap[0...n-2]做最大堆调整。第二次将heap[0]与heap[n-2]交换，再对heap[0...n-3]做最大堆调整。重复该操作直至heap[0]和heap[1]交换。由于每次都是将最大的数并入到后面的有序区间，故操作完后整个数组就是有序的了。
3. 最大堆调整（Max_Heapify）：该方法是提供给上述两个过程调用的。目的是将堆的末端子节点作调整，使得子节点永远小于父节点 。

动画演示：
![image](http://wuchong.me/img/Heapsort-example.gif)

#### 代码

```python
def heap_sort(ary) :
    n = len(ary)
    first = int(n/2-1)       #最后一个非叶子节点
    for start in range(first,-1,-1) :     #构造大根堆
        max_heapify(ary,start,n-1)
    for end in range(n-1,0,-1):           #堆排，将大根堆转换成有序数组
        ary[end],ary[0] = ary[0],ary[end]
        max_heapify(ary,0,end-1)
    return ary

#最大堆调整：将堆的末端子节点作调整，使得子节点永远小于父节点
#start为当前需要调整最大堆的位置，end为调整边界
def max_heapify(ary,start,end):
    root = start
    while True :
        child = root*2 +1               #调整节点的子节点
        if child > end : break
        if child+1 <= end and ary[child] < ary[child+1] :
            child = child+1             #取较大的子节点
        if ary[root] < ary[child] :     #较大的子节点成为父节点
            ary[root],ary[child] = ary[child],ary[root]     #交换
            root = child
        else :
            break
```

### 时间复杂度表

![image](http://ww1.sinaimg.cn/large/81b78497jw1emncvtdf1qj20u10afn0r.jpg)

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


