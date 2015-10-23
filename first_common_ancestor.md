# First Common Ancestor

Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. Note: This is not necessarily a binary search tree.

## Solution 

```java
/**
 * Solution #1: With Links to Parents
 *
 * We could trace just p's path upwards. At each node on this path, check to
 * see if this node is on the path from q to the root. The first such node
 * will be the first common ancestor.
 */

TreeNode commonAncestor(TreeNode p, TreeNode q){
    if (p == q) return null;

    TreeNOde ancestor = p;
    while (ancestor != null){
        if (isOnPath(ancestor, q)){
            return ancestor;
        }
        ancestor = ancestor.parent;
    }
    return ull;
}

boolean isOnPath(TreeNode ancestor, TreeNode node){
    while (node != ancestor && node != null){
        node = node.parent;
    }
    return node == ancestor;
}


/**
 * Solution #2: With Links to Parents(Better worst case runtime)
 * We can just traverse upwards from p, storing the parent and the sibling
 * node in a variable. (The sibling node is always a child of parent and
 * refers to the newly uncovered subtree.) At each iteration, sibling gets
 * set to the old parent's sibling node and parent gets set to parent.parent.
 */

TreeNode commonAncestor(TreeNode root, TreeNode p, TreeNode q){
    if (!covers(root, p) || !covers(root, q)){
        return null;
    }
    else if (covers(p, q)){
        return p;
    }
    else if (covers(q, p)){
        return q;
    }

    TreeNode sibling = getSibling(p);
    TreeNode parent = p.parent;
    while (!covers(sibling, q)){
        sibling = getSibling(parent);
        parent = parent.parent;
    }
    return parent;
}

boolean covers(TreeNode root, TreeNode p){
    if (root == null) return false;
    if (root == p) return true;
    return covers(root.left, p) || covers(root.right, p);
}

TreeNode getSibling(TreeNode node){
    if (node == null || node.parent == null){
        return null;
    }

    TreeNode parent = node.parent;
    return parent.left == node ? parent.right : parent.left;
}


/**
 * Solution #3: Without Links to Parents
 *
 * You could follow a chain in which p and q are on the same side. That is,
 * if p and q are both on the left of the node, branch left to look for the
 * common ancestor. If they are both on the right, branch right to look for
 * the common ancestor. When p and q are no longer on the same side, you
 * must have found the first common ancestor.
 */

TreeNode commonAncestor(TreeNode root, TreeNode p, TreeNode q){
    if (!covers(root, p) || !covcers(root, q)){
        return null;
    }
    return ancestorHelper(root, p, q);
}

TreeNode ancestorHelper(TreeNode root, TreeNode p, TreeNode q){
    if (root == null){
        return null;
    }
    else if (root == p){
        return p;
    }
    else if (root == q){
        return q;
    }

    boolean pIsOnLeft = covers(root.left, p);
    boolean qIsOnLeft = covers(root.left, q);
    if (pIsOnLeft != qIsOnleft){
        return root;
    }
    TreeNode childSide = pIsOnLeft ? root.left : root.right;
    return ancestorHelper(childSide, p, q);
}

boolean covers(TreeNode root, TreeNode p){
    if (root == null) return false;
    if (root == p) return true;
    return covers(root.left, p) || covers(root.right, p);
}
```