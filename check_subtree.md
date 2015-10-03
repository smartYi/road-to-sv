# Check Subtree

T1 and T2 are two very large binary trees, with T1 much bigger than T2. Create an algorithm to determine if T2 is a subtree of T1.

A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.

## Solution

```java
/**
 * We could create a string representing the in-order and pre-order
 * traversals. If T2's pre-order traversal is a substring of T1's pre-order
 * traversal, and T2's in-order traversal is a substring of T1's in-order
 * traversal, then T2 is a subtree of T1.
 *
 * An alternative approach is to search through the larger tree, T1. Each
 * time a node in T1 matches the root of T2, call matchTree. The matchTree
 * method will compare the two subtrees to see if they are identical.
 */

boolean containsTree(TreeNode t1, TreeNode t2){
    if (t2 == null){
        return true;
    }
    return subTree(t1, t2);
}

boolean subTree(TreeNode r1, TreeNode r2){
    if (r1 == null){
        return false;
    }
    else if (r1.data == r2.data && matchTree(r1, r2)){
        return true;
    }
    return (subTree(r1.left, r2) || subTree(r1.right, r2));
}

boolean matchTree(TreeNode r1, TreeNode r2){
    if (r2 == null && r1 == null){
        return true;
    }
    else if (r1 == null || r2 == null){
        return false;
    }
    else if (r1.data != r2.data ){
        return false;
    }
    else {
        return (matchTree(r1.left, r2.left) && matchTree(r1.right, r2.right));
    }
}
```