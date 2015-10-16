+ 难度： 简单

实现 `int sqrt(int x)` 函数，计算并返回 x 的平方根。

样例

    sqrt(3) = 1
    sqrt(4) = 2
    sqrt(5) = 2
    sqrt(10) = 3

挑战

    O(log(x))

## 题解

用枚举的办法会超时以及超过时间限制，所以必须使用二分来进行查找和判断，注意二分的写法

```python
class Solution:
    """
    @param x: An integer
    @return: The sqrt of x
    """
    def sqrt(self, x):
        # write your code here
        low = 0
        high = x
        while low <= high:
            mid = low + (high - low) / 2
            square = mid * mid
            lsquare = (mid-1)*(mid-1)
            hsquare = (mid+1)*(mid+1)

            if square == x:
                return mid
            if lsquare <= x and square > x:
                return mid - 1
            if square < x and hsquare > x:
                return mid
            if square < x:
                low = mid + 1
            else:
                high = mid - 1
```