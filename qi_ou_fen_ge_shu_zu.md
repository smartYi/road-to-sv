+ 难度：简单

分割一个整数数组，使得奇数在前偶数在后。

样例

    给定 [1, 2, 3, 4]，返回 [1, 3, 2, 4]。

挑战

    在原数组中完成，不使用额外空间。

## 题解

两个索引，分别指向需要交换的数，然后交换即可

```python
class Solution:
    # @param nums: a list of integers
    # @return: nothing
    def partitionArray(self, nums):
        # write your code here
        left = 0
        right = len(nums) - 1

        while left < right:
            while left <= right and nums[left] % 2 == 1:
                left = left + 1
            while left <= right and nums[right] % 2 == 0:
                right = right - 1

            if left < right:
                nums[left], nums[right] = nums[right], nums[left]
                left = left + 1
                right = right - 1

```