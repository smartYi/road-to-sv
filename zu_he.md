+ 难度：中等

给出两个整数n和k，返回从1......n中选出的k个数的组合。

样例

    例如 n = 4 且 k = 2
    返回的解为：
    [[2,4],[3,4],[2,3],[1,2],[1,3],[1,4]]

## 题解

利用递归处理，标准流程，加了再删掉

```java
public class Solution {
    /**
     * @param n: Given the range of numbers
     * @param k: Given the numbers of combinations
     * @return: All the combinations of k numbers out of 1..n
     */
    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        List<Integer> list = new ArrayList<Integer>();
        if (n <= 0 || k <= 0) return result;

        helper(n, k, 1, list, result);
        return result;
    }

    private void helper(int n, int k, int pos,
                        List<Integer> list, List<List<Integer>> result) {

        if (list.size() == k) {
            result.add(new ArrayList<Integer>(list));
            return;
        }

        for (int i = pos; i <= n; i++) {
            list.add(i);
            helper(n, k, i + 1, list, result);
            list.remove(list.size() - 1);
        }
    }
}

```