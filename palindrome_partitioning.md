# Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = "aab",

Return

    [
        ["aa","b"],
        ["a","a","b"]
    ]

## Solution

这种需要输出所有结果的基本上都是DFS的解法。

遇到要求所有组合、可能、排列等解集的题目，一般都是用DFS + backtracking来做。要分割回文的前提是能够判断回文。在做DFS的时候，如果每次从start出发查找s[start:end]是否是回文，会使算法复杂度大大增加。而在Longest Palindromic Substring这题中已经知道如何用DP来计算任意s[i:j]是否是回文。因此可以先计算该判断矩阵，在DFS的时候用来剪枝，大大提高效率。

```java
public class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res = new ArrayList<List<String>>();
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for (int i = n - 1; i >= 0; --i) {
            for (int j = i; j < n; ++j) {
                dp[i][j]=(s.charAt(i)==s.charAt(j))&&(j<i+2||dp[i+1][j-1]);
            }
        }
        ArrayList<String> path = new ArrayList<String>();
        dfs(s, dp, 0, path, res);
        return res;
    }
    public void dfs(String s, boolean[][] dp, int start, ArrayList<String> path, List<List<String>> res) {
        if (s.length() == start) {
            res.add(new ArrayList<String>(path));
            return;
        }
        for (int i = start; i < s.length(); ++i) {
            if (dp[start][i] == false) continue;
            path.add(s.substring(start,i+1));
            dfs(s, dp, i+1,path,res);
            path.remove(path.size()-1);
        }
    }
}
```