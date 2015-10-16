# Permutations without Dups

Write a method to compute all permutations of a string of unique characters

## Solution

```java
public static ArrayList<String> getPermutations(String str){
    if (str == null){
        return null;
    }

    ArrayList<String> permutations = new ArrayList<String>();
    if (str.length() == 0){
        permutations.add("");
        return permutations;
    }

    char first = str.charAt(0);
    String rest = str.substring(1);
    ArrayList<String> words = getPermutations(rest);
    for (String word : words){
        for (int j = 0; j <= word.length(); j++){
            permutations.add(word.substring(0,j) + first + word.substring(j));
        }
    }
    return permutations;
}
```