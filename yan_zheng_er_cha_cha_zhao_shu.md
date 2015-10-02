# 验证二叉查找树

给定一个二叉树，判断它是否是合法的二叉查找树(BST)

一棵BST定义为：

1. 节点的左子树中的值要严格小于该节点的值。
2. 节点的右子树中的值要严格大于该节点的值。
3. 左右子树也必须是二叉查找树。

样例

    一个例子：
       1
      / \
     2   3
        /
       4
        \
         5

上述这棵二叉树序列化为"{1,2,3,#,#,4,#,#,5}".

## 题解

用中序遍历的方法遍历BST，BST的性质是遍历后数组是有序的。根据这一点我们只需要中序遍历这棵树，然后保存前驱结点，每次检测是否满足递增关系即可。注意以下代码我么用一个一个变量的数组去保存前驱结点，原因是java没有传引用的概念，如果传入一个变量，它是按值传递的，所以是一个备份的变量，改变它的值并不能影响它在函数外部的值，算是java中的一个小细节。

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
     * @return: True if the binary tree is BST, or false
     */
    public boolean isValidBST(TreeNode root) {
        ArrayList<Integer> pre = new ArrayList<Integer>();
        pre.add(null);
        return helper(root, pre);
    }
    private boolean helper(TreeNode root, ArrayList<Integer> pre)
    {
        if(root == null)
            return true;
        boolean left = helper(root.left,pre);
        if(pre.get(0)!=null && root.val<=pre.get(0))
            return false;
        pre.set(0,root.val);
        return left && helper(root.right,pre);
    }
}

```