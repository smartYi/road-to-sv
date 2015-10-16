# 主元素 II

给定一个整型数组，找到主元素，它在数组中的出现次数严格大于数组元素个数的三分之一。

样例

    给出数组[1,2,1,2,1,3,3] 返回 1

注意

    数组中只有唯一的主元素

挑战

    要求时间复杂度为O(n)，空间复杂度为O(1)。

## 题解

三三抵销法，但是也有需要注意的地方：

我们对cnt1,cnt2减数时，相当于丢弃了3个数字（当前数字，candidate1, candidate2）。也就是说，每一次丢弃数字，我们是丢弃3个不同的数字。

而Majority number超过了1/3所以它最后一定会留下来。

设定总数为N, majority number次数为m。丢弃的次数是x。则majority 被扔的次数是x

而m > N/3, N - 3x > 0.

3m > N,  N > 3x 所以 3m > 3x, m > x 也就是说 m一定没有被扔完

最坏的情况，Majority number每次都被扔掉了，但它一定会在n1,n2中。

为什么最后要再检查2个数字呢(从头开始统计，而不用剩下的count1, count2)？因为数字的编排可以让majority 数被过度消耗，使其计数反而小于n2，或者等于n2.前面举的例子即是。

另一个例子：

1 1 1 1 2 3 2 3 4 4 4 这个 1就会被消耗过多，最后余下的反而比4少。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: The majority number that occurs more than 1/3
     */
    public int majorityNumber(ArrayList<Integer> nums) {
        int candidate1 = 0;
        int candidate2 = 0;
        int count1 = 0;
        int count2 = 0;
        for (int elem : nums) {
            if (count1 == 0) {
                candidate1 = elem;
            }
            if (count2 == 0 && elem != candidate1) {
                candidate2 = elem;
            }
            if (candidate1 == elem) {
                count1++;
            }
            if (candidate2 == elem) {
                count2++;
            }
            if (candidate1 != elem && candidate2 != elem) {
                count1--;
                count2--;
            }
        }

        count1 = 0;
        count2 = 0;
        for (int elem : nums) {
            if (elem == candidate1) count1++;
            else if (elem == candidate2) count2++;
        }
        return count1>count2? candidate1 : candidate2;
    }
}


```