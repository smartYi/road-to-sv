# Validate BST

Implement a function to check if a binary tree is a binary search tree.

## Solution

```java
/**
 * In-Order Traversal
 *
 * Do an in-order traversal, copy the elements to an array, and then check
 * to see if the array is sorted. The only problem is that it can't handle
 * duplicate values in the tree properly. If we assume that the tree cannot
 * have duplicate values, then this approach works.
 */

int index = 0;
ArrayList<Integer> order = new ArrayList<Integer>();
void copyBST(TreeNode root){
    if (root == null)
        return;
    copyBST(root.left);
    order.add(root.data);
    copyBST(root.right);
}

boolean checkBST(TreeNode root){
    copyBST(root);
    for (int i = 1; i < order.size(); i++){
        if (order.get(i) <= order.get(i-1))
            return false;
    }
    return true;
}   
```