+ 难度：简单

如果要将整数A转换为B，需要改变多少个bit位？

样例

    如把31转换为14，需要改变2个bit位。
    (31)10=(11111)2
    (14)10=(01110)2

挑战

    你能想出几种方法？

## 题解


```java
class Solution {
    /**
     *@param a, b: Two integer
     *return: An integer
     */
    public static int bitSwapRequired(int a, int b) {
        int result = a^b;
        int counter = 0;
        while (result != 0) {
            result = result & (result - 1);
            counter++;
        }
        return counter;
    }
};

```