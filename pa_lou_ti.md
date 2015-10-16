+ 难度：简单

假设你正在爬楼梯，需要n步你才能到达顶部。但每次你只能爬一步或者两步，你能有多少种不同的方法爬到楼顶部？

样例

    比如 n=3，1+1+1=1+2=2+1=3，共有3中不同的方法
    返回 3

**题解**

可以用迭代或者递归，我个人比较喜欢迭代，简单粗暴，就是到达第 n 级台阶的方法数目等于 第 n-1 级加上第 n-2 级，循环即可

这里也可以只用两个临时变量，进一步降低空间复杂度

```python
class Solution:
    """
    @param n: An integer
    @return: An integer
    """
    def climbStairs(self, n):
        ans = [1,2]
        for i in range(2,n):
            c = ans[i-2] + ans[i-1]
            ans.append(c)
        return ans[n-1]

```