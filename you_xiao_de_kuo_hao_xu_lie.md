给定一个字符串所表示的括号序列，包含以下字符： '(', ')', '{', '}', '[' and ']'， 判定是否是有效的括号序列。

样例

    括号必须依照 "()" 顺序表示， "()[]{}" 是有效的括号，但 "([)]"则是无效的括号。

挑战

    O(n)的时间，n为括号的个数

## 题解

用一个 stack（或者自己模拟）即可

```python
class Solution:
    # @param {string} s A string
    # @return {boolean} whether the string is a valid parentheses
    def isValidParentheses(self, s):
        # Write your code here
        ans = []
        for i in range(len(s)):
            if s[i] == '(' or s[i] == '[' or s[i] == '{':
                ans.append(s[i])
            else:
                if len(ans) < 1:
                    return False
                cur = ans.pop()
                if s[i] == ')' and cur != '(':
                    return False
                if s[i] == ']' and cur != '[':
                    return False
                if s[i] == '}' and cur != '{':
                    return False
        if len(ans) < 1:
            return True
        else:
            return False

```