给定一个字符串和一个偏移量，根据偏移量旋转字符串(从左向右旋转)

样例

对于字符串 "abcdefg".

    offset=0 => "abcdefg"
    offset=1 => "gabcdef"
    offset=2 => "fgabcde"
    offset=3 => "efgabcd"

挑战

    在数组上原地旋转，使用O(1)的额外空间

## 题解

三步翻转法即可

```python
class Solution:
    # @param s: a list of char
    # @param offset: an integer
    # @return: nothing
    def rotateString(self, s, offset):
        # write you code here
        if s is None or len(s) == 0:
            return s

        offset = offset % len(s)
        head = s[:len(s) - offset]
        tail = s[len(s) - offset:]
        # [::-1] means reverse in Python
        s = head[::-1] + tail[::-1]
        s = s[::-1]
        return s

```
附上 java 版本

```java
// java
public class Solution {
    /**
     * @param str: an array of char
     * @param offset: an integer
     * @return: nothing
     */
    public void rotateString(char[] str, int offset) {
        // write your code here
        if (str == null || str.length == 0) {
            return ;
        }

        int len = str.length;
        offset %= len;
        reverse(str, 0, len - offset - 1);
        reverse(str, len - offset, len - 1);
        reverse(str, 0, len - 1);

    }

    private void reverse(char[] str, int start, int end) {
        while (start < end) {
            char temp = str[start];
            str[start] = str[end];
            str[end] = temp;
            start++;
            end--;
        }
    }
}

```