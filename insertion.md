# Insertion

You are given two 32-bit numbers, N and M, and two bit positions, i and j. Write a method to insert M int N such that M starts at bit j and ends at bit i. You can assume that the bits j through i have enough space to fit all of M. That is, if M = 10011, you can assume that there are at least 5 bits between j and i.

EXAMPLE

    Input: N = 10000000000, M = 10011, i = 2, j = 6
    Output: N = 10001001100
    
## Solution

```java
public static int insertion(int N, int M, int i, int j){
    M = M << i ;
    int mask = ~(1 << (j+1) - 1) | ~(1 << i - 1);
    N = N & mask;
    return M | N;
}
```