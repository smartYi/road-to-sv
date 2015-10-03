# URLify

Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space at the end to hold the additional characters, and that you are given the "true" length of the string.

Note

If implementing in Java, please use a character array so that you can perform this operation in place.

Example

    Input:  "Mr John Smith    ", 13
    Output: "Mr%20John%20Smith"

## Solution

```java
/**
 * The algorithm is simple
 * First pass - count the number of spaces
 * Second pass - From end to beginning, finish the urlify process
 */
public static String urlify(String oriStr, int num){
    char[] str = new char[oriStr.length()];
    int spaceCount = 0;
    // 1st pass
    for (int i = 0; i < num; i++){
        if (oriStr.charAt(i) == ' '){
            spaceCount++;
        }
    }

    // 2nd pass
    // pay attention to the conversion from string to char.
    int newLength = num + 2 * spaceCount;
    for (int i = num - 1; i >= 0; i--){
        if (oriStr.charAt(i) == ' '){
            str[newLength - 1] = '0';
            str[newLength - 2] = '2';
            str[newLength - 3] = '%';
            newLength -= 3;
        }
        else{
            str[newLength - 1] = oriStr.charAt(i);
            newLength--;
        }
    }

    return String.valueOf(str);
}
```