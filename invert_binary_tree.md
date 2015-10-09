# Invert Binary Tree

Invert a binary tree.

         4
       /   \
      2     7
     / \   / \
    1   3 6   9

to

         4
       /   \
      7     2
     / \   / \
    9   6 3   1

## Solution

1. Recursive
2. Iterative

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode invertTree(TreeNode root) {
        if(root!=null){
            helper(root);
        }
     
        return root;    
    }
     
    public void helper(TreeNode p){
     
        TreeNode temp = p.left;
        p.left = p.right;
        p.right = temp;
     
        if(p.left!=null)
            helper(p.left);
     
        if(p.right!=null)
            helper(p.right);
    }
    
    public TreeNode invertTree_2(TreeNode root) {
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
     
        if(root!=null){
            queue.add(root);
        }
     
        while(!queue.isEmpty()){
            TreeNode p = queue.poll();
            if(p.left!=null)
                queue.add(p.left);
            if(p.right!=null)
                queue.add(p.right);
     
            TreeNode temp = p.left;
            p.left = p.right;
            p.right = temp;
        }
     
        return root;    
    }
}
```