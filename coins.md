# Coins

Given an infinite number of quarters(25 cents), dimes(10 cents), nickels(5 cents), and pennies(1 cent), write code to calculate the number of ways of representing n cents.

## Solution

```java
// Use the solution from the book to help understanding
public static int makeChange(int amount, int[] cents, int index){
    if (index == cents.length - 1)
        return 1;
    int coin = cents[index];
    int ways = 0;
    for (int i = 0; i * coin <= amount; i++){
        int rest = amount - i * coin;
        ways += makeChange(rest, cents, index+1);
    }
    return ways;
}

public static int makeChange(int n){
    int[] cents = {25, 10, 5, 1};
    return makeChange(n, cents, 0);
}       
```