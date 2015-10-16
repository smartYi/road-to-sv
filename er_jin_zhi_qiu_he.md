+ 难度：简单

给定两个二进制字符串，返回他们的和（用二进制表示）。

样例

    a = 11
    b = 1
    返回 100

## 题解

简单粗暴，对着位置一个一个加，注意进位即可

```java
public class Solution {
    /**
     * @param a a number
     * @param b a number
     * @return the result
     */
    public String addBinary(String a, String b) {
        int length = a.length() < b.length() ? a.length() : b.length();
        StringBuffer buffer = new StringBuffer();
        int carry = 0;
        for(int i = 0; i < length; i++){
            int aBit = a.charAt(a.length() - 1 - i) - '0';
            int bBit = b.charAt(b.length() - 1 - i) - '0';
            buffer.insert(0, (aBit+bBit+carry)%2);
            carry = (aBit+bBit+carry)/2;
        }
        String res = null;
        if(a.length() != b.length()){
            res = a.length() >  b.length() ? a : b;
        } 
        if(res != null){
            for(int i = res.length() - length - 1; i >= 0; i--){
                buffer.insert(0, (res.charAt(i) - '0' + carry) % 2);
                carry = (res.charAt(i) - '0' + carry) / 2;
            }
        }
        if(carry == 1) buffer.insert(0, carry);
        return buffer.toString();
    }
}
```