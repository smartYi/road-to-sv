# Check Balanced

Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differ by more than one.

## Solution

```java
/**
 * This improved algorithm works by checking the height of each subtree as
 * we recurse down from the root. On each node, we recursively get the
 * heights of the left and right subtrees through the checkHeight method.
 * If the subtree is balanced, then checkHeight will return the actual
 * height of the subtree. If the subtree is not balanced, then checkHeight
 * will return -1. We will immediately break and return -1 from the current
 * call.
 *
 * This code runs in O(N) time and O(H) space, where H is the height of the
 * tree
 */
int checkHeight(TreeNode root){
    if (root == null){
        return 0;
    }

    int leftHeight = checkHeight(root.left);
    if (leftHeight == -1){
        return -1;
    }

    int rightHeight = checkHeight(root.right);
    if (rightHeight == -1){
        return -1;
    }

    int heightDiff = leftHeight - rightHeight;
    if (Math.abs(heightDiff) > 1){
        return -1;
    }
    else
    {
        return Math.max(leftHeight, rightHeight) + 1;
    }
}

boolean isBalanced(TreeNode root){
    if (checkHeight(root) == -1){
        return false;
    }
    else{
        return true;
    }
}
```