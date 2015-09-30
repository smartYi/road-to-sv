+ 难度：中等

给定 n 个非负整数 a1, a2, ..., an, 每个数代表了坐标中的一个点 (i, ai)。画 n 条垂直线，使得 i 垂直线的两个端点分别为(i, ai)和(i, 0)。找到两条线，使得其与 x 轴共同构成一个容器，以容纳最多水。

样例

    给出[1,3,2], 最大的储水面积是2.

注意

    容器不可倾斜。

## 题解

思路是从两头到中间扫描，设i,j分别指向height数组的首尾。

那么当前的area是min(height[i],height[j]) * (j-i)。

当height[i] < height[j]的时候，我们把i往后移，否则把j往前移，直到两者相遇。
这个正确性如何证明呢？

代码里面的注释说得比较清楚了，即每一步操作都能保证当前位置能取得的最大面积已经记录过了，而最开始初始化的时候最大面积记录过，所以有点类似于数学归纳法，证明这个算法是正确的。

```java
public class Solution {
    /**
     * @param heights: an array of integers
     * @return: an integer
     */
    public int maxArea(int[] heights) {
        int length = heights.length;
        if (length< 2) return 0;
        int i = 0, j = length - 1;
        int maxarea = 0;
        while(i < j) {
            int area = 0;
            if(heights[i] < heights[j]) {
                area = heights[i] * (j-i);
                //Since i is lower than j,
                //so there will be no jj < j that make the area from i,jj
                //is greater than area from i,j
                //so the maximum area that can benefit from i is already recorded.
                //thus, we move i forward.
                //因为i是短板，所以如果无论j往前移动到什么位置，都不可能产生比area更大的面积
                //换句话所，i能形成的最大面积已经找到了，所以可以将i向前移。
                ++i;
            } else {
                area = heights[j] * (j-i);
                //the same reason as above
                //同理
                --j;
            }
            if(maxarea < area) maxarea = area;
        }
        return maxarea;
    }
}

```