# String Rotation

Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, s1 and s2, write code to check if s2 is a rotation of s1 using only on call to isSubstring0

## Solution

```java
/**
 * If we imagine that s2 is a rotation of s1, then we can sk what the
 * rotation point is.
 *  s1 = xy = waterbottle
 *  x = wat
 *  y = erbottle
 *  s2 = yx = erbottlewat
 * so we need to check if there's a way to split s1 into x and y such that
 * xy = s1 and yx = s2. Regardless of where the division between x and y
 * is, we can see that yx will always be a substring of xyxy. That is, s2
 * will always be a substring of s1s1.
 */

boolean isRotation(String s1, String s2){
    int len = s1.length();
    if (len == s2.length() && len > 0){
        String s1s1 = s1 + s1;
        return isSubstring(s1s1, s2);
    }
    return false;
}


public static boolean isSubstring(String s1, String s2){
    // body is provided
    return true;
}
```