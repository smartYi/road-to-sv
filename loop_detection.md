# Loop Detection

Given a circular linked list, implement an algorithm that returns the node at the beginning of the loop

 DEFINITION

Circular linked list: A (corrupt) linked list in which a node's next pointer points to an earlier node, so as to make a loop in the linked list.

EXAMPLE

    Input:  A->B->C->D->E->C [the same C as earlier]
    Output: C

## Solution

```java
/**
 * 1. Create two pointers, fast and slow
 * 2. Move fast at a rate of 2 steps and slow at a rate of 1 step
 * 3. When they collide, move slow to head. Keep fast where it is
 * 4. Move slow and fast at a rate of one step. Return the new collision
 * point
 */
public Node findBeginning(Node head){
    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null){
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast){
            break;
        }
    }

    if (fast == null || fast.next == null){
        return null;
    }

    slow = head;
    while(slow != fast){
        slow = slow.next;
        fast = fast.next;
    }
    return fast;
}
```