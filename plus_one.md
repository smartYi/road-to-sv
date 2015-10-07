# Plus One

Given a non-negative number represented as an array of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

## Solution

```java
public class Solution {
    public int[] plusOne(int[] digits) {
        if (digits.length == 0) return digits;
        int carry = 1;
        for (int i = digits.length - 1; i >= 0; --i) {
            digits[i] += carry;
            carry = digits[i] / 10;
            digits[i] = digits[i] % 10;
        }
        if (carry == 0) return digits;
        int[] res = new int[digits.length + 1];
        res[0] = carry;
        System.arraycopy(digits, 0, res, 1, digits.length);
        return res;
    }
}
```