+ 难度：简单

给出一棵二叉树,返回其中序遍历

样例

    给出二叉树 {1,#,2,3},
    1
     \
      2
     /
    3
    返回 [1,3,2].

挑战

你能使用非递归算法来实现么?

## 题解

若二叉树为空，则不进行任何操作：否则

1. 中序遍历左子树。
2. 访问根结点。
3. 中序遍历右子树

非递归的话，则需要麻烦很多，注意入栈出栈的顺序和限制规则。我们采用「左根右」的访问顺序主要由如下四步构成：

1. 首先需要一直对左子树迭代并将非空节点入栈
2. 节点指针为空后不再入栈
3. 当前节点为空时进行出栈操作，并访问栈顶节点
4. 将当前指针p用其右子节点替代

步骤2,3,4对应「左根右」的遍历结构，只是此时的步骤2取的左值为空。


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
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        if root is None:
            return []
        else:
            return self.inorderTraversal(root.left) + [root.val] +  self.inorderTraversal(root.right)

```

迭代版本

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
    @return: Inorder in ArrayList which contains node values.
    """
    def inorderTraversal(self, root):
        result = []
        s = []
        while root is not None or s:
            if root is not None:
                s.append(root)
                root = root.left
            else:
                root = s.pop()
                result.append(root.val)
                root = root.right

        return result

```