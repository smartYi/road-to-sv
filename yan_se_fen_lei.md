+ 难度：中等

给定一个包含红，白，蓝且长度为n的数组，将数组元素进行分类使相同颜色的元素相邻，并按照红、白、蓝的顺序进行排序。

我们可以使用整数0，1和2分别代表红，白，蓝。

注意

    不能使用代码库中的排序函数来解决这个问题

说明

    一个相当直接的解决方案是使用计数排序扫描2遍的算法。
    首先，迭代数组计算0,1,2出现的次数，然后依次用0,1,2出现的次数去覆盖数组。
    你否能想出一个仅使用常数级额外空间复杂度且只扫描遍历一遍数组的算法？

## 题解

By keeping three pointers, red = 0, blue = n - 1, and use white to scan through the whole array, when encountering 0, swap A[red] and A[white], when encountering 2, swap A[blue] and A[white] and move the pointers red, white, blue correspondingly

```java
class Solution {
    /**
     * @param nums: A list of integer which is 0, 1 or 2
     * @return: nothing
     */
    public void sortColors(int[] nums) {
        if(nums == null || nums.length <= 1)
            return;

        int pl = 0;
        int pr = nums.length - 1;
        int i = 0;
        while(i <= pr){
            if(nums[i] == 0){
                swap(nums, pl, i);
                pl++;
                i++;
            }else if(nums[i] == 1){
                i++;
            }else{
                swap(nums, pr, i);
                pr--;
            }
        }
    }

    private void swap(int[] a, int i, int j){
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}

```