# 搜索旋转排序数组 II

跟进“搜索旋转排序数组”，假如有重复元素又将如何？

是否会影响运行时间复杂度？如何影响？为何会影响？

写出一个函数判断给定的目标值是否出现在数组中。

样例

    给出[3,4,4,5,7,0,1,2]和target=4，返回 true

## 题解

仔细分析此题和之前一题的不同之处，前一题我们利用A[start] < A[mid]这一关键信息，而在此题中由于有重复元素的存在，在A[start] == A[mid]时无法确定有序数组，此时只能依次递增start/递减end以缩小搜索范围，时间复杂度最差变为O(n)。

在A[start] == A[mid]时递增start序号即可。

```java
public class Solution {
    /**
     * param A : an integer ratated sorted array and duplicates are allowed
     * param target :  an integer to be search
     * return : a boolean
     */
    public boolean search(int[] A, int target) {
        if(A==null || A.length==0) {
            return false;
        }
        int l = 0;
        int r = A.length-1;

        while(l <= r) {
            int m = (l + r) / 2;
            if (target == A[m]) {
                return true;
            }
            if (A[l] < A[m]) {
                // situation 1, numbers between start and mid are sorted
                if(target >= A[l] && target < A[m]) {
                    r = m - 1;
                }
                else {
                    l =m + 1;
                }
            }
            else if (A[l] > A[m]) {
                // situation 2, numbers between mid and end are sorted
                if (target > A[m] && target <= A[r]) {
                    l = m + 1;
                }
                else {
                    r = m - 1;
                }
            }
            else {
                l++;
            }
        }
        return false;
    }
}


```