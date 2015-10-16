+ 难度：简单

翻转一个链表

样例

    给出一个链表1->2->3->null，这个翻转后的链表为3->2->1->null

挑战

    在原地一次翻转完成

## 题解

在纸上画清晰即可，注意需要用一个临时变量保存节点，最多同时需要三个节点的信息


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
    @param head: The first node of the linked list.
    @return: You should return the head of the reversed linked list.
                  Reverse it in-place.
    """
    def reverse(self, head):
        # write your code here
        if head is None:
            return None
        fast = head.next
        head.next = None
        while fast != None:
            next = fast.next
            fast.next = head
            head = fast
            fast = next
        return head

```