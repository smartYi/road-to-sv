# 三数之和 II

给一个包含n个整数的数组S, 找到和与给定整数target最接近的三元组，返回这三个数的和。

样例

    例如S = [-1, 2, 1, -4] and target = 1.  和最接近1的三元组是 -1 + 2 + 1 = 2.

注意

    只需要返回三元组之和，无需返回三元组本身

## 题解

Similar to 3 SUM.

Starting from the left-element, assume it's the solution. Move the 2 pointers in the right-side-array.

Using the two pointers, trying to find ele1 + ele2 + ele3 = closest number to target.

Note: for comparing closet, use initial value Integer.MAX_VALUE. Be aware of the overflow of integer, use long to handle.

```java
public class Solution {
    /**
     * @param numbers: Give an array numbers of n integer
     * @param target : An integer
     * @return : return the sum of the three integers, the sum closest target.
     */
    public int threeSumClosest(int[] num ,int target) {
        if (num == null || num.length < 3) {
            return Integer.MAX_VALUE;
        }
        Arrays.sort(num);
        long closest = (long) Integer.MAX_VALUE;
        for (int i = 0; i < num.length - 2; i++) {
            int left = i + 1;
            int right = num.length - 1;
            while (left < right) {
                int sum = num[i] + num[left] + num[right];
                if (sum == target) {
                    return sum;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
                closest = Math.abs(sum - target) < Math.abs(closest - target)
                            ? (long) sum : closest;
            }//while
        }//for
        return (int) closest;
    }
}


```