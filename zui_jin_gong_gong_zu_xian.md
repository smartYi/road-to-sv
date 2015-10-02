# 最近公共祖先

给定一棵二叉树，找到两个节点的最近公共父节点(LCA)。

最近公共祖先是两个节点的公共的祖先节点且具有最大深度。

样例

    对于下面这棵二叉树
          4
         / \
        3   7
           / \
          5   6

    LCA(3, 5) = 4
    LCA(5, 6) = 7
    LCA(6, 7) = 7

## 题解

前提条件是查询的两个节点node1和node2是在tree里的，如果没有这个前提条件，我们可以先遍历一遍tree来看这两个node是不是在tree里，由于LCA的算法是O(n)，执行之前确定两个node是否在tree里也是O(n)的，不会影响时间复杂度。这里我们假设这个前提条件是成立的。同样用bottom-up的方法，return的策略如下：

+ 如果curr是null，return null
+ 如果curr等于node1或者node2，return curr。这时候有两种情况，curr就是LCA或者LCA是其他。，如果curr就是LCA那么它会被一直return上去，如果不是的话，找到其他的会return真正的LCA
+ 如果left和right return的都不是null，说明curr就是LCA，return curr, curr会一直被return上去
+ 如果left和right return的都是null，return null
+ 如果left或者right不等于null，return 不是null的那一个，这是为了保证可能是结果的node可以被传上去

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
     * @param root: The root of the binary search tree.
     * @param A and B: two nodes in a Binary.
     * @return: Return the least common ancestor(LCA) of the two nodes.
     */
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        if (root == null)
            return null;
        if (root == A || root == B)
            return root;
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);
        if (left != null && right != null)
            return root;
        else
            return left == null? right: left;
    }
}

```