# Power of Two

Given an integer, write a function to determine if it is a power of two.

## Solution

注意判断 n > 0

```java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        return  (n > 0) && (n & (n-1)) == 0;
    }
}
```