+ 难度：简单

给定一个只含非负整数的`m*n`网格，找到一条从左上角到右下角的可以使数字和最小的路径。

注意

    你在同一时间只能向下或者向右移动一步

## 题解

新开一个数组进行累加即可。

```java
public class Solution {
    /**
     * @param grid: a list of lists of integers.
     * @return: An integer, minimizes the sum of all numbers along its path
     */
    public int minPathSum(int[][] grid) {
        int rowNum = grid.length;
        if (rowNum==0) return 0;
        int colNum = grid[0].length;
        if (colNum==0) return 0;

        int[][] sum = new int[rowNum][colNum];
        sum[0][0] = grid[0][0];
        for (int i=1;i<rowNum;i++) sum[i][0] = sum[i-1][0]+grid[i][0];
        for (int i=1;i<colNum;i++) sum[0][i] = sum[0][i-1]+grid[0][i];

        for (int i=1;i<rowNum;i++)
            for (int j=1;j<colNum;j++){
                sum[i][j] = Math.min(sum[i-1][j],sum[i][j-1])+grid[i][j];
            }

        return sum[rowNum-1][colNum-1];
    }
}
```