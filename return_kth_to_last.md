# Return Kth to Last

Implement an algorithm to find the kth to last element of a singly linked list

## Solution

```java
public static int findKthLast(Node head, int k){
    Node quick = head;
    for (int i = 0; i < k; i++){
        quick = quick.next;
    }
    while(quick != null){
        quick = quick.next;
        head = head.next;
    }
    return head.data;
}
```