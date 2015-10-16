# 加油站

在一条环路上有 N 个加油站，其中第 i 个加油站有汽油gas[i]，并且从第 i 个加油站前往第 i+1个加油站需要消耗汽油cost[i]。

你有一辆油箱容量无限大的汽车，现在要从某一个加油站出发绕环路一周，一开始油箱为空。

求可环绕环路一周时出发的加油站的编号，若不存在环绕一周的方案，则返回-1。

样例

    现在有4个加油站，汽油量gas[i]=[1, 1, 3, 1]，环路旅行时消耗的汽油量cost[i]=[2, 2, 1, 1]。则出发的加油站的编号为2。

注意

    数据保证答案唯一。

挑战

    O(n)时间和O(1)额外空间

## 题解

1. 从i开始，j是当前station的指针，sum += gas[j] – cost[j] （从j站加了油，再算上从i开始走到j剩的油，走到j+1站还能剩下多少油）
2. 如果sum < 0，说明从i开始是不行的。那能不能从i..j中间的某个位置开始呢？既然i出发到i+1是可行的， 又i~j是不可行的， 从而发现i+1~ j是不可行的。
3. 以此类推i+2~j， i+3~j，i+4~j 。。。。等等都是不可行的
4. 所以一旦sum<0，index就赋成j + 1，sum归零。
5. 最后total表示能不能走一圈。

以上做法，其实是贪心的思想：

也就是说brute force的解是 ： 一个一个来考虑， 每一个绕一圈， 但是 实际上 我们发现 i - j不可行 直接index就跳到j+1， 这样周而复始，很快就是绕一圈 就得到解了。

```java
public class Solution {
    /**
     * @param gas: an array of integers
     * @param cost: an array of integers
     * @return: an integer
     */
    public int canCompleteCircuit(int[] gas, int[] cost) {
        if (gas == null || cost == null || gas.length == 0 || cost.length == 0) {
            // Bug 0: should not return false;
            return -1;
        }

        int total = 0;
        int sum = 0;

        int startIndex = 0;

        int len = gas.length;
        for (int i = 0; i < len; i++) {
            int dif = gas[i] - cost[i];
            sum += dif;

            if (sum < 0) {
                // Means that from 0 to this gas station, none of them can be the solution.
                startIndex = i + 1; // Begin from the next station.
                sum = 0; // reset the sum.
            }

            total += dif;
        }

        if (total < 0) {
            return -1;
        }

        return startIndex;
    }
}

```