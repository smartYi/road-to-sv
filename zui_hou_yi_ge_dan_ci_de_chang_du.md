+ 难度：简单

给定一个字符串， 包含大小写字母、空格' '，请返回其最后一个单词的长度。

如果不存在最后一个单词，请返回 0 。

样例

    给定 s = "Hello World"，返回 5。

注意

    一个单词的界定是，由字母组成，但不包含任何的空格。

## 题解

就正常倒着一个一个数即可

```python
class Solution:
    # @param {string} s A string
    # @return {int} the length of last word
    def lengthOfLastWord(self, s):
        # Write your code here
        l = len(s)
        if l == 0:
            return 0
        ans = 0
        for i in range(1,l+1):
            if s[-i] != ' ':
                ans = ans + 1
            else:
                if ans > 0:
                    return ans
        return ans

```