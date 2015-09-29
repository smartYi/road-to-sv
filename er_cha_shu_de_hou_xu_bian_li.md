+ 难度：简单

给出一棵二叉树，返回其节点值的后序遍历。

样例

    给出二叉树 {1,#,2,3},
    1
     \
      2
     /
    3
    返回 [3,2,1]

挑战

你能使用非递归实现么？

## 题解

若二叉树为空，则不进行任何操作：否则

1. 后序遍历左子树。
2. 后序遍历右子树。
3. 访问根结点。

使用递归写后序遍历那是相当的简单，我们来个不使用递归的迭代版。整体思路仍然为「左右根」，那么怎么才能知道什么时候该访问根节点呢？问题即转化为如何保证左右子节点一定先被访问到？由于入栈之后左右节点已无法区分，因此需要区分左右子节点是否被访问过(加入到最终返回结果中)。除了有左右节点的情况，根节点也可能没有任何子节点，此时也可直接将其值加入到最终返回结果中。

先是递归版本，比较简单粗暴

```python

"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    """
    @param root: The root of binary tree.
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        if root is None:
            return []
        else:
            return self.postorderTraversal(root.left) + self.postorderTraversal(root.right) + [root.val]

```

递归版本

```python
"""
Definition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""


class Solution:
    """
    @param root: The root of binary tree.
    @return: Postorder in ArrayList which contains node values.
    """
    def postorderTraversal(self, root):
        result = []
        if root is None:
            return result
        s = []
        # previously traversed node
        prev = None
        s.append(root)
        while s:
            curr = s[-1]
            noChild = curr.left is None and curr.right is None
            childVisited = (prev is not None) and \
                           (curr.left == prev or curr.right == prev)
            if noChild or childVisited:
                result.append(curr.val)
                s.pop()
                prev = curr
            else:
                if curr.right is not None:
                    s.append(curr.right)
                if curr.left is not None:
                    s.append(curr.left)

        return result
```