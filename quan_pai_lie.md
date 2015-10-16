# 全排列

给定一个数字列表，返回其所有可能的排列。

样例

    给出一个列表[1,2,3]，其全排列为：
    [
      [1,2,3],
      [1,3,2],
      [2,1,3],
      [2,3,1],
      [3,1,2],
      [3,2,1]
    ]

挑战

    能否不使用递归来实现？

## 题解

```java
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public ArrayList<ArrayList<Integer>> permute(ArrayList<Integer> nums) {
        ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();
        if (nums == null || nums.size() == 0) {
            return result;
        }

        //start from an empty list
        result.add(new ArrayList<Integer>());

        //add nums[i] to all positions of each list in the current result => new result
        for (int i = 0; i < nums.size(); i++) {
            ArrayList<ArrayList<Integer>> nextResult = new ArrayList<ArrayList<Integer>>();

            //for each list l in the result
            for (ArrayList<Integer> l : result) {
                // insert num[i] from 0 to l.size()
                for (int j = 0; j < l.size() + 1; j++) {
                    l.add(j, nums.get(i));
                    ArrayList<Integer> temp = new ArrayList<Integer>(l);
                    nextResult.add(temp); //add the new list to the next result.
                    l.remove(j);
                }
            }
            result = nextResult;
        }
        return result;
    }
}


```