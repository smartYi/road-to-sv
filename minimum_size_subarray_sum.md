# Minimum Size Subarray Sum

Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array [2,3,1,2,4,3] and s = 7,

the subarray [4,3] has the minimal length under the problem constraint.

## Solution

我们需要定义两个指针left和right，分别记录子数组的左右的边界位置，然后我们让right向右移，直到子数组和大于等于给定值或者right达到数组末尾，此时我们更新最短距离，并且将left像右移一位，然后再sum中减去移去的值，然后重复上面的步骤，直到right到达末尾，且left到达临界位置，即要么到达边界，要么再往右移动，和就会小于给定值。

```java
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int sum = 0;
        int st = 0;
        int c = nums.length + 1;
        for (int i = 0; i < nums.length; i++) {
            sum += nums[i];

            if(sum >= s){
                while(sum - nums[st] >= s) {
                    sum -= nums[st++];
                }

                c = Math.min(c, i - st + 1);
            }

        }

        if(c > nums.length) {
            return 0;
        }

        return c;
    }
}
```