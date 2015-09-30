+ 难度：中等

假设一个旋转排序的数组其起始位置是未知的（比如0 1 2 4 5 6 7 可能变成是4 5 6 7 0 1 2）。

你需要找到其中最小的元素。

你可以假设数组中不存在重复的元素。

样例

    给出[4,5,6,7,0,1,2]  返回 0

## 题解

1.如何找中间断开的区间（也就是说旋转过）

我们的目的是要找出存在断口的地方。所以我们可以每次求一下mid的值，把mid 跟左边比一下，如果是正常序，就丢掉左边，反之丢掉右边，不断反复直到找到断口。

分析一下：

比如4 5 6 7 0 1 2  从中间断开后，它是由一个有序跟一个无序的序列组成的。

如果left = 0, right = 6,mid = 3, 那么4， 5， 6， 7 是正序， 7， 0， 1， 2是逆序，所以我们要丢掉左边。这样反复操作，直到数列中只有2个数字，就是断开处，这题我们会得到7，0，返回后一个就可以了。

2.特别的情况：

如果发现 A.mid > A.left，表示左边是有序，丢掉左边。

如果发现 A.mid < A.left, 表示无序的状态在左边，丢掉右边

如果A.mid = A.left，说明无法判断。这时我们可以把left++，丢弃一个即可。不必担心丢掉我们的目标值。因为A.left == A.mid，即使丢掉了left,还有mid在嘛！

每次进入循环，我们都要判断A.left < A.right，原因是，前面我们丢弃一些数字时，有可能造成余下的数组是有序的，这时应直接返回A.left! 否则的话 我们可能会丢掉解。

就像以下的例子，在1 10 10中继续判断会丢弃1 10.

举例: 10 1 10 10 如果我们丢弃了最左边的10，则1 10 10 是有序的

```java
public class Solution {
    /**
     * @param num: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] num) {
        if (num == null || num.length == 0) {
            return 0;
        }

        int len = num.length;
        if (len == 1) {
            return num[0];
        } else if (len == 2) {
            return Math.min(num[0], num[1]);
        }

        int left = 0;
        int right = len - 1;

        while (left < right - 1) {
            int mid = left + (right - left) / 2;
            // In this case, the array is sorted.
            // 这一句很重要，因为我们移除一些元素后，可能会使整个数组变得有序...
            if (num[left] < num[right]) {
                return num[left];
            }

            // left side is sorted. CUT the left side.
            if (num[mid] > num[left]) {
                left = mid;
            // left side is unsorted, right side is sorted. CUT the right side.
            } else if (num[mid] < num[left]) {
                right = mid;
            } else {
                left++;
            }
        }

        return Math.min(num[left], num[right]);
    }
}

```