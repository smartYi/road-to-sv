# 区间求和 I

给定一个整数数组（下标由 0 到 n-1，其中 n 表示数组的规模），以及一个查询列表。每一个查询列表有两个整数 [start, end] 。 对于每个查询，计算出数组中从下标 start 到 end 之间的数的总和，并返回在结果列表中。

样例

    对于数组 [1,2,7,8,5]，查询[(1,2),(0,4),(2,4)], 返回 [9,23,20]

注意

    在做此题前，建议先完成以下三题：线段树的构造， 线段树的查询，以及线段树的修改。
    
## 题解

用前缀和

```java
/**
 * Definition of Interval:
 * public classs Interval {
 *     int start, end;
 *     Interval(int start, int end) {
 *         this.start = start;
 *         this.end = end;
 *     }
 */
public class Solution {
    /**
     *@param A, queries: Given an integer array and an query list
     *@return: The result list
     */
    public ArrayList<Long> intervalSum(int[] A, 
                                       ArrayList<Interval> queries) {
       ArrayList<Long> res = new ArrayList<Long>();
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return res;
        }
        long[] preSum = new long[A.length];
        long sum = 0;
        for (int i = 0; i < A.length; i++) {
            sum += A[i];
            preSum[i] = sum;
        }
        for (Interval interval : queries) {
            if (interval.start == 0) {
                res.add(preSum[interval.end]);
            } else {
                res.add(preSum[interval.end] - preSum[interval.start - 1]);
            }
        }
        return res;
    }
}


```