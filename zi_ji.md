# 子集

给定一个含不同整数的集合，返回其所有的子集

样例

    如果 S = [1,2,3]，有如下的解：
    [
      [3],
      [1],
      [2],
      [1,2,3],
      [1,3],
      [2,3],
      [1,2],
      []
    ]

注意

    子集中的元素排列必须是非降序的，解集必须不包含重复的子集

## 题解

比较通常的做法是先排序然后回溯法递归,但是我觉得直接用循环更加简单粗暴

```java
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> S) {
        ArrayList<ArrayList<Integer>> pre = new ArrayList<ArrayList<Integer>>();
        pre.add(new ArrayList<Integer>());
        Collections.sort(S);
        for(Integer num : S){
            ArrayList<ArrayList<Integer>> next = new ArrayList<ArrayList<Integer>>();
            next.addAll(pre);
            for(ArrayList<Integer> subList : pre){
                ArrayList<Integer> cur = new ArrayList<Integer>(subList);
                cur.add(num);
                next.add(cur);
            }
            pre = next;
        }
        return pre;
    }
}

```

递归 

```java
public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> S) {
    // write your code here
    ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
    if(S == null || S.size() == 0)
        return res;
    Collections.sort(S);
    ArrayList<Integer> tmp = new ArrayList<Integer>();
    res.add(tmp);
    dfs(res, S, tmp, 0);
    return res;
}
public void dfs(ArrayList<ArrayList<Integer>> res, ArrayList<Integer> S, ArrayList<Integer> tmp, int pos) {
    for(int i = pos; i < S.size(); i++) {
        tmp.add(S.get(i));
        res.add(new ArrayList<Integer>(tmp));
        dfs(res, S, tmp, i+1);
        tmp.remove(tmp.size()-1);
    }
}
```