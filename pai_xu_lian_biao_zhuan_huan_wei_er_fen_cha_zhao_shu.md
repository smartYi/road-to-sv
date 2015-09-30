+ 难度：中等

给出一个所有元素以升序排序的单链表，将它转换成一棵高度平衡的二分查找树

## 题解

快慢指针找到中间，属于暴力法

```java
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: a tree node
     */
    public TreeNode sortedListToBST(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;

        ListNode pre = head;

        if (head == null) {
            return null;
        }

        TreeNode root = null;
        if (head.next == null) {
            root = new TreeNode(head.val);
            root.left = null;
            root.right = null;
            return root;
        }

        // get the middle node.
        while (fast != null && fast.next != null) {
            fast = fast.next.next;

            // record the node before the SLOW.
            pre = slow;
            slow = slow.next;
        }

        // cut the list to two parts.
        pre.next = null;
        TreeNode left = sortedListToBST(head);
        TreeNode right = sortedListToBST(slow.next);

        root = new TreeNode(slow.val);
        root.left = left;
        root.right = right;

        return root;
    }
}


```