+ 难度：中等

给定一个整数，将其转换成罗马数字。

返回的结果要求在1-3999的范围内。

样例

    4 -> IV
    12 -> XII
    21 -> XXI
    99 -> XCIX

## 题解

注意需要处理 9xx 的情况，类似一样除即可。

```java
public class Solution {
    /**
     * @param n The integer
     * @return Roman representation
     */
    public String intToRoman(int n) {
        if(n <= 0) {
            return "";
        }
        int[] nums = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbols = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        StringBuilder res = new StringBuilder();
        int digit=0;
        while (n > 0) {
            int times = n / nums[digit];
            n -= nums[digit] * times;
            for ( ; times > 0; times--) {
                res.append(symbols[digit]);
            }
            digit++;
        }
        return res.toString();
    }
}

```