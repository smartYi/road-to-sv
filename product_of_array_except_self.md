# Product of Array Except Self

Given an array of n integers where n > 1, nums, return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Solve it without division and in O(n).

For example, given [1,2,3,4], return [24,12,8,6].

Follow up:

Could you solve it with constant space complexity? (Note: The output array does not count as extra space for the purpose of space complexity analysis.)

## Solution

1. Space O(n)
2. Space O(1)

```java
public class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];
 
        int[] t1 = new int[nums.length];
        int[] t2 = new int[nums.length];
     
        t1[0]=1;
        t2[nums.length-1]=1;
     
        //scan from left to right
        for(int i=0; i<nums.length-1; i++){
            t1[i+1] = nums[i] * t1[i];
        }
     
        //scan from right to left
        for(int i=nums.length-1; i>0; i--){
            t2[i-1] = t2[i] * nums[i];
        }
     
        //multiply
        for(int i=0; i<nums.length; i++){
            result[i] = t1[i] * t2[i];
        }
     
        return result;
    }
    
    public int[] productExceptSelf_2(int[] nums) {
        int[] result = new int[nums.length];
        result[result.length-1] = 1;
     
        for(int i=nums.length-2; i>=0; i--) {
            result[i] = result[i+1] * nums[i+1];
        }
     
        int left = 1;
        for(int i=0; i<nums.length; i++) {
            result[i] *= left;
            left *= nums[i];
        }
     
        return result;
    }
}
```