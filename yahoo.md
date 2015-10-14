# Yahoo

## Phone Screen

一定要注意问清楚细节，要求我做什么


> 语言的基础知识

Python, C++

> 位运算

复习

> java，设计address book。问要实现哪些功能。add, remove, get

TODO

> 找出一个Binary tree 的 deepest node

TODO, BFS

> compress string

TODO

> Valid 一串 id : 假设id从1开始，到N (N 已知)， validate 一下这串id是否是从1到N每个数字都出现，且仅出现了一次。 输入是一个array包含了这些数字，输出是true （valid）或者false （invalid）

我用了类似于bucket sort的方法，见到一个数字就减去1，放到相应的index里面。遇到重复的或者超出数组范围的就返回false，否则继续检查下一个index，直到所有数字通过测试，返回true。

> longgest common sequence of letters

leetcode longgest common prefix, trie-tree

> Product of Array Exclude itself

lintcode, 另一个array来存储从左到右或者是从右到左的连续product

> Array a has ten elements ,array b has nine elements that are from array a, find the missing one, do not use extra space

TODO

> @@@ remove duplicated character from string without changing character order

hashset 做

> find least common ancestor

第二题的tree node还给了parent，所以只要向上找最先出现的公共parent就行就行了，非常简单。

> 斐波那奇数列不用递归

dp 搞起

> 1到100缺一个数找到它

先用公式去减，优化的化就二分

> leetcode 积水题目

TODO

> 反转字符串同时判断回文

TODO

> 写个bst class，各种操作写出signature即可，然后让在class里面写一个找successor的方法

TODO

> 

## On Site

> lru implementation + sychronization analys (哪些方法需要 synchronize， 为什么)

这一题感觉时间略紧张，要在40分钟不到的时间里写完，还要求尽量把code 的readability 弄好一点

> 给定两个array, 从每个array的最后一个元素起，交叉放置两个array里面的元素，形成一个新的array。就像洗牌一样，交叉放置，从array1的最后一个开始

TODO

> tree 的 dfs traversal

TODO

> min stack



> 写hash function

TODO

> Valid Telephone number

TODO

> 设计一个算法比较字符串间的相似度，返回[0,1]间的一个数

open question


