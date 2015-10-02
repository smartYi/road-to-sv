# 最长上升子序列

给定一个整数序列，找到最长上升子序列（LIS），返回LIS的长度。

样例

    给出[5,4,1,2,3]，这个LIS是[1,2,3]，返回 3
    给出[4,2,4,5,3,7]，这个LIS是[4,4,5,7]，返回 4

挑战

    要求时间复杂度为O(n^2) 或者O(nlogn)

## 题解

A DP Solution:

This is a sequence DP problem, so we define

1. dp[N], whereas dp[i] denotes the length of the LIS including the array element arr[i].
2. Initial state: dp[i] = 1
3. Transit function: for each j, where 0 <= j < i, if (A[i] >= A[j]), dp[i] = Math.max(dp[i], dp[j] + 1);
4. Final state: result = 1, Math.max(result, dp[i]);

```java
public class Solution {
    /**
     * @param nums: The integer array
     * @return: The length of LIS (longest increasing subsequence)
     */
    public int longestIncreasingSubsequence(int[] nums) {
        if (nums == null || nums.length == 0) {
            return 0;
        }

        if (nums.length == 1) {
            return 1;
        }

        int[] dp = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            dp[i] = 1;
        }

        for (int i = 0; i < nums.length; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] >= nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }

        int result = 1;
        for (int i = 0; i < nums.length; i++) {
            result = Math.max(dp[i], result);
        }

        return result;
    }
}


```