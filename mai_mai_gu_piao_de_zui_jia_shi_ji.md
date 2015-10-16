+ 难度：中等

假设有一个数组，它的第i个元素是一支给定的股票在第i天的价格。如果你最多只允许完成一次交易(例如,一次买卖股票),设计一个算法来找出最大利润。

样例

    给出一个数组样例 [3,2,3,1,2], 返回 1

## 题解

keep updating the smallest value and the max difference.

```java
public class Solution {
    /**
     * @param prices: Given an integer array
     * @return: Maximum profit
     */
    public int maxProfit(int[] prices) {
        if (prices.length == 0) return 0;
        int min = prices[0];
        int res = 0;
        for (int i=1; i<prices.length; i++){
            if (prices[i] < min) min = prices[i];
            else if ((prices[i]-min) > res){
                res = (prices[i]-min);
            }
        }
        return res;
    }
}

```
