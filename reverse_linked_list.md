# Reverse Linked List

Reverse a singly linked list.

## Solution

1. recursive
2. iterative

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return head;

        ListNode tail = head.next;
        ListNode reversed = reverseList(head.next);

        tail.next = head;

        head.next = null;

        return reversed;
    }
    
    public ListNode reverseList_2(ListNode head) {
        if(head==null || head.next == null) 
            return head;
     
        ListNode p1 = head;
        ListNode p2 = head.next;
     
        head.next = null;
        while(p1!= null && p2!= null){
            ListNode t = p2.next;
            p2.next = p1;
            p1 = p2;
            if (t!=null){
                p2 = t;
            }else{
                break;
            }
        }
     
        return p2;
    }
}
```