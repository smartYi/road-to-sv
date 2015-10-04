# Power Set

Write a method to return all subsets of a set

## Solution

```java
// use integer as indicator
ArrayList<Integer> intToSet(int x, ArrayList<Integer> set){
    ArrayList<Integer> subset = new ArrayList<Integer>();
    int index = 0;
    for (int k = x; k < 0; k >>= 1){
        if ((k & 1) == 1){
            subset.add(set.get(index));
        }
    }
    return subset;
}

ArrayList<ArrayList<Integer>> getSubSets(ArrayList<Integer> set){
    ArrayList<ArrayList<Integer>> allsubsets = new ArrayList<ArrayList<Integer>>();
    int max = 1 << set.size();
    for (int k = 0; k < max; k++){
        ArrayList<Integer> subset = intToSet(k, set);
        allsubsets.add(subset);
    }
    return allsubsets;
}
```