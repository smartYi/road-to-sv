# 复制带随机指针的链表

给出一个链表，每个节点包含一个额外增加的随机指针可以指向链表中的任何节点或空的节点。

返回一个深拷贝的链表。

挑战

    可否使用O(1)的空间

## 题解

Using HashMap to store the RandomListNode info.

We can create a hashMap<RandomListNode, RandomListNode>, the Key stores the original ListNode, and the value stores the new RandomListNode.

In the implementation, we only need to iterate the RandomListNode once. and for each original node, we first copy the next pointer. and then copy the Random pointer.

The important is every time when you copy the next or the random pointer, you should check whether the node is/not exist.

```java
/**
 * Definition for singly-linked list with a random pointer.
 * class RandomListNode {
 *     int label;
 *     RandomListNode next, random;
 *     RandomListNode(int x) { this.label = x; }
 * };
 */
public class Solution {
    /**
     * @param head: The head of linked list with a random pointer.
     * @return: A new head of a deep copy of the list.
     */
    public RandomListNode copyRandomList(RandomListNode head) {
        if (null == head) {
       return null;
       }
       Map<RandomListNode, RandomListNode> map = new HashMap<RandomListNode, RandomListNode>();
       RandomListNode dummy = new RandomListNode(-1);
       dummy.next = head;

       RandomListNode copyNode = dummy;
       RandomListNode temp;

       while (head != null) {
           // copy current node
           if (map.containsKey(head)) {
               temp = map.get(head);
           } else {
               temp = new RandomListNode(head.label);
               map.put(head, temp);
           }
           copyNode.next = temp;
           // copy random node
           if (head.random != null) {
               if (map.containsKey(head.random)) {
                   temp.random = map.get(head.random);
               } else {
                   temp.random = new RandomListNode(head.random.label);
                   map.put(head.random, temp.random);
               }
           }
           copyNode = copyNode.next;
           head = head.next;
       }
       return dummy.next;
    }
}

```