# Subsets II

Given a collection of integers that might contain duplicates, nums, return all possible subsets.

Note:

+ Elements in a subset must be in non-descending order.
+ The solution set must not contain duplicate subsets.

For example,

If nums = [1,2,2], a solution is:

    [
      [2],
      [1],
      [1,2,2],
      [2,2],
      [1,2],
      []
    ]
    
## Solution

注意起始的下标

```java
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] S) {
        Arrays.sort(S);
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        res.add(new ArrayList<Integer>());
        int presz = 0;
        for (int i = 0; i < S.length; ++i) {
            int sz = res.size();
            for (int j = 0; j < sz; ++j) {
                if (i == 0 || S[i] != S[i-1] || j >= presz) {
                    List<Integer> path = new ArrayList<Integer>(res.get(j));
                    path.add(S[i]);
                    res.add(path);
                }
            }
            presz = sz;
        }
        return res;
    }
}
```