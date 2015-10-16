+ 难度：简单

字符串查找（又称查找子字符串），是字符串操作中一个很有用的函数。你的任务是实现这个函数。

对于一个给定的 source 字符串和一个 target 字符串，你应该在 source 字符串中找出 target 字符串出现的第一个位置(从0开始)。

如果不存在，则返回 -1。

样例

    如果 source = "source" 和 target = "target"，返回 -1。
    如果 source = "abcdabcdefg" 和 target = "bcd"，返回 1。

挑战

    O(n2)的算法是可以接受的。如果你能用O(n)的算法做出来那更加好。（提示：KMP）

说明

    在面试中我是否需要实现KMP算法？

不需要，当这种问题出现在面试中时，面试官很可能只是想要测试一下你的基础应用能力。当然你需要先跟面试官确认清楚要怎么实现这个题。

## 题解

O(n2) 方法需要注意边界条件

```python
class Solution:
    def strStr(self, source, target):
        # write your code here
        index = -1
        if source is None or target is None:
            return -1
        if source == target:
            return 0
        if target == "":
            return 0

        for i in range(len(source)-len(target)):
            for j in range(len(target)):
                if source[i+j] == target[j]:
                    index = i
                else:
                    index = -1
                    break
            if index != -1:
                break
        return index

```