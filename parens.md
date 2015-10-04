# Parens

Implement an algorithm to print all valid (i.e., properly opened and closed) combinations of n pairs of parentheses.

## Solution

```java
/**
 * Left Paren: As long as we haven't used up all the left parentheses, we
 * can always insert a left paren.
 *
 * Right Paren: We will get a syntax error if there are more right
 * parentheses than left
 */
// Use the solution from the book to help understanding
public static void addParen(ArrayList<String> list, int leftRem,
                            int rightRem, char[] str, int count){
    if (leftRem < 0 || rightRem < leftRem){
        return;
    }

    if (leftRem == 0 && rightRem == 0){
        list.add(String.copyValueOf(str));
    }
    else {
        if (leftRem > 0){
            str[count] = '(';
            addParen(list, leftRem - 1, rightRem, str, count + 1);
        }

        if (rightRem > leftRem){
            str[count] = ')';
            addParen(list, leftRem, rightRem - 1, str, count + 1);
        }
    }
}

public static ArrayList<String> generateParens(int count){
    char[] str = new  char[count*2];
    ArrayList<String> list = new ArrayList<String>();
    addParen(list, count, count, str, 0);
    return list;
}
```