+ 难度：简单

将两个排序链表合并为一个新的排序链表

样例

    给出 1->3->8->11->15->null，2->null， 返回 1->2->3->8->11->15->null。

## 题解

此题为两个链表的合并，合并后的表头节点不一定，故应联想到使用dummy节点。链表节点的插入主要涉及节点next指针值的改变，两个链表的合并操作则涉及到两个节点的next值变化，若每次合并一个节点都要改变两个节点next的值且要对NULL指针做异常处理，势必会异常麻烦。嗯，第一次做这个题时我就是这么想的... 下面看看相对较好的思路。

首先dummy节点还是必须要用到，除了dummy节点外还引入一个lastNode节点充当下一次合并时的头节点。在l1或者l2的某一个节点为空指针NULL时，退出while循环，并将非空链表的头部链接到lastNode->next中。



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
    @param two ListNodes
    @return a ListNode
    """
    def mergeTwoLists(self, l1, l2):
        dummy = ListNode(0)
        lastNode = dummy
        while (l1 != None) and (l2 != None):
            if l1.val < l2.val:
                lastNode.next = l1
                l1 = l1.next
            else:
                lastNode.next = l2
                l2 = l2.next

            lastNode = lastNode.next

        if l1 != None:
            lastNode.next = l1
        else:
            lastNode.next = l2

        return dummy.next

```

源码分析

1. 异常处理，包含在dummy->next中。
2. 引入dummy和lastNode节点，此时lastNode指向的节点为dummy
3. 对非空l1,l2循环处理，将l1/l2的较小者链接到lastNode->next，往后递推lastNode
4. 最后处理l1/l2中某一链表为空退出while循环，将非空链表头链接到lastNode->next
5. 返回dummy->next，即最终的首指针

注意lastNode的递推并不影响dummy->next的值，因为lastNode和dummy是两个不同的指针变量。

链表的合并为常用操作，务必非常熟练，以上的模板非常精炼，有两个地方需要记牢。

1. 循环结束条件中为条件与操作；
2. 最后处理lastNode->next指针的值。
