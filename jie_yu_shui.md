+ 难度：中等

给出 n 个非负整数，代表一张X轴上每个区域宽度为 1 的海拔图, 计算这个海拔图最多能接住多少（面积）雨水。

样例

    如上图所示，海拔分别为 [0,1,0,2,1,0,1,3,2,1,2,1], 返回 6.

挑战

    O(n) 时间, O(1) 空间
    O(n) 时间, O(n) 空间也可以接受

## 题解

对于任何一个坐标，检查其左右的最大坐标，然后相减就是容积。所以，

1. 从左往右扫描一遍，对于每一个坐标，求取左边最大值。
2. 从右往左扫描一遍，对于每一个坐标，求最大右值。
3. 再扫描一遍，求取容积并加和。

```java
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: a integer
     */
    public int trapRainWater(int[] heights) {
        if(heights.length < 3) return 0;
        int max = heights[0];
        int[] left = new int[heights.length];
        int[] right = new int[heights.length];
        int sum = 0;

        for(int i = 1; i < heights.length-1; i++){
            left[i] = max;
            if(heights[i] > max){
                max = heights[i];
            }
        }
        max = heights[heights.length-1];
        for(int i = heights.length-2; i >=1; i--){
            right[i] = max;
            if(heights[i] > max){
                max = heights[i];
            }
        }
        for(int i = 1; i < heights.length-1; i++){
            int cur = Math.min(left[i], right[i]) - heights[i];
            if(cur > 0){
                sum += cur;
            }
        }
        return sum;
    }
}

```