写出一个高效的算法来搜索 m × n矩阵中的值。

这个矩阵具有以下特性：

+ 每行中的整数从左到右是排序的。
+ 每行的第一个数大于上一行的最后一个整数。

样例

考虑下列矩阵：

    [
      [1, 3, 5, 7],
      [10, 11, 16, 20],
      [23, 30, 34, 50]
    ]
    给出 target = 3，返回 true

挑战

    O(log(n) + log(m)) 时间复杂度

## 题解

二分确定在哪一行，然后再二分确定是哪一个

一次二分搜索: 由于矩阵按升序排列，因此可将二维矩阵转换为一维问题。对原始的二分搜索进行适当改变即可(求行和列)。转化为一维后就变成了普通的二分。

两次二分搜索: 先按行再按列进行搜索，即两次二分搜索。时间复杂度相同。

```python
class Solution:
    """
    @param matrix, a list of lists of integers
    @param target, an integer
    @return a boolean, indicate whether matrix contains target
    """
    def searchMatrix(self, matrix, target):
        # write your code here
        if len(matrix) == 0:
            return False

        low, high = 0, len(matrix)*len(matrix[0])-1
        while(low<=high):
            m = low + ((high-low)>>1)
            v = matrix[m / len(matrix[0])][m % len(matrix[0])]
            if v < target:
                low = m + 1
            elif v > target:
                high = m-1
            elif v == target:
                return True
        return False
```