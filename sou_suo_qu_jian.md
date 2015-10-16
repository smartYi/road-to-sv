# 搜索区间

给定一个包含 n 个整数的排序数组，找出给定目标值 target 的起始和结束位置。

样例

    给出[5, 7, 7, 8, 8, 10]和目标值target=8,
    返回[3, 4]

挑战

    时间复杂度 O(log n)

## 题解

Search for a range 的题目可以拆解为找 first & last position 的题目，即要做两次二分。由上题二分查找可找到满足条件的左边界，因此只需要再将右边界找出即可。注意到在(target == nums[mid]时赋值语句为end = mid，将其改为start = mid即可找到右边界，解毕。

```java
public class Solution {
    /**
     *@param A : an integer sorted array
     *@param target :  an integer to be inserted
     *return : a list of length 2, [index1, index2]
     */
    public ArrayList<Integer> searchRange(ArrayList<Integer> A, int target) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        int start, end, mid;
        result.add(-1);
        result.add(-1);

        if (A == null || A.size() == 0) {
            return result;
        }

        // search for left bound
        start = 0;
        end = A.size() - 1;
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            if (A.get(mid) == target) {
                end = mid; // set end = mid to find the minimum mid
            } else if (A.get(mid) > target) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (A.get(start) == target) {
            result.set(0, start);
        } else if (A.get(end) == target) {
            result.set(0, end);
        } else {
            return result;
        }

        // search for right bound
        start = 0;
        end = A.size() - 1;
        while (start + 1 < end) {
            mid = start + (end - start) / 2;
            if (A.get(mid) == target) {
                start = mid; // set start = mid to find the maximum mid
            } else if (A.get(mid) > target) {
                end = mid;
            } else {
                start = mid;
            }
        }
        if (A.get(end) == target) {
            result.set(1, end);
        } else if (A.get(start) == target) {
            result.set(1, start);
        } else {
            return result;
        }

        return result;
    }
}


```

源码分析

1. 首先对输入做异常处理，数组为空或者长度为0
2. 初始化 start, end, mid三个变量，注意mid的求值方法，可以防止两个整型值相加时溢出
3. 使用迭代而不是递归进行二分查找
4. while终止条件应为start + 1 < end而不是start <= end，start == end时可能出现死循环
5. 先求左边界，迭代终止时先判断A.get(start) == target，再判断A.get(end) == target，因为迭代终止时target必取start或end中的一个，而end又大于start，取左边界即为start.
6. 再求右边界，迭代终止时先判断A.get(end) == target，再判断A.get(start) == target
7. 两次二分查找除了终止条件不同，中间逻辑也不同，即当A.get(mid) == target如果是左边界（first postion），中间逻辑是end = mid；若是右边界（last position），中间逻辑是start = mid
8. 两次二分查找中间勿忘记重置 start, end 的变量值。