# 3 Sum

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

Elements in a triplet (a,b,c) must be in non-descending order. (ie, a ≤ b ≤ c)

The solution set must not contain duplicate triplets.
    
    For example, given array S = {-1 0 1 2 -1 -4},

    A solution set is:
    (-1, 0, 1)
    (-1, -1, 2)

## Solution

Simplify '3sum' to '2sum' O(n^2). http://en.wikipedia.org/wiki/3SUM

```java
public class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        Arrays.sort(nums);
        int N = nums.length;
        for (int i = 0; i < N-2 && nums[i] <= 0; ++i)
        {
            if (i > 0 && nums[i] == nums[i-1])
                continue; // avoid duplicates
            int twosum = 0 - nums[i];
            int l = i + 1, r = N - 1;
            while (l < r)
            {
                int sum = nums[l] + nums[r];
                if (sum < twosum) ++l;
                else if (sum > twosum) --r;
                else {
                    ArrayList<Integer> tmp = new ArrayList<Integer>();
                    tmp.add(nums[i]); tmp.add(nums[l]); tmp.add(nums[r]);
                    res.add(tmp);
                    ++l; --r;
                    while (l < r && nums[l] == nums[l-1]) ++l;  // avoid duplicates
                    while (l < r && nums[r] == nums[r+1]) --r;  // avoid duplicates
                }
            }
        }
        return res;
    }
}
```