# BrightEdge

Round 1: behavioral电面

Round 2：一个48小时coding assignment，每个人的具体内容略不相同，大体上是用 java实现一个网络爬虫。我当时的任务是在sears.com上做keyword搜索并返回相关结果。

Round 3：tech电面。一名小印。

1. Fibonacci数列。当时还问recursive的复杂度，是O(2^n)对。
2. Leetcode原题：如何判断一个BST是否valid。

---

第一题：String permutation

依照之前在cracking coding interview的方法迅速写了一个递归的方法。

不过后来指出这个对于有duplicate不太好。于是我告诉他用hashset存放最后的结果可以免除重复。但是后来他又说有没有其他方法。

于是我依稀记得leetcode上的permutation 2就是讲duplicate怎么处理的，于是用dfs+backtracking的方法重新写了一个可以去除duplicate的版本。同时针对这个算法还争论了一番，最后烙印表示应该是正确的。

第二题：判断一个Binary Tree是不是Binary Search Tree


高频!!第三题：design的题目，设计一个parking lot。这是一个非常常见的设计题目，stackoverflow有一个帖子对这个题目描述的非常好，可以参考一下。

---

第一题 reverse linkedlist  CC原题不能更多

第二题 all characters combinations 

leetcode原题，不能更多，就是 combinations的变形

---

第二题叫你写SQL，有Employee的last name和first name，要求列出Emplyee’s first name and last name, but last name should not start with A and L. Use LIKE

第三题是给你一个ATM Function to withdraw money,问如何测试

第四题就是假如给你一个website，问你如何测试。

---

Partition array

1. input: [1, 2, 3, 6, 0, 0, 3, 1, 9, 0]
2. output: [1, 2, 3, 6, 3, 1, 9, 0, 0, 0]


Parking Garage

+ multiple floors
+ each floor has multiple parking spots
+ parking spots has two types. handicapped or regular parking.
+ Question: how many regular parking are available at 2nd floor?


---

1. Binary tree lowest common ancestor with parent pointer. 需要O（1）space
2.Design a parking lot. 需要实现（1）知道第二层停了多少车 （2）知道第二层有多少available

---

1. You are given the root of the Binary Search Tree and two children nodes, please return the first common ancestor of these two nodes.
2. Mirror a binary tree.
3. implement pow(double a, int b).