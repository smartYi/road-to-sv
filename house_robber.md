# House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

## Solution

dp

```java
public class Solution {
    public int rob(int[] nums) {
        int opt[] = new int[nums.length + 1];
        opt[0] = 0;
        if (nums.length == 0) {
            return opt[0];
        }
        
        opt[1] = nums[0];
        for (int i = 2; i < opt.length; i++) {
            opt[i] = Math.max(opt[i-1], opt[i-2] + nums[i-1]);
        }
        
        return opt[nums.length];
    }
}
```