+ 难度：简单

有一个机器人的位于一个M×N个网格左上角。

机器人每一时刻只能向下或者向右移动一步。机器人试图达到网格的右下角（下图中标记为'Finish'）。

问有多少条不同的路径？

注意

    n和m均不超过100

## 题解

这个和之前语音识别做编辑距离有点类似，本质就是一个矩阵的递推：当前位置的值等于左边和上边的值相加，然后循环处理即可。这里利用二维矩阵只用一次的性质，可以直接利用一维来模拟二维进行累加

```python
class Solution:
    """
    @param n and m: positive integer(1 <= n , m <= 100)
    @return an integer
    """
    def uniquePaths(self, m, n):
        result = [1] * n
        if m == 1:
            return result[-1]
        for i in range(1,m):
            for j in range(1,n):
                result[j] = result[j] + result[j-1]
        return result[-1]
```