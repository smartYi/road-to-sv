+ 难度：简单

给定一个整型数组，找出主元素，它在数组中的出现次数严格大于数组元素个数的二分之一。

样例

    给出数组[1,1,1,1,2,2,2]，返回 1

挑战

    要求时间复杂度为O(n)，空间复杂度为O(1)

## 题解

逐个标记，一旦遇到不同的，计数减一，否则加一，计数为零时，更换答案。因为一定大于二分之一，所以可以保证最后留下来的就是那个主元素

```python
class Solution:
    """
    @param nums: A list of integers
    @return: The majority number
    """
    def majorityNumber(self, nums):
        count = 0
        ans = 0
        for i in nums:
            if count == 0:
                ans = i
                count = count + 1
            elif count > 0 and ans == i:
                count = count + 1
            elif count > 0 and ans != i:
                count = count - 1

        return ans
```