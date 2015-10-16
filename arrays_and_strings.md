# Hash Tables

A hash table is ad data structure that maps keys to values for highly efficient lookup.

If the number of collisions is very high, the worst case runtime is O(N), where N is the number of keys. However, we generally assume a good implementation that keeps collisions to a minimum, in which case the **lookup time is O(1)**.

可以用一个数组，数组的每个元素是一个链表来实现。