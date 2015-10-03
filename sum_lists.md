# Sum Lists

You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

EXAMPLE

    Input:  (7->1->6) + (5->9->2). That is 617 + 295
    Output: 2->1->9. That is 912

FOLLOW UP

    Suppose the digits are stored in forward order. Repeat the above problem.

EXAMPLE

    Input:  (6->1->7) + (2->9->5). That is 617 + 295
    Output: 9->1->2. That is 912

## Solution

```java
public static Node sumListReverse(Node head1, Node head2){
    Node head = new Node(-1);
    int num1 = 0, count1 = 0;
    int num2 = 0, count2 = 0;
    int result = 0;

    while (head1 != null){
        num1 += head1.data * Math.pow(10,count1);
        count1++;
        head1 = head1.next;
    }

    while (head2 != null){
        num2 += head2.data * Math.pow(10,count2);
        count2++;
        head2 = head2.next;
    }
    result = num1 + num2;

    Node cur = head;

    while(result > 0){
        cur.next = new Node(result % 10);
        cur = cur.next;
        result /= 10;
    }

    return head.next;
}


public static Node sumList(Node head1, Node head2){
    Node head = new Node(-1);
    int num1 = 0, count1 = 0;
    int num2 = 0, count2 = 0;
    int result = 0;

    while (head1 != null){
        num1 = num1 * 10 + head1.data;
        count1++;
        head1 = head1.next;
    }

    while (head2 != null){
        num2 = num2 * 10 + head2.data;
        count2++;
        head2 = head2.next;
    }
    result = num1 + num2;

    Node cur = head;

    while(result > 0){
        Node temp = new Node(result % 10);
        temp.next = cur.next;
        cur.next = temp;
        result /= 10;
    }

    return head.next;
}
```