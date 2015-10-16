# Sum Root to Leaf Numbers

Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

        1
       / \
      2   3
  
The root-to-leaf path 1->2 represents the number 12.

The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.

## Solution

Recursion (add to sum when reaching the leaf).

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
   public int sumNumbers(TreeNode root) {
        if (root == null) return 0;
        return sumNumbersRe(root,0);
    }
    public int sumNumbersRe(TreeNode root, int last) {
        if (root == null) return 0;
        int res = last * 10 + root.val;
        if (root.left == null && root.right == null) return res;
        if (root.left == null) return sumNumbersRe(root.right, res);
        if (root.right == null) return sumNumbersRe(root.left, res);
        return sumNumbersRe(root.left, res) + sumNumbersRe(root.right, res);
    }
}
```
