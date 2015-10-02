# 搜索二维矩阵 II

写出一个高效的算法来搜索m×n矩阵中的值，返回这个值出现的次数。

这个矩阵具有以下特性：

+ 每行中的整数从左到右是排序的。
+ 每一列的整数从上到下是排序的。
+ 在每一行或每一列中没有重复的整数。

样例

    考虑下列矩阵：

    [
        [1, 3, 5, 7],
        [2, 4, 7, 8],
        [3, 5, 9, 10]
    ]

    给出target = 3，返回 2

挑战

    要求O(m+n) 时间复杂度和O(1) 额外空间

## 题解

很巧妙的思路，可以从左下或者右上开始找

```java
public class Solution {
    /**
     * @param matrix: A list of lists of integers
     * @param: A number you want to search in the matrix
     * @return: An integer indicate the occurrence of target in the given matrix
     */
    public int searchMatrix(int[][] matrix, int target) {
        if (matrix==null || matrix.length==0 || matrix[0].length==0) return 0;
        int m = matrix.length;
        int n = matrix[0].length;
        int count = 0;
        int row = m-1;
        int col = 0;
        while (row>=0 && row < m && col >= 0 && col < n) {
            int cur = matrix[row][col];
            if (cur == target) {
                count++;
                col++;
                row--;
            }
            else if (cur > target) {
                row--;
            }
            else col++;
        }
        return count;
    }
}


```