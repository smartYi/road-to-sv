# Palindrome

Implement a function to check if a linked list is a palindrome

## Solution

```java
public static boolean isPalindrome(Node head, int i){
    Node reverse = reverseLinkedList(head);
    Node oriHead = linkedList.createLinkedList(numberGroup[i]);

    return linkedList.isEqual(oriHead, reverse);
}

public static Node reverseLinkedList(Node head){
    linkedList.printLinkedList(head);
    System.out.println();
    Node dummy = new Node(-1);
    dummy.next = head;
    Node temp = head;
    head = head.next;
    temp.next = null;

    while(head != null) {
        dummy.next = head;
        Node next = head.next;
        head.next = temp;
        temp = head;
        head = next;
    }
    linkedList.printLinkedList(dummy.next);
    System.out.println();
    return dummy.next;
}
```