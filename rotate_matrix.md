# Rotate Matrix

Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

## Solution

```java
public static void rotate(int[][] mat, int n){
    for (int layer = 0; layer < n/2; layer++){
        int first = layer;
        int last = n - 1 - layer;
        for (int i = first; i < last; i++){
            int offset = i - first;
            // save top
            int top = matrix[first][i];
            // left -> top
            matrix[first][i] = matrix[last-offset][first];
            // bottom -> left
            matrix[last-offset][first] = matrix[last][last-offset];
            // right -> bottom
            matrix[last][last-offset] = matrix[i][last];
            // top -> right
            matrix[i][last] = top;
        }
    }
}

public static void rotateinv(int[][] mat, int n){
    for (int layer = 0; layer < n/2; layer++){
        int first = layer;
        int last = n - 1 - layer;
        for (int i = first; i < last; i++){
            int offset = i - first;
            // save top
            int top = matrix1[first][i];
            // right -> top
            matrix1[first][i] = matrix1[i][last];
            // bottom -> right
            matrix1[i][last] = matrix1[last][last-offset];
            // left -> bottom
            matrix1[last][last-offset] = matrix1[last - offset][first];
            // top -> right
            matrix1[last-offset][first] = top;
        }
    }
}
```