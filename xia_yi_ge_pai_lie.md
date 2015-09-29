+ 难度：中等

给定一个若干整数的排列，给出按正数大小进行字典序从小到大排序后的下一个排列。

如果没有下一个排列，则输出字典序最小的序列。

样例

    左边是原始排列，右边是对应的下一个排列。
    1,2,3 → 1,3,2
    3,2,1 → 1,2,3
    1,1,5 → 1,5,1

挑战

    不允许使用额外的空间。

## 题解

下面简要介绍一下字典序算法：

1. 从后往前寻找索引满足 a[k] < a[k + 1], 如果此条件不满足，则说明已遍历到最后一个。
2. 从后往前遍历，找到第一个比a[k]大的数a[l], 即a[k] < a[l].
3. 交换a[k]与a[l].
4. 反转k + 1 ~ n之间的元素。

由于这道题中规定对于[4,3,2,1], 输出为[1,2,3,4], 故在第一步稍加处理即可。

```java
public class Solution {
    /**
     * @param nums: an array of integers
     * @return: return nothing (void), do not return anything, modify nums in-place instead
     */
    public void nextPermutation(int[] nums) {
        if (nums == null || nums.length <= 1) {
            return;
        }
        // step1: find nums[i] < nums[i + 1]
        int i = 0;
        for (i = nums.length - 2; i >= 0; i--) {
            if (nums[i] < nums[i + 1]) {
                break;
            } else if (i == 0) {
                // reverse nums if reach maximum
                reverse(nums, 0, nums.length - 1);
                return;
            }
        }
        // step2: find nums[i] < nums[j]
        int j = 0;
        for (j = nums.length - 1; j > i; j--) {
            if (nums[i] < nums[j]) {
                break;
            }
        }
        // step3: swap between nums[i] and nums[j]
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
        // step4: reverse between [i + 1, n - 1]
        reverse(nums, i + 1, nums.length - 1);

        return;
    }

    private void reverse(int[] nums, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
}

```