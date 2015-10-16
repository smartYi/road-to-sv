# 平衡二叉树

给定一个二叉树,确定它是高度平衡的。对于这个问题,一棵高度平衡的二叉树的定义是：一棵二叉树中每个节点的两个子树的深度相差不会超过1。

样例

    给出二叉树 A={3,9,20,#,#,15,7}, B={3,#,20,15,7}
    A)  3            B)    3
       / \                  \
      9  20                 20
        /  \                / \
       15   7              15  7

二叉树A是高度平衡的二叉树，但是B不是

## 题解

很锻炼DP/recursive思路的一道题，个人感觉DP/recursive算是比较难写的题目了。这道题解法的巧妙之处在于巧用-1，并且使用临时存储，节省了很多开支。

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
     * @return: True if this Binary tree is Balanced, or false.
     */
    public boolean isBalanced(TreeNode root) {
        if (root == null) return true;
        if (checkBalance(root) != -1) return true;
        else return false;
    }

    public int checkBalance(TreeNode root) {
        if (root == null) return 0;
        int leftHeight = checkBalance(root.left);
        int rightHeight = checkBalance(root.right);
        if (leftHeight == -1 || rightHeight == -1) return -1;
        else if (Math.abs(leftHeight - rightHeight) > 1) return -1;
        return Math.max(leftHeight, rightHeight) + 1;
    }
}

```