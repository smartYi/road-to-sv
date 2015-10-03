# 线段树的修改

对于一棵 最大线段树, 每个节点包含一个额外的 max 属性，用于存储该节点所代表区间的最大值。

设计一个 modify 的方法，接受三个参数 root、 index 和 value。该方法将 root 为根的线段树中 [start, end] = [index, index] 的节点修改为了新的 value ，并确保在修改后，线段树的每个节点的 max 属性仍然具有正确的值。

样例

    对于线段树:
    
                          [1, 4, max=3]
                        /                \
            [1, 2, max=2]                [3, 4, max=3]
           /              \             /             \
    [1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=3]
    
    如果调用 modify(root, 2, 4), 返回:
    
                          [1, 4, max=4]
                        /                \
            [1, 2, max=4]                [3, 4, max=3]
           /              \             /             \
    [1, 1, max=2], [2, 2, max=4], [3, 3, max=0], [4, 4, max=3]
    
    或 调用 modify(root, 4, 0), 返回:
    
                          [1, 4, max=2]
                        /                \
            [1, 2, max=2]                [3, 4, max=0]
           /              \             /             \
    [1, 1, max=2], [2, 2, max=1], [3, 3, max=0], [4, 4, max=0]

注意

    在做此题前，最好先完成线段树的构造和 线段树查询这两道题目。

挑战

    时间复杂度 O(h) , h 是线段树的高度

## 题解

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
     *@param root, index, value: The root of segment tree and 
     *@ change the node's value with [index, index] to the new given value
     *@return: void
     */
    public void modify(SegmentTreeNode root, int index, int value) {
        if (root == null || index > root.end || index < root.start) {
           return;
       }
       if (root.start == root.end) {
           root.max = value;
           return;
       }
       if (index <= (root.end + root.start) / 2) {
           modify(root.left, index, value);
       } else {
           modify(root.right, index, value);
       }
       root.max = Math.max(root.left.max, root.right.max);
    }
}

```