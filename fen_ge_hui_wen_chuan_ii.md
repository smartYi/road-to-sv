# 分割回文串 II

给定一个字符串s，将s分割成一些子串，使每个子串都是回文。

返回s符合要求的的最少分割次数。

样例

    比如，给出字符串s = "aab"，
    返回 1， 因为进行一次分割可以将字符串s分割成["aa","b"]这样两个回文子串

## 题解

使用DP来解决：

1. D[i]  表示前i 个字符切为回文需要的切割数
2. P[i][j]: S.sub(i-j) is a palindrome.
3. 递推公式： D[i] = Math.min(D[i], D[j] + 1), 0 <= j <= i - 1) ，并且要判断 P[j][i - 1]是不是回文。
4. 注意D[0] = -1的用意，它是指当整个字符串判断出是回文是，因为会D[0] + 1 其实应该是结果为0（没有任何切割），所以，应把D[0] 设置为-1

有个转移函数之后，一个问题出现了，就是如何判断[i,j]是否是回文？每次都从i到j比较一遍？太浪费了，这里也是一个DP问题。

定义函数

P[i][j] = true if [i,j]为回文

那么
P[i][j] = str[i] == str[j] && P[i+1][j-1];

```java
public class Solution {
    /**
     * @param s a string
     * @return an integer
     */
    public int minCut(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        int len = s.length();

        // D[i]: 前i 个字符切为回文需要的切割数
        int[] D = new int[len + 1];
        D[0] = -1;

        // P[i][j]: S.sub(i-j) is a palindrome.
        boolean[][] P = new boolean[len][len];

        for (int i = 1; i <= len; i++) {
            // The worst case is cut character one by one.
            D[i] = i - 1;
            for (int j = 0; j <= i - 1; j++) {
                P[j][i - 1] = false;
                if (s.charAt(j) == s.charAt(i - 1) && (i - 1 - j <= 2 || P[j + 1][i - 2])) {
                    P[j][i - 1] = true;
                    D[i] = Math.min(D[i], D[j] + 1);
                }
            }
        }

        return D[len];
    }
};

```