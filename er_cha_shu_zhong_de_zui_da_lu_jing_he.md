# 二叉树中的最大路径和

给出一棵二叉树，寻找一条路径使其路径和最大，路径可以在任一节点中开始和结束（路径和为两个节点之间所在路径上的节点权值之和）

样例

    给出一棵二叉树：
       1
      / \
     2   3

    返回 6

## 题解

思路

1. 最优路径上的节点一定是连续的，不能中断
2. 最优路径中一定包含某个子树的根节点
3. 写一个递归函数，实现计算根节点到任意点的最大路径和，以及穿过根节点的最大路径和，用一个全局变量保存最优解。

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: An integer.
     */
    int maxSum = Integer.MIN_VALUE;
    public int maxPathSum(TreeNode root) {
        _getMaxSum(root);
        return maxSum;
    }

    private int _getMaxSum(TreeNode root){
        if(root == null) return 0;
        int left = _getMaxSum(root.left);
        int right = _getMaxSum(root.right);
        int sum = left > 0 ? left : 0;
        sum += right > 0 ? right : 0;
        sum += root.val;
        maxSum = Math.max(maxSum, sum);
        return Math.max(left, right) + root.val;
    }
}

```