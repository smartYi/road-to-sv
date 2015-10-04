# Eight Queens

Write an algorithm to print all ways of arranging eight queens on an 8x8 chess board so that none of them share teh same row, column, or diagonal. In this case, "diagonal" means all diagonals, not just the two that bisect the board.

## Solution

```java
// Use the solution from the book to help understanding
private static int GRID_SIZE = 8;

public static void placeQueens(int row, Integer[] columns, ArrayList<Integer[]> results){
    if (row == GRID_SIZE){
        results.add(columns.clone());
    }
    else {
        for (int col = 0; col < GRID_SIZE; col++){
            if (checkValid(columns, row, col)){
                columns[row] = col;
                placeQueens(row + 1, columns, results);
            }
        }
    }
}

public static boolean checkValid(Integer[] columns, int row1, int column1){
    for (int row2 = 0; row2 < row1; row2++){
        int column2 = columns[row2];
        if (column1 == column2){
            return false;
        }

        int columnDis = Math.abs(column2 - column1);
        int rowDis = row1 - row2;
        if (columnDis == rowDis){
            return false;
        }
    }
    return true;
}
```