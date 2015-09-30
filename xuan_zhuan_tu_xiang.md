+ 难度：中等

给定一个N×N的二维矩阵表示图像，90度顺时针旋转图像。

样例

    给出一个矩形[[1,2],[3,4]]，90度顺时针旋转后，返回[[3,1],[4,2]]

挑战

    能否在原地完成？

## 题解

一次转一层，画图即可

```java
public class Solution {
    /**
     * @param matrix: A list of lists of integers
     * @return: Void
     */
    public void rotate(int[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0) {
            return;
        }

        int length = matrix.length;

        for (int i = 0; i < length / 2; i++) {
            for (int j = 0; j < (length + 1) / 2; j++){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[length - j - 1][i];
                matrix[length -j - 1][i] = matrix[length - i - 1][length - j - 1];
                matrix[length - i - 1][length - j - 1] = matrix[j][length - i - 1];
                matrix[j][length - i - 1] = tmp;
            }
        }
    }
}

```