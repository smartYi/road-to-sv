# Two Sum

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where `index1` must be less than `index2`. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

    Input: numbers={2, 7, 11, 15}, target=9
    Output: index1=1, index2=2

## Solution

Hash table. `O(n)`

Note:  Hash Table solution has been updated.  In case that the two elements are the same, all the indices should be stored in the map.

```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for (int i = 0; i < nums.length; ++i) {
            int b = target - nums[i];
            if (map.get(b) != null) {
                return new int[]{map.get(b),i+1};
            } else map.put(nums[i],i+1);
        }
        return null;
    }
}
```


+ O(n2) runtime, O(1) space – Brute force:

The brute force approach is simple. Loop through each element x and find if there is another value that equals to target – x. As finding another value requires looping through the rest of array, its runtime complexity is O(n2).

+ O(n) runtime, O(n) space – Hash table:

We could reduce the runtime complexity of looking up a value to O(1) using a hash map that maps a value to its index.