+ 难度：简单

给定一个排序链表，删除所有重复的元素每个元素只留下一个。

样例

    给出1->1->2->null，返回 1->2->null
    给出1->1->2->3->3->null，返回 1->2->3->null

## 题解

遍历之，遇到当前节点和下一节点的值相同时，删除下一节点，并将当前节点next值指向下一个节点的next, 当前节点首先保持不变，直到相邻节点的值不等时才移动到下一节点。


```python
"""
Definition of ListNode
class ListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
"""
class Solution:
    """
    @param head: A ListNode
    @return: A ListNode
    """
    def deleteDuplicates(self, head):
        # write your code here
        if head is None:
            return None

        node = head
        while node.next is not None:
            if node.val == node.next.val:
                node.next = node.next.next
            else:
                node = node.next

        return head
```