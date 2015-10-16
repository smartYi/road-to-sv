# 单词切分

给出一个字符串s和一个词典，判断字符串s是否可以被空格切分成一个或多个出现在字典中的单词。

样例

    给出 s = "lintcode"
    dict = ["lint","code"]
    返回 true 因为"lintcode"可以被空格切分成"lint code"

## 题解

It is a DP problem. However, we need to use charAt() instead of substring() to optimize speed.Declare an boolean array of (input string length) + 1, and dp[i] mean whether or not subtring(0,i) is work-breakable. Then the problem is clear and easy.

```java
public class Solution {
    /**
     * @param s: A string s
     * @param dict: A dictionary of words dict
     */
    public boolean wordBreak(String s, Set<String> dict) {
        if (s.length()==0) return true;

        char[] chars = new char[256];
        for (String word : dict)
            for (int i=0;i<word.length();i++)
                chars[word.charAt(i)]++;

        for (int i = 0;i<s.length();i++)
            if (chars[s.charAt(i)]==0) return false;

        boolean[] d = new boolean[s.length()+1];
        Arrays.fill(d,false);
        d[0] = true;
        for (int i=1;i<=s.length();i++){
        StringBuilder builder = new StringBuilder();
            for (int j=i-1;j>=0;j--){
                builder.insert(0,s.charAt(j));
                String cur = builder.toString();
                if (d[j] && dict.contains(cur)){
                    d[i]=true;
                    break;
                }
            }
        }

        return d[s.length()];
    }
}

```

