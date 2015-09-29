+ 难度：简单

给定一个整数数组，找到一个具有最大和的子数组，返回其最大和。

给出数组[−2,2,−3,4,−1,2,1,−5,3]，符合要求的子数组为[4,−1,2,1]，其最大和为6

注意

    子数组最少包含一个数

挑战

    要求时间复杂度为O(n)

## 题解

每次新加一个数，和原来的最大和比较，如果比最大和大，更新最大和，如果比最大和小，继续加。如果新加一个数之后和小于零，那么 sum 设为 0 重新开始累加。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: A integer indicate the sum of max subarray
     */
    public int maxSubArray(ArrayList<Integer> nums) {
        if (nums.size() == 0) return 0;
        int Sum = 0;
        int maxSum = nums.get(0);

        for (int i = 0; i < nums.size(); i++) {
            Sum += nums.get(i);
            maxSum = Math.max(Sum, maxSum);
            if (Sum < 0) {
                Sum = 0;
            }

        }

        return maxSum;
    }
}

```