# Robot in a Grid

Imagine a robot sitting on the upper left corner of grid with r rows and c columns. The robot can only move in two directions, right and down, but certain cells are "off limits" such that the robot cannot step on them. Design an algorithm to find a path from the top left to the bottom right.

## Solution

```java
// Learn to code from the solution provided by the book
class Point{
    int row;
    int col;

    public Point(int r, int c){
        row = r;
        col = c;
    }
}


ArrayList<Point> getPath(boolean[][] maze){
    if (maze == null || maze.length == 0)
        return null;

    ArrayList<Point> path = new ArrayList<Point>();
    HashMap<Point, Boolean> cache = new HashMap<Point, Boolean>();
    int lastRow = maze.length - 1;
    int lastCol = maze[0].length - 1;

    if (getPath(maze, lastRow, lastCol, path, cache)){
        return path;
    }
    return null;
}

boolean getPath(boolean[][] maze, int row, int col, ArrayList<Point> path,
                HashMap<Point, Boolean> cache){
    if (col < 0 || row < 0 || !maze[row][col]){
        return false;
    }

    Point p = new Point(row, col);

    if (cache.containsKey(p)){
        return cache.get(p);
    }

    boolean isAtOrigin = (row == 0) && (col == 0);
    boolean success = false;
    if (isAtOrigin || getPath(maze, row, col - 1, path, cache)
            || getPath(maze, row - 1, col, path, cache)){
        path.add(p);
        success = true;
    }

    cache.put(p, success);
    return success;
}
```