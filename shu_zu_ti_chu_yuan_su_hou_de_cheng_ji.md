+ 难度：简单

给定一个整数数组A。

定义`B[i] = A[0] * ... * A[i-1] * A[i+1] * ... * A[n-1]`， 计算B的时候请不要使用除法。

样例

    给出A=[1, 2, 3]，返回 B为[6, 3, 2]

## 题解

左右分治法

完全可以在最终返回结果result基础上原地计算左右两半部分的积。

```java
public class Solution {
    /**
     * @param A: Given an integers array A
     * @return: A Long array B and B[i]= A[0] * ... * A[i-1] * A[i+1] * ... * A[n-1]
     */
    public ArrayList<Long> productExcludeItself(ArrayList<Integer> A) {
        ArrayList<Long> result = new ArrayList<Long>();
        if (A == null || A.size() == 0) {
            return result;
        }
        //leftToI = A[0] * ... * A[i-1]
        long leftToI = 1;
        result.add(leftToI);
        for (int i = 1; i < A.size(); i++) {
            leftToI *= A.get(i-1);
            result.add(leftToI);
        }
        //rightToI = A[i-1] * A[i+1] * ... * A[n-1]
        long rightToI = 1;
        result.set(A.size() - 1, result.get(A.size() - 1) * rightToI);
        for (int i = A.size()-2; i >= 0; i--) {
            rightToI *= A.get(i+1);
            result.set(i, result.get(i) * rightToI);
        }
        return result;
    }
}


```