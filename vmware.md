# VMware

从 OA 搞起

## 这一部分主要是 OA

> Removing Duplicate Entries

![](vm1.jpg)

```java
static LinkedListNode removeDuplicates(LinkedListNode list) {
    LinkedListNode cur = list;
    HashSet<Integer> set = new HashSet<Integer>();
    set.add(list.val);
    while (cur.next != null) {
        if (!set.contains(cur.next.val)) {
            set.add(cur.next.val);
            cur = cur.next;
        }
        else {
            cur.next = cur.next.next;
        }
    }
    return list;
}
```