+ 难度：简单

实现一个算法确定字符串中的字符是否均唯一出现

样例

    给出"abc"，返回 true
    给出"aab"，返回 false

挑战

    如果不使用额外的存储空间，你的算法该如何改变？

## 题解

用额外空间的话，一个 hash 表就可以快速完成，如果不用的话，那就只能简单粗暴的枚举找一次了

```python
class Solution:
    # @param s: a string
    # @return: a boolean
    def isUnique(self, str):
        # write your code here
        for i in range(len(str)-1):
            for j in range(i+1, len(str)-1):
                if str[i] == str[j]:
                    return False
        return True
```