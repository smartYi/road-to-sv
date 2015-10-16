# Single Number III

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

For example:

Given nums = [1, 2, 1, 3, 2, 5], return [3, 5].

Note:

1. The order of the result is not important. So in the above example, [5, 3] is also correct.
2. Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?

## Solution

对于一个数组只有一个数出现一次的，其他都出现2次，我们找出这个数很简单。利用异或运算的性质，即两个相同的数异或结果是0，把数组中所有的数都异或一遍，就可以直接得出这个单个的数。 
本题目相当于是对上述的扩展，利用上述的方法，我们很直观的就会想到因为有两个数只出现一次，那么是不是可以把这个数组分成两个部分，每个部分都形如上述问题中的数组，即只有一个是单个的，其余的都是两个的。 
那么主要的问题就是怎么分才能分成这样的数组。

下面来看两个简单的异或的例子： 

    111^100 = 011； 
    010^011 = 001； 

再结合异或的计算方法，即相同的位的异或结果为0，不同的是1。可以很清楚的知道，如果一个位上的结果是1，那么肯定是由一个0和一个1异或得到的。也就是说这一位上必然有一个数是0，另一个是1。刚好就是我们的分组条件，即假设这一位的index = m，那么原来的数组就可以被分成两个部分，一个部分第m位是0，另一部分第m位是1。分完组之后，把两个数组分别异或出来的结果就是所求的结果。 

```java
public class Solution {
    public int[] singleNumber(int[] nums) {
        int[] value = new int[2];
        if (nums.length <= 2) {
            return nums;
        }
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            result ^= nums[i];
        }
        String hex = Long.toBinaryString(result);
        int x = 0;
        for (int i = hex.length() - 1; i >= 0; i--) {
            if (hex.charAt(i) == '1') {
                x = (int)Math.pow(2, hex.length() - 1 - i);
                break;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if ((nums[i] & x) == x) {
                value[0] ^= nums[i];
            }else{
                value[1] ^= nums[i];
            }
        }
        return value;
    }
}
```