# Flip Bit to Win

You have an integer and you can flip exact one bit from a 0 to a 1. Write code to find the length of the longest sequence of 1s you could create

EXAMPLE

    Input: 1775 (11011101111)
    Output: 8
    
## Solution

```java
public static int flipBit(int n){
    int longest1s = 0;

    for (int i = 0; i < 32; i++){
        int temp = getlongest1s(n, i);
        if (longest1s < temp){
            longest1s = temp;
        }
    }
    return longest1s;
}

public static int getlongest1s(int n, int index){
    int longest = 0;
    int count = 0;
    for (int i = 0; i < 32; i++){
        if (i == index | getBit(n, i)){
            count++;
            if (count > longest){
                longest = count;
            }
        }
        else{
            count = 0;
        }
    }
    return longest;
}

public static boolean getBit(int num, int i){
    return ((num & (1 << i)) != 0);
}
```