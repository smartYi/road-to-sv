# 统计比给定整数小的数的个数

给定一个整数数组 （下标由 0 到 n-1，其中 n 表示数组的规模，数值范围由 0 到 10000），以及一个 查询列表。对于每一个查询，将会给你一个整数，请你返回该数组中小于给定整数的元素的数量。

样例

    对于数组 [1,2,7,8,5] ，查询 [1,8,5]，返回 [0,4,2]

注意
    
    在做此题前，最好先完成 线段树的构造 and 线段树查询 II 这两道题目。

挑战

    可否用一下三种方法完成以上题目。

    1. 仅用循环方法
    2. 分类搜索 和 二进制搜索
    3. 构建 线段树 和 搜索

## 题解

binary search

```java
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
        Arrays.sort(A);
        ArrayList<Integer> res = new ArrayList<Integer>();
        for (int q : queries) {
            res.add(binarySearch(A, q));
        }
        return res;
    }
    private int binarySearch(int[] A, int val) {
        int start = 0, end = A.length - 1;
        int res = 0;
        while (start <= end) {
            int mid = (start + end) / 2;
            if (A[mid] >= val) {
                end = mid - 1;
            } else {
                res = mid + 1;
                start = mid + 1;
            }
        }
        return res;
    }
}
```

Segment tree

```java
public class Solution {
   /**
     * @param A: An integer array
     * @return: The number of element in the array that 
     *          are smaller that the given integer
     */
    public ArrayList<Integer> countOfSmallerNumber(int[] A, int[] queries) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        if (A == null || queries == null || queries.length == 0) {
            return res;
        }
        Arrays.sort(A);
        MaxNode root = buildTree(A, 0, A.length - 1);
         
        for (int q : queries) {
            res.add(query(A, root, q));
        }
        return res;
    }
    private MaxNode buildTree(int[] A, int start, int end) {
        if (start > end) {
            return null;
        }
        MaxNode root = new MaxNode(start, end);
        if (start == end) {
            root.val = A[start];
        } else {
            root.left = buildTree(A, start, (start+end)/2);
            root.right = buildTree(A, (start+end)/2+1, end);
            root.val = root.left == null ? root.right.val : Math.max(root.left.val,
                root.right == null ? 0 : root.right.val);
        }
        return root;
    }
    private int query(int[] A, MaxNode root, int val) {
        if (root == null || A[root.start] > val) {
            return 0;
        }
        if (root.val < val) {
            return root.end - root.start + 1;
        }
        return query(A, root.left, val) + query(A, root.right, val);
    }
    class MaxNode {
        int start;
        int end;
        int val; //how many nodes are between the range start - end (inclusive)
        MaxNode left;
        MaxNode right;
        public MaxNode() {
             
        }
        public MaxNode(int start, int end) {
            this.start = start;
            this.end = end;
        }
        public MaxNode(int start, int end, int val) {
            this(start, end);
            this.val = val;
        }
    }
}
```