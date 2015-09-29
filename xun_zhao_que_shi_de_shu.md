给出一个包含 0 .. N 中 N 个数的序列，找出0 .. N 中没有出现在序列中的那个数。

样例

    N = 4 且序列为 [0, 1, 3] 时，缺失的数为2。

注意

    可以改变序列中数的位置。

挑战

    在数组上原地完成，使用O(1)的额外空间和O(N)的时间。

## 题解

可以根据题目所给的信息利用数学进行计算，利用求和公式得到总和，然后分别减去数组元素就可以知道是少了哪个数。

```java
public class Solution {
    /**
     * @param nums: an array of integers
     * @return: an integer
     */
    public int findMissing(int[] nums) {
        int length = nums.length;
        int sum = length * (length + 1) / 2;

        for (int i = 0; i < length; i++){
            sum -= nums[i];
        }

        return sum;
    }
}
```