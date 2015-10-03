# Random Node

You are implementing a binary tree class from scratch which, in addition to insert, find, and delete, has a method getRandomNode() which returns a random node from the tree. All nodes should be equally likely to be chosen. Design and implement an algorithm for getRandomNode, and explain how you would implement the rest of the methods.

## Solution

```java
// need a extra variable to record the size of the subtree
public class TreeNode{
    private int data;
    public TreeNode left;
    public TreeNode right;
    private int size;

    public TreeNode(int d){
        data = d;
        size = 1;
    }
}

public TreeNode getRandomNode() {
    int leftSize = 0;
    if (left != null){
        leftSize = left.size();
    }

    Random random = new Random();
    int index = random.nextInt(size);
    if (index < leftSize){
        return left.getRandomNode();
    }
    else if(index > leftSize){
        return right.getRandomNode();
    }
    else{
        return this;
    }
}
```