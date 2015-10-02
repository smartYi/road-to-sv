# 最长公共子序列

给出两个字符串，找到最长公共子序列(LCS)，返回LCS的长度。

样例

    给出"ABCD" 和 "EDCA"，这个LCS是 "A" (或 D或C)，返回1
    给出 "ABCD" 和 "EACB"，这个LCS是"AC"返回 2

## 题解

DP.

1. D[i][j] 定义为s1, s2的前i,j个字符串的最长common subsequence.
2. D[i][j] 当char i == char j， D[i - 1][j - 1] + 1

当char i != char j, D[i ][j - 1], D[i - 1][j] 里取一个大的（因为最后一个不相同，所以有可能s1的最后一个字符会出现在s2的前部分里，反之亦然。

```java
public class Solution {
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
    public int longestCommonSubsequence(String A, String B) {
        if (A == null || B == null) {
            return 0;
        }

        int lenA = A.length();
        int lenB = B.length();
        int[][] D = new int[lenA + 1][lenB + 1];

        for (int i = 0; i <= lenA; i++) {
            for (int j = 0; j <= lenB; j++) {
                if (i == 0 || j == 0) {
                    D[i][j] = 0;
                } else {
                    if (A.charAt(i - 1) == B.charAt(j - 1)) {
                        D[i][j] = D[i - 1][j - 1] + 1;
                    } else {
                        D[i][j] = Math.max(D[i - 1][j], D[i][j - 1]);
                    }
                }
            }
        }

        return D[lenA][lenB];
    }
}


```