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

### 无重复元素的情况

P(a1) = a1

P(a1a2) = a1a2, a2a1

P(a1a2a3) = a1a2a3, a1a3a2, a2a1a3, a2a3a1, a3a1a2, a3a2a1

从第二步到第三步，恩可以看作是，对第二步中的两个结果，分别把 a3 插入到的每个结果中可能的各个位置，于是我们可以根据这个规律写出代码

```java
public static ArrayList<String> getPermutations(String str){
    if (str == null){
        return null;
    }

    ArrayList<String> permutations = new ArrayList<String>();
    if (str.length() == 0){
        permutations.add("");
        return permutations;
    }

    char first = str.charAt(0);
    String rest = str.substring(1);
    ArrayList<String> words = getPermutations(rest);
    for (String word : words){
        for (int j = 0; j <= word.length(); j++){
            permutations.add(word.substring(0,j) + first + word.substring(j));
        }
    }
    return permutations;
}
```

### 有重复元素的情况

We would like to only create the unique permuatations, rather than creating every permutation and then ruling out the duplicates

We can start with computing the count of each letter(hash table) for a string such as aabbbbc, we have a(2) b(4) c(1).

The first choice we make is whether to use an a, b or c as the first character. After that, we have a subp;roblem to solve: find all permutations of the remaining characters, and append those to the already picked "prefix"

P(a2|b4|c1) = {a + P(a1|b4|c1)} + {b + P(a2|b3|c1)} + {c + P(a2|b4|c0)} 依次递推

```java

ArrayList<String> get Perms(String s){
    ArrayList<String> result = new ArrayList<String>();
    HashMap<Character, Integer> map = buildFreqTable(s);
    getPerms(map, "", s.length(), result);
    return result;
}

HashMap<Character, Integer> buildFreqTable(String s){
    ArrayList<String> map = new HashMap<Character, Integer>();
    for (char c : s.toCharArray()) {
        if (!map.containsKey(c))
            map.put(c, 0);
            
        map.put(c, map.get(c) + 1;
    }
    return map;
}

void getPerms(HashMap<Character, Integer> map, String prefix, int remaining, ArrayList<String> result) {
    if (remaining == 0){
        result.add(prefix);
        return;
    }
    
    for (Character c : map.keySet()) {
        int count = map.get(c);
        if (count > 0) {
            map.put(c, count - 1);
            getPerms(map, prefix + c, remaining - 1, result);
            map.put(c, count);
        }
    }
}

```

## reverse linkedlist

```java
public class Solution {

    // recursive
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        if(head.next == null) return head;

        ListNode tail = head.next;
        ListNode reversed = reverseList(head.next);

        tail.next = head;

        head.next = null;

        return reversed;
    }

    // iterative
    public ListNode reverseList_2(ListNode head) {
        if(head==null || head.next == null) 
            return head;

        ListNode p1 = head;
        ListNode p2 = head.next;

        head.next = null;
        while(p1!= null && p2!= null){
            ListNode t = p2.next;
            p2.next = p1;
            p1 = p2;
            if (t!=null){
                p2 = t;
            }else{
                break;
            }
        }

        return p2;
    }
}
```

## all characters combinations 


## 设计一个parking lot

这是一个非常常见的设计题目，stackoverflow有一个帖子对这个题目描述的非常好，可以参考一下。

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