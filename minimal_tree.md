# Minimal Tree

Given a sorted (increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height.

## Solution

```java
/**
 * The algorithm is as follows:
 * 1. Insert into the tree the middle element of the array
 * 2. Insert (into the left subtree) the left subarray elements.
 * 3. Insert (into the right subtree) the right subarray elements.
 * 4. Recurse.
 */
TreeNode createMinimalBST(int array[]){
    return createMinimalBST(array, 0, array.length - 1);
}

TreeNode createMinimalBST(int arr[], int start, int end){
    if (end < start){
        return null;
    }
    int mid = (start + end) / 2;
    TreeNode n = new TreeNode(arr[mid]);
    n.left = createMinimalBST(arr, start, mid - 1);
    n.right = createMinimalBST(arr, mid + 1, end);
    return n;
}
```