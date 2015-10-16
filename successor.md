# Successor

Write an algorithm to find the "next" node (i.e., in-order successor) of a given node in a binary search tree. You may assume that each node has a link to its parent

## Solution

```java
/**
 * This is not the most algorithmically complex problem but it can be tricky
 * to code perfectly. In a problem like this, it's useful to sketch out
 * pseudocode to carefully outline the different cases.
 */
TreeNode inorderSucc(TreeNode n){
    if (n == null)
        return null;

    // Found right children -> return leftmost node of right subtree
    if (n.right != null){
        return leftMostChild(n.right);
    }
    else{
        TreeNode q = n;
        TreeNode x = q.parent;
        // Go up until we're on left instead of right
        while (x != null && x.left != q){
            q = x;
            x = x.parent;
        }
        return x;
    }
}

TreeNode leftMostChild(TreeNode n){
    if (n == null){
        return null;
    }
    while (n.left != null){
        n = n.left;
    }
    return n;
}   
```