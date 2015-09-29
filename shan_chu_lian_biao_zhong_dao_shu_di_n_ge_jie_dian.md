+ 难度：简单

给定一个链表，删除链表中倒数第n个节点，返回链表的头节点。

样例

    给出链表1->2->3->4->5->null和 n = 2.
    删除倒数第二个节点之后，这个链表将变成1->2->3->5->null.

注意

    链表中的节点个数大于等于n

挑战

    O(n)时间复杂度


## 题解

简单题， 使用快慢指针解决此题，需要注意最后删除的是否为头节点。让快指针先走n步，直至快指针走到终点，找到需要删除节点之前的一个节点，改变node->next域即可。


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
    @param head: The first node of linked list.
    @param n: An integer.
    @return: The head of linked list.
    """
    def removeNthFromEnd(self, head, n):
        if head is None or n < 0:
            return None

        preN = head
        tail = head
        # slow fast pointer
        for i in range(n):
            if tail is None:
                return None
            tail = tail.next

        if tail is None:
            return head.next

        while tail.next:
            tail = tail.next
            preN = preN.next

        preN.next = preN.next.next
        return head;
```