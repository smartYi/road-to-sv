# Permutations without Dups

Write a method to compute all permutations of a string of unique characters

## Solution

P(a1) = a1

P(a1a2) = a1a2, a2a1

P(a1a2a3) = a1a2a3, a1a3a2, a2a1a3, a2a3a1, a3a1a2, a3a2a1

从第二步到第三步，恩可以看作是，对第二步中的两个结果，分别把 a3 插入到的每个结果中可能的各个位置，于是我们可以根据这个规律写出代码

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