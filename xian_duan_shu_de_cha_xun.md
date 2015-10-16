# 线段树的查询

对于一个有n个数的整数数组，在对应的线段树中, 根节点所代表的区间为0-n-1, 每个节点有一个额外的属性max，值为该节点所代表的数组区间start到end内的最大值。

为SegmentTree设计一个 query 的方法，接受3个参数root, start和end，线段树root所代表的数组中子区间[start, end]内的最大值。

样例

    对于数组 [1, 4, 2, 3], 对应的线段树为：
    
                      [0, 3, max=4]
                     /             \
              [0,1,max=4]        [2,3,max=3]
              /         \        /         \
       [0,0,max=1] [1,1,max=4] [2,2,max=2], [3,3,max=3]
   
    query(root, 1, 1), return 4
    query(root, 1, 2), return 4
    query(root, 2, 3), return 3
    query(root, 0, 2), return 4

注意

    在做此题之前，请先完成 线段树构造 这道题目。
    
## 题解

知道是如何构造的话就非常简单了

```java
/**
 * Definition of SegmentTreeNode:
 * public class SegmentTreeNode {
 *     public int start, end, max;
 *     public SegmentTreeNode left, right;
 *     public SegmentTreeNode(int start, int end, int max) {
 *         this.start = start;
 *         this.end = end;
 *         this.max = max
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     *@param root, start, end: The root of segment tree and 
     *                         an segment / interval
     *@return: The maximum number in the interval [start, end]
     */
    public int query(SegmentTreeNode root, int start, int end) {
       if (root == null || start > root.end || end < root.start) {
            return 0;
        }
        if (root.start == root.end) {
            return root.max;
        }
        int mid = ((root.start + root.end) / 2);
        return Math.max(query(root.left, start, end), query(root.right, start, end));
    }
}

```