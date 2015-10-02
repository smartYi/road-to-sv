# 最长公共前缀

给k个字符串，求出他们的最长公共前缀(LCP)

样例

    在 "ABCD" "ABEF" 和 "ACEF" 中,  LCP 为 "A"
    在 "ABCDEFG", "ABCEFG", "ABCEFA" 中, LCP 为 "ABC"

## 题解

To solve this problem, we need to find the two loop conditions. One is the length of the shortest string. The other is iteration over every element of the string array.

```java
public class Solution {
    /**
     * @param strs: A list of strings
     * @return: The longest common prefix
     */
    public String longestCommonPrefix(String[] strs) {
        if(strs == null || strs.length == 0)
            return "";
        String prefix = strs[0];
        for(int i = 1; i < strs.length; i++) {
            int j = 0;
            while(j < prefix.length() && j < strs[i].length() && prefix.charAt(j)==strs[i].charAt(j))
                j++;
            prefix = prefix.substring(0,j);
        }
        return prefix;
    }
}

```