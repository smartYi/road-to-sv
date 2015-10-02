# N 皇后问题

n皇后问题是将n个皇后放置在n*n的棋盘上，皇后彼此之间不能相互攻击。

给定一个整数n，返回所有不同的n皇后问题的解决方案。

每个解决方案包含一个明确的n皇后放置布局，其中“Q”和“.”分别表示一个女王和一个空位置。

样例

    对于4皇后问题存在两种解决的方案：

    [
        [".Q..", // Solution 1
         "...Q",
         "Q...",
         "..Q."],
        ["..Q.", // Solution 2
         "Q...",
         "...Q",
         ".Q.."]
    ]

挑战

    你能否不使用递归完成？

## 题解

```java
class Solution {
    /**
     * Get all distinct N-Queen solutions
     * @param n: The number of queens
     * @return: All distinct solutions
     * For example, A string '...Q' shows a queen on forth position
     */
    ArrayList<ArrayList<String>> solveNQueens(int n) {
        ArrayList<ArrayList<String>> res = new ArrayList<ArrayList<String>>();
        dfs(n, 0, new int[n], res);
        return res;
    }


    private void dfs(int n, int start, int[] row, ArrayList<ArrayList<String>> res){
        if(n == start){
            res.add(generating(row, n));
            return;
        }
        for(int i = 0; i < n; i++){
            row[start] = i;
            boolean isValid = true;
            for(int j = 0; j < start; j++){
                if(row[j] == i || Math.abs(row[j] - row[start]) == start - j){
                    isValid = false;
                    break;
                }

            }

            if(isValid == true)
                dfs(n, start+1, row, res);

        }
    }

    private ArrayList<String> generating(int[] row, int n){
        ArrayList<String> res = new ArrayList<String>();
        for(int i = 0; i < n; i++){
            StringBuffer buffer = new StringBuffer();
            for(int j = 0; j < n; j++){

                if(row[i] == j){
                    buffer.append("Q");
                } else {
                    buffer.append(".");
                }

            }
            res.add(buffer.toString());
        }
        return res;
    }
};

```