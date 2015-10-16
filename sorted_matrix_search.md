# Sorted Matrix Search

Given an M x N matrix in which each row and each column is sorted in ascending order, write a method to find an element.

## Solution

```java
/**
 * 1. If the start of a column is greater than x, then x is to the left of
 * the column.
 * 2. If the end of a column is less than x, then x is to the right of the
 * column.
 * 3. If the start of a row is greater than x, then x is above the row.
 * 4. If the end of a row is less than x, then x is below that row
 */
boolean findElement(int[][] matrix, int elem){
    int row = 0;
    int col = matrix[0].length - 1;
    while (row < matrix.length && col >= 0){
        if (matrix[row][col] == elem){
            return true;
        }
        else if (matrix[row][col] > elem){
            col--;
        }
        else{
            row++;
        }
    }
    return false;
}
```