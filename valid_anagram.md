# Valid Anagram

Given two strings s and t, write a function to determine if t is an anagram of s.

For example,

s = "anagram", t = "nagaram", return true.

s = "rat", t = "car", return false.

Note:

You may assume the string contains only lowercase alphabets.

## Solution

basic hash table with array

```java
public class Solution {
    public boolean isAnagram(String s, String t) {
        if (null == s && null == t) {
            return true;
        }
        if (null == s || null == t || s.length() != t.length()) {
            return false;
        }
        int[] letters = new int[26];
        for (int i = 0; i < s.length(); i++) {
            letters[s.charAt(i) - 'a']++;
        }
        for (int i = 0; i < t.length(); i++) {
            if (--letters[t.charAt(i) - 'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```