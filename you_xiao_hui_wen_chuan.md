+ 难度：容易

给定一个字符串，判断其是否为一个回文串。只包含字母和数字，忽略大小写。

样例

    "A man, a plan, a canal: Panama" 是一个回文。
    "race a car" 不是一个回文。

注意

    你是否考虑过，字符串有可能是空字符串？这是面试过程中，面试官常常会问的问题。
    在这个题目中，我们将空字符串判定为有效回文。

挑战

    O(n) 时间复杂度，且不占用额外空间。

## 题解

两步走：

1. 找到最左边和最右边的第一个合法字符(字母或者字符)
2. 一致转换为小写进行比较

字符的判断尽量使用语言提供的 API

```java
public class Solution {
    /**
     * @param s A string
     * @return Whether the string is a valid palindrome
     */
    public boolean isPalindrome(String s) {
        if (s == null || s.isEmpty()) return true;

        int l = 0, r = s.length() - 1;
        while (l < r) {
            // find left alphanumeric character
            if (!Character.isLetterOrDigit(s.charAt(l))) {
                l++;
                continue;
            }
            // find right alphanumeric character
            if (!Character.isLetterOrDigit(s.charAt(r))) {
                r--;
                continue;
            }
            // case insensitive compare
            if (Character.toLowerCase(s.charAt(l)) == Character.toLowerCase(s.charAt(r))) {
                l++;
                r--;
            } else {
                return false;
            }
        }

        return true;
    }
}

```