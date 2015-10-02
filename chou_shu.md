# 丑数

设计一个算法，找出只含素因子3，5，7 的第 k 大的数。

符合条件的数如：3，5，7，9，15......

样例

    如果k=4， 返回 9

挑战

    要求时间复杂度为O(nlogn)或者O(n)

## 题解

DP method O(k)

For 3, 5 and 7, multiply each one by 1 (default ugly number) at first, then find which prime number yields the smallest product and update its multiplier to the next ugly number. In this manner, accumulatively multiply each one with the next smallest ugly number.

We cannot use if else for checking which prime number (3, 5, or 7) yields the smallest product as there may be duplicates. (for e.g., 3 x 5 == 5 x 3)

```java
class Solution {
    /**
     * @param k: The number k.
     * @return: The kth prime number as description.
     */
    public long kthPrimeNumber(int k) {
        long[] uglyNumbers = new long[k + 1];
        int indexFor3 = 0, indexFor5 = 0, indexFor7 = 0; //multiplier index
        uglyNumbers[0] = 1;
        for (int i = 1; i <= k; i++) {
            uglyNumbers[i] = Math.min(Math.min(3 * uglyNumbers[indexFor3], 5 * uglyNumbers[indexFor5]), 7 * uglyNumbers[indexFor7]);
            if (uglyNumbers[i] == 3 * uglyNumbers[indexFor3]) {
                indexFor3++;
            }
            if (uglyNumbers[i] == 5 * uglyNumbers[indexFor5]) {
                indexFor5++;
            }
            if (uglyNumbers[i] == 7 * uglyNumbers[indexFor7]) {
                indexFor7++;
            }
        }
        return uglyNumbers[k];
    }
};


```