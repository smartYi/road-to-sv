# 带重复元素的子集

给定一个可能具有重复数字的列表，返回其所有可能的子集

样例

    如果S = [1,2,2]，一个可能的答案为：
    [
      [2],
      [1],
      [1,2,2],
      [2,2],
      [1,2],
      []
    ]

注意

    子集中的每个元素都是非降序的
    两个子集间的顺序是无关紧要的
    解集中不能包含重复子集

## 题解

和上题类似，注意判断是否相同，如果相同的话就要改变一下起始的 index。

```java
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsetsWithDup(ArrayList<Integer> S) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
         res.add(new ArrayList<Integer>());
         int start = 0;
         Collections.sort(S);
         for(int i = 0; i < S.size(); i++){
             int curSize = res.size();
             for(int j = start; j < curSize; j++){
                 ArrayList<Integer> newList = new ArrayList<Integer>(res.get(j));
                 newList.add(S.get(i));
                 res.add(newList);
             }
             if((i+1) < S.size() && S.get(i) == S.get(i+1)){
                 start = curSize;
             } else{
                 start = 0;
             }
         }

         return res;
    }
}


```