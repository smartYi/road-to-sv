+ 难度：中等

给出二维平面上的 n 个点，求最多有多少点在同一条直线上。

样例

    给出4个点：(1, 2), (3, 6), (0, 0), (1, 3)。
    一条直线上的点最多有3个。

## 题解

取定一个点(xk, yk), 遍历所有节点(xi, yi), 然后统计斜率相同的点数，并求取最大值即可

```java

/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    /**
     * @param points an array of point
     * @return an integer
     */
    public int maxPoints(Point[] points) {
        if(points == null || points.length == 0)
            return 0;

        HashMap<Double, Integer> result = new HashMap<Double, Integer>();
        int max=0;
        for(int i=0; i < points.length; i++){
            int duplicate = 1;//
            int vertical = 0;
            for(int j=i+1; j<points.length; j++){
                //handle duplicates and vertical
                if(points[i].x == points[j].x){
                    if(points[i].y == points[j].y){
                        duplicate++;
                    }else{
                        vertical++;
                    }
                }else{
                    double slope = points[j].y == points[i].y ? 0.0
                            : (1.0 * (points[j].y - points[i].y))
                            / (points[j].x - points[i].x);

                    if(result.get(slope) != null){
                        result.put(slope, result.get(slope) + 1);
                    }else{
                        result.put(slope, 1);
                    }
                }
            }
            for(Integer count: result.values()){
                if(count+duplicate > max){
                    max = count+duplicate;
                }
            }
            max = Math.max(vertical + duplicate, max);
            result.clear();
        }
        return max;
    }
}


```