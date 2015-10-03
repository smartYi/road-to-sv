# List of Depth

Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.g., if you have a tree with depth D, you'll have D linked lists)

## Solution

```java
ArrayList<LinkedList<TreeNode>> createLevelLinkedListBFS(TreeNode root){
    ArrayList<LinkedList<TreeNode>> result =
            new ArrayList<LinkedList<TreeNode>>();

    LinkedList<TreeNode> current = new LinkedList<TreeNode>();
    if (root != null){
        current.add(root);
    }

    while (current.size() > 0){
        result.add(current);
        LinkedList<TreeNode> parents = current;
        current = new LinkedList<TreeNode>();
        for (TreeNode parent : parents){
            if (parent.left != null){
                current.add(parent.left);
            }
            if (parent.right != null){
                current.add(parent.right);
            }
        }
    }
}
```