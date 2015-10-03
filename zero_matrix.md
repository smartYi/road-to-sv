# Zero Matrix

Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0

## Solution

```java
/**
 * We can reduce the space to O(1) by using the first row and the first
 * column to record if we need to set the specific row or column to zero.
 *
 * 1. Check if the first row and first column have any zeros
 * 2. Iterate through the rest of the matrix, setting matrix[i][0] and
 * matrix[0][j] to zero whenever there's a zero in matrix[i][j]
 * 3. Iterate through rest of matrix, nullifying row i if there's a zero
 * in matrix[i][0]
 * 4. Iterate through rest of matrix, nullifying column j if there's a zero
 * in matrix[0][j]
 * 5. Nullify the first row and first column , if necessary (based on 1.)
 */

public static int[][] zeroMatrix(int[][] mat, int n){
    boolean rowHasZero = false;
    boolean colHasZero = false;

    // Check if first row has a zero
    for (int j = 0; j < mat[0].length; j++){
        if (mat[0][j] == 0){
            rowHasZero = true;
            break;
        }
    }

    // Check if first column has a zero
    for (int i = 0; i < mat.length; i++){
        if (mat[i][0] == 0){
            colHasZero = true;
            break;
        }
    }

    // Check for zeros in the rest of the array
    for(int i = 1; i < mat.length; i++){
        for (int j = 1; j < mat[0].length; j++){
            if (mat[i][j] == 0){
                mat[i][0] = 0;
                mat[0][j] = 0;
            }
        }
    }

    // Nullify rows based on values in first column
    for (int i = 1; i < mat.length; i++){
        if (mat[i][0] == 0){
            nullifyRow(mat, i);
        }
    }

    // Nullify columns based on values in first row
    for (int j = 1; j < mat[0].length; j++){
        if (mat[0][j] == 0){
            nullifyColumn(mat, j);
        }
    }

    if (rowHasZero){
        nullifyRow(mat, 0);
    }

    if (colHasZero){
        nullifyColumn(mat, 0);
    }

    return mat;

}

public static void nullifyRow(int[][] mat, int row){
    for (int j = 0; j < mat[0].length; j++){
        mat[row][j] = 0;
    }
}

public static void nullifyColumn(int[][] mat, int col){
    for (int i = 0; i < mat.length; i++){
        mat[i][col] = 0;
    }
}
```