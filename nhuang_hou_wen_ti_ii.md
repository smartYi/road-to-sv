# N 皇后问题 II

根据n皇后问题，现在返回n皇后不同的解决方案的数量而不是具体的放置布局。

样例

    比如n=4，存在2种解决方案

## 题解

```java
class Solution {
    /**
     * Calculate the total number of distinct N-Queen solutions.
     * @param n: The number of queens.
     * @return: The total number of distinct solutions.
     */
    public int totalNQueens(int n) {
        if (n == 0) {
            return 0;
        }

        // Bug 1: forget to modify the parameters of the function.
        return dfs(n, 0, new ArrayList<Integer>());
    }

    public int dfs(int n, int row, ArrayList<Integer> path) {
        if (row == n) {
            // The base case: 当最后一行，皇后只有1种放法(就是不放)
            return 1;
        }

        int num = 0;

        // The queen can select any of the slot.
        for (int i = 0; i < n; i++) {
            if (!isValid(path, i)) {
                continue;
            }
            path.add(i);

            // All the solutions is all the possiablities are add up.
            num += dfs(n, row + 1, path);
            path.remove(path.size() - 1);
        }

        return num;
    }

    public boolean isValid(ArrayList<Integer> path, int col) {
        int size = path.size();
        for (int i = 0; i < size; i++) {
            // The same column with any of the current queen.
            if (col == path.get(i)) {
                return false;
            }

            // diagonally lines.
            // Bug 2: forget to add a ')'
            if (size - i == Math.abs(col - path.get(i))) {
                return false;
            }
        }

        return true;
    }
};

```