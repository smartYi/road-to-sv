# Permutations

Given a collection of numbers, return all possible permutations.

For example,

    [1,2,3] have the following permutations:
    [1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], and [3,2,1].
    
## Solution

dfs...

```java
public class Solution {
    public List<List<Integer>> permute(int[] num) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        List<Integer> path = new ArrayList<Integer>();
        boolean[] free = new boolean[num.length];
        Arrays.fill(free, true);
        permuteRe(num, res, path,free);
        return res;
    }
    
    void permuteRe(int[] num, List<List<Integer>> res, List<Integer> path, boolean[] free) {
        if(path.size() == num.length) {
            ArrayList<Integer> tmp = new ArrayList<Integer>(path);
            res.add(tmp);
            return;
        }
        for (int i = 0; i < num.length; ++i) {
            if (free[i] == true) {
                free[i] = false;
                path.add(num[i]);
                permuteRe(num, res, path, free);
                path.remove(path.size() - 1);
                free[i] = true;
            }
        }
    }
}
```