# 交替字符串

输入三个字符串`s1`、`s2`和`s3`，判断第三个字符串`s3`是否由前两个字符串`s1`和`s2`交错而成，即不改变`s1`和`s2`中各个字符原有的相对顺序，例如当`s1 = “aabcc”，s2 = “dbbca”，s3 = “aadbbcbcac”`时，则输出`true`，但如果`s3=“accabdbbca”`，则输出`false`。

## 分析与解法

此题不能简单的排序，因为一旦排序，便改变了`s1`或`s2`中各个字符原始的相对顺序，既然不能排序，咱们可以考虑下用动态规划的方法，令`dp[i][j]`代表`s3[0...i+j-1]`是否由`s1[0...i-1]`和`s2[0...j-1]`的字符组成

+ 如果`s1`当前字符(即`s1[i-1]`)等于`s3`当前字符(即`s3[i+j-1]`)，而且`dp[i-1][j]`为真，那么可以取`s1`当前字符而忽略`s2`的情况，`dp[i][j]`返回真；
+ 如果`s2`当前字符等于`s3`当前字符，并且`dp[i][j-1]`为真，那么可以取`s2`而忽略`s1`的情况，`dp[i][j]`返回真，其它情况，`dp[i][j]`返回假

参考代码如下：

```cpp
public boolean IsInterleave(String s1, String 2, String 3){
    int n = s1.length(), m = s2.length(), s = s3.length();

    //如果长度不一致，则s3不可能由s1和s2交错组成
    if (n + m != s)
        return false;

    boolean[][]dp = new boolean[n + 1][m + 1];

    //在初始化边界时，我们认为空串可以由空串组成，因此dp[0][0]赋值为true。
    dp[0][0] = true;

    for (int i = 0; i < n + 1; i++){
        for (int j = 0; j < m + 1; j++){
            if ( dp[i][j] || (i - 1 >= 0 && dp[i - 1][j] == true &&
                //取s1字符
                s1.charAT(i - 1) == s3.charAT(i + j - 1)) ||

                (j - 1 >= 0 && dp[i][j - 1] == true &&
                //取s2字符
                s2.charAT(j - 1) == s3.charAT(i + j - 1)) )

                dp[i][j] = true;
            else
                dp[i][j] = false;
        }
    }
    return dp[n][m]
}
```

理解本题及上段代码，对真正理解动态规划有一定帮助。