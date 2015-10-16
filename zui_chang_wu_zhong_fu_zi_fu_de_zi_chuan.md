+ 难度：中等

给定一个字符串，请找出其中无重复字符的最长子字符串。

样例

    例如，在"abcabcbb"中，其无重复字符的最长子字符串是"abc"，其长度为 3。
    对于，"bbbbb"，其无重复字符的最长子字符串为"b"，长度为1。

挑战

    O(n) 时间

## 题解

维护一个 sliding window，然后利用 Hashset 来判断是否有出现这个值

```java
public class Solution {
    /**
     * @param s: a string
     * @return: an integer
     */
    public int lengthOfLongestSubstring(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }

        HashSet<Character> set = new HashSet<Character>();

        int leftBound = 0, max = 0;
        for (int i = 0; i < s.length(); i++) {
            if (set.contains(s.charAt(i))) {
                while (leftBound < i && s.charAt(leftBound) != s.charAt(i)) {
                    set.remove(s.charAt(leftBound));
                    leftBound ++;
                }
                leftBound ++;
            } else {
                set.add(s.charAt(i));
                max = Math.max(max, i - leftBound + 1);
            }
        }

        return max;
    }
}

```