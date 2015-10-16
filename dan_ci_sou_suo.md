# 单词搜索

给出一个二维的字母板和一个单词，寻找字母板网格中是否存在这个单词。

单词可以由按顺序的相邻单元的字母组成，其中相邻单元指的是水平或者垂直方向相邻。每个单元中的字母最多只能使用一次。

样例

    给出board =
    [
      "ABCE",
      "SFCS",
      "ADEE"
    ]

    word = "ABCCED"， ->返回 true,
    word = "SEE"，-> 返回 true,
    word = "ABCB"， -> 返回 false.

## 题解

1. Locate the first char and start the recursive search
2. Backtracking. Set and unset the visited flag.

```java
public class Solution {
    /**
     * @param board: A list of lists of character
     * @param word: A string
     * @return: A boolean
     */
    public boolean exist(char[][] board, String word) {
        if (board==null || board.length==0) return false;
        boolean visited[][] = new boolean[board.length][board[0].length];
        for (int i=0; i < board.length; i++){
            for (int j=0; j < board[0].length; j++){
                if (search(word, 0, board, i, j, visited)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean search(String word, int index, char[][] board, int i, int j, boolean[][] visited){
        if (i<0 || j<0 || i==board.length || j==board[0].length || visited[i][j]==true) return false;
        visited[i][j] = true;
        boolean result = false;
        if (board[i][j]==word.charAt(index)){
            if (index == word.length()-1) return true;
            //save the result here instead of just return the result, as we need to unset the visited matrix before return
            result = search(word, index+1, board, i-1, j, visited) ||
                            search(word, index+1, board, i+1, j, visited) ||
                            search(word, index+1, board, i, j-1, visited) ||
                            search(word, index+1, board, i, j+1, visited);
        }
        visited[i][j] = false;
        return result;
    }
}

```