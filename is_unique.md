# Is Unique

Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

Detail: ASCII string or Unicode string

## Solution

详情见代码注释

```java
/**
* The first solution comes with an array to record the times that a letter
* has shown up. If the value of any elements in the array exceeds 1, then
* we known that it is not unique.
*/
public static boolean isUniqueHashTable(String testStr){
    int[] alphebet = new int[256];
    String str = testStr.toLowerCase();
    for (int i = 0; i < testStr.length(); i++){
        alphebet[str.charAt(i)]++;
        if (alphebet[str.charAt(i)] > 1){
            return false;
        }
    }
    return true;
}

/**
* If it is impossible to use extra data structure, the hash table method
* will be unavailable. It is possible to use only a bit vector to represent
* the appearance of the characters. But it needs some intinct about bit
* manipulation techniques
*/
public static boolean isUniqueNoExtraDataStructure(String testStr){
    int checker = 0;
    String str = testStr.toLowerCase();
    for (int i = 0; i < str.length(); i++){
        int val = str.charAt(i) - 'a';
        if ((checker & (1 << val)) > 0){
            return false;
        }
        checker |= (1 << val);
    }
    return true;
}
```