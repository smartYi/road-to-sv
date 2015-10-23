# BrightEdge

小公司，题目重复率也比较高，基本都是印度人面。我的体验是第一轮  behavioral电面，然后就发一个48小时coding assignment，每个人的具体内容略不相同，大体上是用 java实现一个网络爬虫。

下面是具体的面试题准备

## Fibonacci 数列

### 递归版本

```
int fibonacciRecursive(int n){
    if (n < 2)
        return n;
    
    return f(n-1) + f(n-2);
}
```
时间复杂度 O(2^n) 空间复杂度 O(n)

### 迭代版本

```
int fibonacci(int n){
    if (n < 2)
        return n;
    
    int a = 0;
    int b = 1;
    int c = -1;
    for (int i = 2; i < n; i++){
        c = a + b;
        a = b;
        b = c;
    }
    return c;
}

```

时间复杂度 O(n) 空间复杂度 O(1)


## 如何判断一个BST是否valid。

也就是判断一个Binary Tree是不是Binary Search Tree

1. Add lower & upper bound. O(n)
2. Inorder traversal with one additional parameter (value of predecessor). O(n)

```java
public class Solution {
    boolean isValidBSTRe(TreeNode root, long left, long right)
    {
        if(root == null) return true;
        return left < root.val && root.val < right &&
                isValidBSTRe(root.left,left,root.val) 
                && isValidBSTRe(root.right, root.val, right);
    }
    public boolean isValidBST_1(TreeNode root) {
        if (root == null) return true;
        return isValidBSTRe(root, (long)Integer.MIN_VALUE - 1, (long)Integer.MAX_VALUE + 1);
    }
    
    boolean isValidBST(TreeNode root) {
        long[] val = new long[1];
        val[0] = (long)Integer.MIN_VALUE - 1;
        return inorder(root, val);
    }
    
    boolean inorder(TreeNode root, long[] val) {
        if (root == null) return true;
        if (inorder(root.left, val) == false) 
            return false;
        if (root.val <= val[0]) return false;
        val[0] = root.val;
        return inorder(root.right, val);
    }
}

```

用中序遍历的方法遍历BST，BST的性质是遍历后数组是有序的。根据这一点我们只需要中序遍历这棵树，然后保存前驱结点，每次检测是否满足递增关系即可。注意以下代码我么用一个一个变量的数组去保存前驱结点，原因是java没有传引用的概念，如果传入一个变量，它是按值传递的，所以是一个备份的变量，改变它的值并不能影响它在函数外部的值，算是java中的一个小细节。

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
    public boolean isValidBST(TreeNode root) {
        ArrayList<Integer> pre = new ArrayList<Integer>();
        pre.add(null);
        return helper(root, pre);
    }
    private boolean helper(TreeNode root, ArrayList<Integer> pre)
    {
        if(root == null)
            return true;
        boolean left = helper(root.left,pre);
        if(pre.get(0)!=null && root.val<=pre.get(0))
            return false;
        pre.set(0,root.val);
        return left && helper(root.right,pre);
    }
}
```

## String permutation

lintcode: 带重复元素的排列

cc: Permutations with / without Dups







## 设计一个parking lot

这是一个非常常见的设计题目，stackoverflow有一个帖子对这个题目描述的非常好，可以参考一下。

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