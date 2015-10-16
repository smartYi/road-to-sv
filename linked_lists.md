# Linked Lists

链表(linked list)是一种常见的线性数据结构。对于单向链表(singly linked list)，每个节点有一个next指针指向后一个节点，还有一个成员变量用以存储数值；对于双向链表(doubly Linked List)，还有一个prev指针指向前一个节点。与数组类似，搜索链表需要O(n)的时间复杂度，但是链表不能通过常数时间(O(1))读取第k个数据。链表的优势在于能够以较高的效率在任意位置插入或删除一个节点。

## 模式识别

+ 当涉及对头节点的操作，我们不妨考虑创建哑节点。
+ 由于题目涉及在链表中寻找特定位置，我们用两个指针变量以不同的速度遍历该链表。
+ 循环遍历链表, 每次只处理当前指针的next 变量，由此实现链表的逆转。


### 链表的基本操作

凡是修改单向链表的操作，只需考虑：

1. 哪个节点的next指针会受到影响，则需要修正该指针；
2. 如果待删除节点是动态开辟的内存空间，则需要释放这部分空间(C/C++)

毕竟，一个链表节点，无非是包含value和next这两个成员变量的数据结构而已。对于双向链表，类似的，则只需额外考虑谁的prev指针会受到影响。

举例如下：

```
void delNode(ListNode *prev) {
  ListNode *curr = prev->next;
  // 删除curr节点只会使prev节点的next受到影响
  prev->next = curr->next;    
  delete curr;    // 清理trash指针
}
```

注：操作链表时务必注意边界条件：curr == head, curr == tail 或者 curr == NULL

### 哑节点

链表操作时利用哑节点(Dummy Node)是一个非常好用的技巧：只要涉及操作head节点，不妨创建dummy node: ListNode *dummy = new ListNode(0); dummy->next = head; 这使得操作head节点与操作其他节点无异。

> Given a linked list and a value x, write a function to reorder this list such that all nodes less than x come before the nodes greater than or equal to x.

解题分析：将链表分成两部分，但这两部分的头节点连是不是NULL都不确定。当涉及对头节点的操作，我们不妨考虑创建哑节点。这样做的好处是：我们总是可以创建两个哑节点，用dummy->next作为真正头节点的引用， 然后在此基础上append，这样就不用处理头节点是否为空的边界条件了。

参考解答：

```
ListNode *reorderList(ListNode *head, int x) {        
    ListNode *newHead = NULL;
    ListNode *aDummy = new ListNode(0);
    ListNode *aCurr = aDummy;
    ListNode *bDummy = new ListNode(0);
    ListNode *bCurr = bDummy;

    while(head) {       
        ListNode *next = head->next;
        head->next = NULL;        
        if( head->val < x ) {
            aCurr->next = head;
            aCurr = head;
        } else {
            bCurr->next = head;
            bCurr = head;
        }         
        head = next;”
    }      

    aCurr->next = bDummy->next;
    newHead  = aDummy->next;

    delete aDummy;
    delete bDummy;

    return newHead;
}
```

### Runner And Chaser

对于寻找链表的某个特定位置的问题，不妨用两个指针变量runner与chaser (ListNode*runner = head, *chaser = head)，以不同的速度遍历该链表，以找到目标位置。在验证算法正确性时，可以用几个简单的小测试用例 (例如长度为4和5的链表)。注意，通常需要分别选取链表长度为奇数和偶数的测试用例以验证算法在一般情况下的正确性。

> Given a linked list, write a function to return the middle point of that list.

解题分析：由于题目涉及在链表中寻找特定位置，我们用两个指针变量以不同的速度遍历该链表。其中，runner以两倍速前进，chaser 一倍速。这样，当runner到达尾节点时，chaser即为所求解。在实际实现的时候，还需要注意链表的越界问题。

参考解答：

```
ListNode *midpoint( ListNode *head) {
    ListNode *chaser = head, *runner = head;
    if (head == NULL)
        return NULL;

    while (runner->next && runner->next->next ) {
        chaser = chaser->next;
        runner = runner->next->next;
    }
    return chaser;
}
```

> Find the kth to last element of a singly linked list. For example, if given a list 1->2->3->4, and k equals to 2, the function should return element 2.

解题分析：与上题类似，由于题目涉及在链表中寻找特定位置，我们用两个指针变量遍历该list。只是在本例中，runner与chaser以相同倍速前进，但runner提前k步出发。

参考解答：

```
ListNode *findkthtoLast(ListNode *head,int k){
    ListNode *runner = head;
    ListNode *chaser = head;

    if (head == NULL || k < 0)
return NULL;
    for(int i = 0; i < k; i++)
        runner = runner->next;
    if( runner == NULL )
        return NULL;
    while( runner->next != NULL ) {
        chaser = chaser->next;
        runner = runner->next;
    }
    return chaser;
}
```

> Given a linked list that may contain a circle, write a function to return the node at the beginning of that circle. Return NULL if such list doesn’t contain a circle.

解题分析：寻找某个特定位置，用runner和chaser。这里的技巧是，如果runner以两倍速度前进，chaser以一倍速前进，在有环的情况下，runner与chaser一定能在某点相遇。相遇后，再让chaser从头节点出发再次追赶runner，此时两者都以一倍速前进，可以证明，第二次相遇的节点即为环的开始位置。

参考解答：

```
ListNode* findLoop(ListNode* head) {
    if (head == NULL)
        return NULL;

    ListNode *slow = head;
    ListNode *fast = head;

    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast)
            break;
    }

    if (fast == NULL || fast->next == NULL)
        return NULL;
  
    slow = head;
    while (slow != fast) {
        slow = slow->next;
        fast = fast->next;
    }
    return slow;
}
```

> Given a list, rotate the list to the right by k places, where k is non-negative. e.g: 1->2->3->4->5, k = 2; after rotation: 4->5->1->2->3

解题分析：假设链表长度为len，新的链表的尾节点是原链表中第 len-k 个节点。则我们的runner指针需要前进len – k – 1 次，到达的位置即为新链表中的尾节点。同时，下一个节点即为新链表的头结点。

参考解答：

```
ListNode *rotateRight(ListNode *head, int k) {
    int len = 1;
    ListNode *cur = head;
    
    if(head == NULL)
        return head;
    
    while(cur->next){
        len++;
        cur = cur->next;
    }    // find last element in the list and calculate list length

    k = k%len;
    if(k == 0)  
        return head;

    cur->next = head;  // tail of the original list should link to the original head

    // use runner pointer to find the new tail
    cur = head;
    for(int i = 0; i < len - k - 1; i++)
        cur = cur->next;
  
    ListNode *const newTail = cur;
    ListNode *const newHead = cur->next;

    newTail ->next = NULL;

    return newHead；
};
```

### 遍历并处理节点

