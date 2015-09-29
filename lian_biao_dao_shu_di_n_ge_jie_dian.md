找到单链表倒数第n个节点，保证链表中节点的最少数量为n。

样例

    给出链表 3->2->1->5->null和n = 2，返回倒数第二个节点的值1.

## 题解

快慢指针，快指针先走 n 步即可

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
    @return: Nth to last node of a singly linked list.
    """
    def nthToLast(self, head, n):
        # write your code here
        fast = head
        slow = head
        for i in range(n):
            fast = fast.next
        while fast != None:
            slow = slow.next
            fast = fast.next
        return slow

```