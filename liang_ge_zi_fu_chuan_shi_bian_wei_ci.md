+ 难度：简单

写出一个函数 anagram(s, t) 去判断两个字符串是否是颠倒字母顺序构成的

样例

    给出 s="abcd"，t="dcab"，返回 true

## 题解

分别对两个字符串排序，然后比较即可，注意在 python 中字符串是没有办法修改的，所以需要转换成 list 然后进行处理


```python
# python
class Solution:
    """
    @param s: The first string
    @param b: The second string
    @return true or false
    """
    def anagram(self, s, t):
        # write your code here
        ls = list(s)
        lt = list(t)
        ls.sort()
        lt.sort()
        s = "".join(ls)
        t = "".join(lt)

        if s == t:
            return True
        return False
```

```java
// java
public class Solution {
    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
    public boolean anagram(String s, String t) {
        // write your code here
        if (s==null || t==null || s.length()!=t.length()) return false;
        int[] ss = new int[256];
        int[] tt = new int[256];
        for (int i=0; i<s.length(); i++) {
            ss[s.charAt(i)]++;
            tt[t.charAt(i)]++;
        }
        for (int j=0; j<256; j++) {
            if (ss[j] != tt[j]) return false;
        }
        return true;
    }
};

```