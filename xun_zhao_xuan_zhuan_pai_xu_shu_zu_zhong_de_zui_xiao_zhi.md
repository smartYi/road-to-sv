+ 难度：中等

假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。

你需要找到其中最小的元素。

你可以假设数组中不存在重复的元素。

样例

    给出[4,5,6,7,0,1,2]  返回 0

## 题解

Understand how to use binary in this problem: compare the mid point with end point.
In this problem, because the sorted line is cut at one point then rotate, so one of the line is absolutely greater than the other line.

Situation 1:

 mid < end :  that means minimum is on the end point's line. Move end to left. end = mid.

Situation 2:

if mid > end: that means there must be a mountain-jump somewhere after mid and before end, which is the minimum point. Now move start to mid.


```java
public class Solution {
    /**
     * @param num: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] num) {
        int start = 0, end = num.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (num[mid] >= num[end]) {
                start = mid;
            } else {
                end = mid;
            }
        }
        if (num[start] < num[end]) {
            return num[start];
        } else {
            return num[end];
        }
    }
}

```