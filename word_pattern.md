# Word Pattern

Given a pattern and a string str, find if str follows the same pattern.

Examples:

pattern = "abba", str = "dog cat cat dog" should return true.
pattern = "abba", str = "dog cat cat fish" should return false.
pattern = "aaaa", str = "dog cat cat dog" should return false.
pattern = "abba", str = "dog dog dog dog" should return false.

Notes:

1. patterncontains only lowercase alphabetical letters, and str contains words separated by a single space. Each word in str contains only lowercase alphabetical letters.
2. Both pattern and str do not have leading or trailing spaces.
3. Each letter in pattern must map to a word with length that is at least 1.

## Solution
这道题给我们一个模式字符串，又给我们一个单词字符串，让我们求单词字符串中单词出现的规律是否符合模式字符串中的规律。那么首先想到就是用哈希表来做，建立模式字符串中每个字符和单词字符串每个单词之间的映射，而且这种映射必须是一对一关系的，不能'a‘和’b'同时对应‘dog'，所以我们在碰到一个新字符时，首先检查其是否在哈希表中出现，若出现，其映射的单词若不是此时对应的单词，则返回false。如果没有在哈希表中出现，我们还要遍历一遍哈希表，看新遇到的单词是否已经是哈希表中的映射，如果没有，再跟新遇到的字符建立映射，

```java
public class Solution {
    public boolean wordPattern(String pattern, String str) {
        Map<Character, String> map = new HashMap<Character, String>();
        Set<String> set = new HashSet<String>();
        String[] pieces = str.split(" ");
        if(pieces.length != pattern.length()) return false;
        int i = 0;
        for(String s : pieces){
            char p = pattern.charAt(i);
            System.out.println(p);
            if(map.containsKey(p)){
                if(!s.equals(map.get(p))) return false;
            } else {
                if(set.contains(s)) return false;
                map.put(p, s);
                set.add(s);
            }
            i++;
        }
        return true;
    }
}
```