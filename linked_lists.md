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

在遍历链表时，注意每次循环内只处理一个或一对节点，否则很容易出现重复处理的问题。

> Reverse the linked list and return the new head.

解题分析：循环遍历链表, 每次只处理当前指针的next 变量，由此实现链表的逆转。

参考解答：

```
ListNode *reverseLinkedList( ListNode *head ) {
    ListNode *prev = NULL;
    ListNode *next = NULL;
    while(head) {
        next = head->next;
        head->next = prev;

        prev = head;
        head = next;
    }
    return prev;
}
```

### 交换节点的问题

如果需要交换两个节点的位置，则对这两个节点的前驱节点，它们的next指针会受到影响；这两个节点本身的next指针，也会受到影响。但是，如下过程总是可以实现交换：

1. 先交换两个前驱节点的next指针的值；
2. 再交换这两个节点的next指针的值。

无论这两个节点的相对位置和绝对位置如何，以上的处理方式总是成立。

> Given a linked list, swap every two adjacent nodes and return its head

解题分析：依照上述交换规则给出解答如下。

参考解答：

```
ListNode *swapPairs(ListNode *head) {
    if(head == NULL)    
        return head;

    ListNode *dummy = new ListNode(0);
    dummy->next = head;
    ListNode *prev = dummy;
    ListNode *node1 = head, *node2 = head->next;
    ListNode *newHead = NULL;

    while(node1 && node1->next) {
        node2 = node1->next;

        // swap the "next" of prev nodes;
        ListNode *tmp1 = prev->next;    // = node1
        prev->next = node1->next;    // = node2
        node1->next = tmp1;    // = node1
    
        // swap the "next" of current nodes;
        ListNode *tmp2 = node1->next;    // = node1
        node1->next = node2->next;
        node2->next = tmp2;    // = node1
    
        prev = node1;
        node1 = prev->next;
    }

    newHead = dummy->next;
    delete dummy;
    return newHead;
}
```

根据comment优化之后，循环代码段可以缩写为：
```
while(node1 && node1->next) {
    node2 = node1->next;
    
    // swap the "next" of prev nodes;
    prev->next = node1->next;    // = node2

    // swap the "next" of current nodes;
    node1->next = node2->next;
    node2->next = node1

    prev = node1;
    node1 = prev->next;
}
```

但应当注意到，就算不优化，代码仍然是有效的，充其量是多用了几个临时变量而已。

### 同时操作两个链表

遇到同时处理两个链表的问题，循环的条件一般可以用 while( list1 && list2 )，当循环跳出后，再处理剩下非空的链表。这相当于：边界情况特殊处理，常规情况常规处理。

> Given two sorted linked lists, write a function to merge these two lists, and return a new list which is sorted.

解题分析：这是一个典型的需要同时处理两个链表的问题，可以先处理常规情况(两个链表都有剩下节点)，再处理边界情况(其中一个链表已经遍历完毕)。在处理常规情况的时候，我们将当前两个链表中较小的那个节点放入新的链表。

参考解答：

```
ListNode *mergeTwoLists(ListNode *l1, ListNode *l2) {
    ListNode *dummy = new ListNode(0);
    ListNode *cur = dummy;
    ListNode *newHead = NULL;

    while(l1&&l2) {
        if(l1->val <= l2->val){
            cur->next = l1;
            l1 = l1->next;
        } else {
            cur->next = l2;
            l2 = l2->next;
        }
        cur = cur->next;
    }

    if(l1)
        cur->next = l1;
    else
        cur->next = l2;

    newHead = dummy->next；
    delete dummy;
    return newHead;
}
```

### 倒序处理

如果对靠前节点的处理必须在靠后节点之后，即类似于倒序访问的问题，可以用递归(recursion)，或者等效地，用栈来解决。

> Traverse the linked list reversely.

解题分析：倒序访问问题本身，我们利用递归处理。

参考解答：

void reversedTraverse(ListNode *head) {
    if (head == NULL)
        return;
    traverse(head->next);
    visit(head);
}

> Given two linked lists, each element of the lists is a integer. Write a function to return a new list, which is the “sum” of the given two lists.
> 
> Part a. Given input (7->1->6) + (5->9->2), output 2->1->9. 
> Part b. Given input (6->1->7) + (2->9->5), output 9->1->2.

