+ 难度：容易

用 O(1) 时间检测整数 n 是否是 2 的幂次。

样例

    n=4，返回 true;
    n=5，返回 false.

注意

    O(1) 时间复杂度
    
## 题解

注意因为对时间复杂度有要求，所以必须利用位操作来进行判断。注意一些边界情况，例如是负数或者是0,1 之类的，需要分别处理。

2 的幂次表示在二进制中只有最高位是 1 其余都是零，所以利用 `n & n-1 == 0` 来进行检测  


```python
class Solution:
    """
    @param n: An integer
    @return: True or false
    """
    def checkPowerOf2(self, n):
        # write your code here
        if n < 0:
            return False
        elif n == 1:
            return True
        elif n == 0:
            return False
        elif (n & n - 1) == 0:
            return True
        return False
```