# 交叉字符串

给出三个字符串:s1、s2、s3，判断s3是否由s1和s2交叉构成。

样例

    比如 s1 = "aabcc" s2 = "dbbca"
    
    当 s3 = "aadbbcbcac"，返回  true.
    当 s3 = "aadbbbaccc"， 返回 false.

挑战

    要求时间复杂度为O(n^2)或者更好

## 题解

这是一道关于字符串操作的题目，要求是判断一个字符串能不能由两个字符串按照他们自己的顺序，每次挑取两个串中的一个字符来构造出来。

像这种判断能否按照某种规则来完成求是否或者某个量的题目，很容易会想到用动态规划来实现。

动态规划重点在于找到：维护量，递推式。维护量通过递推式递推，最后往往能得到想要的结果

先说说维护量，`res[i][j`]表示用`s1`的前`i`个字符和`s2`的前`j`个字符能不能按照规则表示出`s3`的前`i+j`个字符，如此最后结果就是`res[s1.length()][s2.length()]`，判断是否为真即可。接下来就是递推式了，假设知道`res[i][j]`之前的所有历史信息，我们怎么得到`res[i][j]`。可以看出，其实只有两种方式来递推，一种是选取`s1`的字符作为`s3`新加进来的字符，另一种是选`s2`的字符作为新进字符。而要看看能不能选取，就是判断`s1(s2)`的第`i(j)`个字符是否与`s3`的`i+j`个字符相等。如果可以选取并且对应的`res[i-1][j](res[i][j-1])`也为真，就说明`s3`的`i+j`个字符可以被表示。这两种情况只要有一种成立，就说明`res[i][j]`为真，是一个或的关系。所以递推式可以表示成

    res[i][j] = res[i-1][j] && s1.charAt(i-1)==s3.charAt(i+j-1) || 
                res[i][j-1] && s2.charAt(j-1)==s3.charAt(i+j-1)

```java
public class Solution {
    /**
     * Determine whether s3 is formed by interleaving of s1 and s2.
     * @param s1, s2, s3: As description.
     * @return: true or false.
     */
    public boolean isInterleave(String s1, String s2, String s3) {
        if (s3.length() != s1.length() + s2.length()) return false;
        if (s1=="" && s2=="" && s3=="") return true;
        boolean[][] res = new boolean[s1.length()+1][s2.length()+1];
        for (int i=0; i<=s1.length(); i++) {
            for (int j=0; j<=s2.length(); j++) {
                if (i==0 && j==0) 
                    res[i][j] = true;
                else if (i>0 && j==0) 
                    res[i][j] = res[i-1][j] && s1.charAt(i-1)==s3.charAt(i+j-1);
                else if (i==0 && j>0) 
                    res[i][j] = res[i][j-1] && s2.charAt(j-1)==s3.charAt(i+j-1);
                else {
                    res[i][j] = res[i-1][j] && s1.charAt(i-1)==s3.charAt(i+j-1) || res[i][j-1] && s2.charAt(j-1)==s3.charAt(i+j-1);
                }
            }
        }
        return res[s1.length()][s2.length()];
    }
}

```