解题分析：对于a，靠前节点的解不依赖靠后节点，因此顺序遍历求解即可。对于b，靠前节点的解依赖于靠后节点(进位)，因此必须用递归或栈处理。并且，子问题返回的结果，可以是一个自定义的结构(进位 + sub-list)。当然，对于问题b，也可以通过逆向列表之后再用a的解法求解。同时，注意到该题还是处理两个链表的问题，所以可以先处理常规情况(两个链表都有剩下节点)，再处理边界情况(其中一个链表已经遍历完毕)。

参考解答：

a.

```
ListNode *addList(ListNode *l1, ListNode *l2) {
    int carry = 0;
    ListNode *dummy = new ListNode(0);
    ListNode *curr = dummy;
    ListNode *newHead = NULL;

    while(l1 && l2) {
        int sum = l1->value + l2->value + add_on;
        add_on = sum/10;
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
        l1 = l1->next;
        l2 = l2->next;
    }
    ListNode *rest = l1 ? l1 : l2;
    
    while(rest) {
        int sum = rest->value + add_on;
        carry = sum/10;
        curr->next = new ListNode(sum % 10);
        curr = curr->next;
        rest = rest->next;
    }
    
    if (add_on) 
        curr->next = new ListNode(carry);
    
    newHead = dummy->next;
    delete dummy;
    return newHead;
}
```

b.

```
struct ListWithCarry {
    ListNode *head;
    int Carry;
};

int getLen(ListNode *head) {
    int len = 0;
    while(head) {
        len++;
        head = head->next;
    }
    return len;
}

ListNode *padList(ListNode *head, int diff){
    ListNode *dummy = new ListNode(0);
    ListNode *curr = dummy;
    ListNode *newHead = NULL;

    for(int i = 0; i < diff; i++) {
        curr->next = new ListNode(0);
        curr = curr->next;
    }
    curr->next = head;

    newHead = dummy->next;
    delete dummy;
    return newHead;
}

ListWithCarry addListImpl(ListNode *l1, ListNode *l2) {
    ListWithCarry res;
    res.head = NULL;
    res.Carry = 0;
    if (!l1 && !l2)
        return res;
    int sum = 0;
    
    ListWithCarry subList = addListImpl(l1->next, l2->next);
    
    if(l1)
        sum += l1->value;
    if(l2)
        sum += l2->value;
    sum += subList.Carry;
    
    res.Carry = sum/10;
    res.head->val = sum%10;
    res.head->next = subList.head;
    
    return res;
    ListNode *curr = dummy;
    ListNode *newHead = NULL;

    for(int i = 0; i < diff; i++) {
        curr->next = new ListNode(0);
        curr = curr->next;
    }
    curr->next = head;

    newHead = dummy->next;
    delete dummy;
    return newHead;
}

ListWithCarry addListImpl(ListNode *l1, ListNode *l2) {
    ListWithCarry res;
    res.head = NULL;
    res.Carry = 0;
    if (!l1 && !l2)
        return res;
    int sum = 0;
    
    ListWithCarry subList = addListImpl(l1->next, l2->next);
    
    if(l1)
        sum += l1->value;
    if(l2)
        sum += l2->value;
    sum += subList.Carry;
    
    res.Carry = sum/10;
    res.head->val = sum%10;
    res.head->next = subList.head;
    
    return res;
}

ListNode *addList(ListNode *l1, ListNode *l2) {
    int len1 = getLen(l1);
    int len2 = getLen(l2);
    if (len1 > len2) {
        l2 = padList(l2, len1 - len2);
    } else if (len1 < len2){
        l1 = padList(l1, len2 - len1);
    }
    
    ListWithCarry resStruct = addListImpl(l1, l2);
    ListNode *dummy = new ListNode(0);
    ListNode *newHead = NULL;

    if(resStruct.Carry) {
        dummy->next = new ListNode(resStruct.Carry);
        dummy->next->next = resStruct.head;
    } else {
        dummy->next = resStruct.head;
    }

    newHead = dummy->next;
    delete dummy;
    return newHead;
}
```

## 工具箱

“对于利用链表解决问题，而非解构链表的问题，可以考虑使用标准库。

对C++，

双链表(Doubly linked list)的实现类是std::list<T>.

常用iterator： begin(), end(), rbegin(), rend().

常用函数:

empty(), size(), push_back(T value), pop_back(T value);

erase(iterator pos), insert(iterator pos, T value);

对于Java,

双链表的实现类是 LinkedList<E>

常用函数：

add(E e), add(int index, E element), remove(int index),

addAll(Collection<? Extends E> c),  get(int index),
