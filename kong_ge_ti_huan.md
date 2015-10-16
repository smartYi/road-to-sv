+ 难度：简单

设计一种方法，将一个字符串中的所有空格替换成 %20 。你可以假设该字符串有足够的空间来加入新的字符，且你得到的是“真实的”字符长度。

样例

    对于字符串"Mr John Smith", 长度为 13
    替换空格之后的结果为"Mr%20John%20Smith"

注意

    如果使用 Java 或 Python, 程序中请用字符数组表示字符串。

挑战

    在原字符串(字符数组)中完成替换，不使用额外空间

## 题解

主要就是数组操作，注意即可

```python
class Solution:
    # @param {char[]} string: An array of Char
    # @param {int} length: The true length of the string
    # @return {int} The true length of new string
    def replaceBlank(self, string, length):
        # Write your code here
        i = 0
        while i < length:
            if string[i] == " ":
                string.append("")
                string.append("")
                length += 2
                for j in xrange(length-1, i, -1):
                    string[j] = string[j-2]

                string[i:i+3] = list("%20")
                i += 2
            i += 1

        return length

```