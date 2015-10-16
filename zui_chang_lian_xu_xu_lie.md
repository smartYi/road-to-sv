# 最长连续序列

给定一个未排序的整数数组，找出最长连续序列的长度。

样例

    给出数组[100, 4, 200, 1, 3, 2]，这个最长的连续序列是 [1, 2, 3, 4]，返回所求长度 4

说明

    要求你的算法复杂度为O(n)

## 题解

我们可以把所有的数字放在hashset中，来一个数字后，取出HashSet中的某一元素x，找x-1,x-2....x+1,x+2...是否也在set里。

```java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return an integer
     */
    public int longestConsecutive(int[] num) {
        if (num == null) {
            return 0;
        }

        HashSet<Integer> set = new HashSet<Integer>();
        for (int i: num) {
            set.add(i);
        }

        int max = 0;
        for (int i: num) {
            int cnt = 1;
            set.remove(i);

            int tmp = i - 1;
            while (set.contains(tmp)) {
                set.remove(tmp);
                cnt++;
                tmp--;
            }

            tmp = i + 1;
            while (set.contains(tmp)) {
                set.remove(tmp);
                cnt++;
                tmp++;
            }

            max = Math.max(max, cnt);
        }

        return max;
    }
}

```