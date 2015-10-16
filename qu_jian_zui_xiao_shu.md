# 区间最小数

给定一个整数数组（下标由 0 到 n-1，其中 n 表示数组的规模），以及一个查询列表。每一个查询列表有两个整数 [start, end]。 对于每个查询，计算出数组中从下标 start 到 end 之间的数的最小值，并返回在结果列表中。

样例

    对于数组 [1,2,7,8,5]， 查询 [(1,2),(0,4),(2,4)]，返回 [2,1,5]

注意

    在做此题前，建议先完成以下三道题 线段树的构造， 线段树的查询 及 线段树的修改。

挑战

    每次查询在O(logN)的时间内完成

## 题解

构造线段树进行选取

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
    public ArrayList<Integer> intervalMinNumber(int[] A, 
                                                ArrayList<Interval> queries) {
        ArrayList<Integer> res = new ArrayList<Integer>(); 
        if (A == null || A.length == 0 || queries == null || queries.size() == 0) {
            return res;
        }
        MinTreeNode root = buildTree(A, 0, A.length - 1);
        for (Interval interval : queries) {
            res.add(getVal(root, interval.start, interval.end));
        }
        return res;
    }
     
    private int getVal(MinTreeNode root, int from, int to) {
        if (root == null || root.end < from || root.start > to) {
            return Integer.MAX_VALUE;
        }
        if (root.start == root.end || (from <= root.start && to >= root.end)) {
            return root.min;
        }
        return Math.min(getVal(root.left, from, to), getVal(root.right, from, to));
    }
     
    private MinTreeNode buildTree(int[] A, int from, int to) {
        if (from > to) {
            return null;
        }
        if (from == to) {
            return new MinTreeNode(from, from, A[from]);
        }
        MinTreeNode root = new MinTreeNode(from, to);
        root.left = buildTree(A, from, (from + to) / 2);
        root.right = buildTree(A, (from + to) / 2 + 1, to);
        if (root.left == null) {
            root.min = root.right.min;
        } else if (root.right == null) {
            root.min = root.left.min;
        } else {
            root.min = Math.min(root.left.min, root.right.min);
        }
        return root;
    }
     
    class MinTreeNode {
        int start;
        int end;
        int min;
        MinTreeNode left;
        MinTreeNode right;
        public MinTreeNode(int start, int end) {
            this.start = start;
            this.end = end;
        }
        public MinTreeNode(int start, int end, int min) {
            this(start, end);
            this.min = min;
        }
    }
}
```