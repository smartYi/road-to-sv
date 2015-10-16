# 二叉树的锯齿形层次遍历

给出一棵二叉树，返回其节点值的锯齿形层次遍历（先从左往右，下一层再从右往左，层与层之间交替进行）

样例

    给出一棵二叉树 {3,9,20,#,#,15,7},
        3
       / \
      9  20
        /  \
       15   7

    返回其锯齿形的层次遍历为：
    [
      [3],
      [20,9],
      [15,7]
    ]

## 题解

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
     * @return: A list of lists of integer include
     *          the zigzag level order traversal of its nodes' values
     */
    public ArrayList<ArrayList<Integer>> zigzagLevelOrder(TreeNode root) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        if(root == null) return res;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(root);
        boolean left = true;
        while(!stack.isEmpty()){
            int size = stack.size();
            ArrayList<Integer> sub = new ArrayList<Integer>();
            Stack<TreeNode> nextStack = new Stack<TreeNode>();
            while(!stack.isEmpty()){
                TreeNode node = stack.pop();
                sub.add(node.val);
                if(left){
                    if(node.left != null) nextStack.add(node.left);
                    if(node.right != null) nextStack.add(node.right);
                } else {
                    if(node.right != null) nextStack.add(node.right);
                    if(node.left != null) nextStack.add(node.left);
                }

            }
            res.add(sub);
            left = !left;
            stack = nextStack;
        }

        return res;
    }
}

```