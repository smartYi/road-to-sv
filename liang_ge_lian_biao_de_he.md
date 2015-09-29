+ 难度：简单

你有两个用链表代表的整数，其中每个节点包含一个数字。数字存储按照在原来整数中相反的顺序，使得第一个数字位于链表的开头。写出一个函数将两个整数相加，用链表形式返回和。

样例

给出两个链表 `3->1->5->null` 和 `5->9->2->null` ，返回 8->0->8->null

## 题解

注意进位，按照规则加即可。这里有一个技巧，因为两个数字相加的和肯定不超过二十，也就是进位的可能只有 1 或者 0，所以就不需要用 `ans / 10` 这种开销较大的运算方式，直接判断是否大于等于 10 来确定进位的数字即可，这样时间可以从 512ms 缩短至 345 ms



```python
"""
Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None
"""
class Solution:
    # @param l1: the first list
    # @param l2: the second list
    # @return: the sum list of l1 and l2
    def addLists(self, l1, l2):
        # write your code here
        if l1 == None and l2 != None:
            return l2
        elif l1 != None and l2 == None:
            return l1
        elif l1 == None and l2 == None:
            return None

        head = ListNode(0)
        prehead = head
        addit = 0

        while l1 != None and l2 != None:
            head.next = ListNode((l1.val + l2.val + addit) % 10)
            if l1.val + l2.val + addit >= 10:
                addit = 1
            else:
                addit = 0
            l1 = l1.next
            l2 = l2.next
            head = head.next

        if l1 != None:
            while l1 != None:
                head.next = ListNode((l1.val + addit) % 10)
                if l1.val + addit >= 10:
                    addit = 1
                else:
                    addit = 0
                l1 = l1.next
                head = head.next
        else:
            while l2 != None:
                head.next = ListNode((l2.val + addit) % 10)
                if l2.val + addit >= 10:
                    addit = 1
                else:
                    addit = 0
                l2 = l2.next
                head = head.next

        if l1 == None and l2 == None and addit > 0:
            head.next = ListNode(addit)

        return prehead.next

```