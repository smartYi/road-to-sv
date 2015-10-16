# Conversion

Write a function to determine the number of bits you would need to flip to convert integer A to integer B.

EXAMPLE

    Input: 29(11101), 15(01111)
    Output: 2

## Solution

```java
public static int conversion(int a, int b){
    int count = 0;
    int c = a ^ b;
    while (c != 0){
        if ((c & 1) == 1)
            count++;
        c = c >> 1;
    }
    return count;
}
// c = c & (c - 1) will clear the least significant bit in c
```