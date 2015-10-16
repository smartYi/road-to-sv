+ 难度：简单

计算在一个 32 位的整数的二进制表式中有多少个 1.

样例

    给定 32 (100000)，返回 1
    给定 5 (101)，返回 2
    给定 1023 (111111111)，返回 9

## 题解

直接一直除2，看看有多少个1即可



```python
class Solution:
    # @param num: an integer
    # @return: an integer, the number of ones in num
    def countOnes(self, num):
        count = 0
        while num > 0:
            if num % 2 != 0:
                count = count + 1
                num = (num-1) / 2
            else:
                num = num / 2
        return count
```