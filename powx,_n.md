# Pow(x, n)

Implement pow(x, n).

## Solution

recursion.

```java
public class Solution {
    public double myPow(double x, int n) {
        if (x < 0) return (n % 2 == 0) ? myPow(-x, n) : -myPow(-x, n);
        if (x == 0 || x == 1) return x;
        if (n < 0) return 1.0 / myPow(x,-n);
        if (n == 0) return 1.0;
        if (n == 1) return x;
        double half = myPow(x,n/2);
        if (n % 2 == 0) return half * half;
        else return x * half * half;
    }
}
```