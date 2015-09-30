+ 难度：中等

给定一个链表，旋转链表，使得每个节点向右移动k个位置，其中k是一个非负数

样例

    给出链表1->2->3->4->5->null和k=2
    返回4->5->1->2->3->null

## 题解

根据题意找到最后的结果然后进行变换，快慢指针

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param head: the List
     * @param k: rotate to the right k places
     * @return: the list after rotation
     */
    public ListNode rotateRight(ListNode head, int k) {
        int len = 0;
        ListNode run = head;
        if(head == null || head.next == null) return head;
        while(run != null){
            len++;
            run = run.next;
        }

        k = k % len;
        int m = len - k;

        ListNode nextHead = head;
        ListNode pre = null;

        while(m-- > 0){
            pre = nextHead;
            nextHead = nextHead.next;
        }
        pre.next = null;
        run = nextHead;
        while(run.next != null){
            run = run.next;
        }

        run.next = head;
        return nextHead;
    }
}

```