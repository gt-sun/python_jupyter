- 问题

求最大矩形面积：
![image](http://xuelangzf-github.qiniudn.com/20151103_histogram_area.png)

- 思路

在程序中维护一个栈，栈中元素为直方图中 bar 的下标，然后从头开始扫描每个 bar：

1. 如果当前 bar 的高度大于栈顶 bar 的高度，则将当前 bar 的下标入栈；
2. 否则执行出栈操作，记录弹出下标对应的 bar 的高度，并计算出一个面积，然后用这个面积更新最大面积。

- 代码

```python
def largestRectangleArea(height):
    height.append(0)
    size = len(height)
    no_decrease_stack = [0]  #维护一个堆栈，存放最大最大bar的位置索引
    max_size = height[0]
    i = 0
    while i < size:
        cur_num = height[i]
        if (not no_decrease_stack or
                    cur_num > height[no_decrease_stack[-1]]):
            no_decrease_stack.append(i)
            i += 1
        else:
            index = no_decrease_stack.pop()
            if no_decrease_stack:
                width = i - no_decrease_stack[-1] - 1
            else:
                width = i
            max_size = max(max_size, width * height[index])
    return max_size

x = largestRectangleArea([2,1,5,6,2,3])
print(x)
```

- 再分析

通过不断地对多个直方图的观察，发现面积最大的那个矩形好像都包含至少一个完整的 bar，那么这条规律适用于所有的直方图吗？我们用反证法来证明，假设某个最大矩形中每个竖直块都是所在的 bar 的一小段，那么这个矩形高度增加 1 后仍然是一个合法的矩形，但新的矩形面积更大，与假设矛盾，所以面积最大的矩形必须至少有一个竖直块是整个 bar。

至此我们找到了面积最大矩形的一个特性：各组成竖直块中至少有一个是完整的Bar。有了这条特性，我们再找面积最大的矩形时，就有了一个比较小的范围。具体来说就是针对每个 bar，我们找出包含这个 bar 的面积最大的矩形，然后只需要比较这 N 个矩形即可（N 为 bar 的个数）。

那么问题又来了，如何找出 “包含某个 bar 的面积最大的矩形呢”？对于上面的直方图，包含下标为 4 的 bar 的最大矩形如下图橘黄色部分：
![image](http://xuelangzf-github.qiniudn.com/20151103_histogram_more.png)

简单观察一下，就会发现要找到包含某个 bar 的最大矩形其实很简答，只需要找到高度小于该 bar 的左、右边界即可，上图中分别是下标为 1 的 bar 和下标为 10 的 bar。

至此问题已经变为 “对于给定的 bar，如何确定高度比它小的左、右边界”。其实求左边界和右边界是同样的求法，下面我们考虑求每个 bar 的左边界。最直接的思路是对于每个 bar，扫面其前面所有的 bar，找出最后一个高度小于它的 bar，这样的话时间复杂度明显又是 N^2 ，Holy Shit。

到这里似乎没有路可走了，但如果我们继续绞尽脑汁地去想，可能（或许你对栈理解的很深入，或许是你在一个类似的问题中用到了栈，当然你也可能想到动态规划的思想，那也是可行的）会联想到栈这一数据结构。用栈维护一个高度递增的 bar 的集合，也就是说栈底到栈顶部对应的 bar 的高度越来越大。那么对应一个刚读入的 bar，我们只需要比较它的高度和栈顶对应 bar 的高度，如果当前 bar 比较高，则弹出栈顶元素继续比较，直到栈顶 bar 比它低或者栈为空。之后，将当前 bar 入栈，更新栈内的递增序列。

我们从左到右扫一遍得到每个 bar 对应的左边界，然后从右到左扫一遍得到 bar 的右边界。两次扫描过程中，每个 bar 都只有出栈、入栈操作，所以时间复杂度为 O(N)。通过这样的预处理，即可以 O(N) 的时间复杂度得到每个 bar 的左右边界。之后对于每个 bar 求出包含它的最大面积，也即是由左右边界和 bar 的高度围起来的矩形的面积。再做 N 次比较，即可得出最终的结果。

这里先预处理用两个栈扫描两次得到左、右边界，再计算面积，是按照推导过程一步一步来的。当我们写完程序后，再综合看这个问题，可能会发现其实没必要这样分开来做，我们可以在扫描的同时，维护一个递增的栈，同时在 “合适的” 时候计算面积，然后更新最大面积。具体实现方法就是前面给出的那个神奇的算法，不过现在看来一点也不神奇了，我们已经探索到了它背后的思维历程。


参考链接：
https://leetcode.com/problems/largest-rectangle-in-histogram/

http://chuansong.me/n/996770152239