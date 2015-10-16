+ 难度：中等

求逆波兰表达式的值。

在逆波兰表达法中，其有效的运算符号包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰计数表达。

样例

    ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
    ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6

## 题解

利用栈就可以完美解决，每次弹出两个，如果有需要压回去一个

```java
public class Solution {
    /**
     * @param tokens The Reverse Polish Notation
     * @return the value
     */
    public int evalRPN(String[] tokens) {
        if (tokens == null) {
            return 0;
        }
        
        int len = tokens.length;
        Stack<Integer> s = new Stack<Integer>();
        
        for (int i = 0; i < len; i++) {
            String str = tokens[i];
            if (str.equals("+") || str.equals("-") || str.equals("*") || str.equals("/")) {
                // get out the two operation number.
                int n2 = s.pop();
                int n1 = s.pop();
                if (str.equals("+")) {
                    s.push(n1 + n2);
                } else if (str.equals("-")) {
                    s.push(n1 - n2);
                } else if (str.equals("*")) {
                    s.push(n1 * n2);
                } else if (str.equals("/")) {
                    s.push(n1 / n2);
                }
            } else {
                s.push(Integer.parseInt(str));
            }
        }
        
        if (s.isEmpty()) {
            return 0;
        }
        
        return s.pop();
    }
}

```