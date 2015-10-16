+ 难度：中等

给出一组非负整数，重新排列他们的顺序把他们组成一个最大的整数

样例

    给出样例 [1, 20, 23, 4, 8]，返回组合最大的整数为8423201

注意

    最后的结果可能很大，所以我们返回一个字符串来代替这个整数

## 题解

Override comparator:  put the larger number ahead of smaller number.

```java
public class Solution {
    /**
     *@param num: A list of non negative integers
     *@return: A string
     */
    public String largestNumber(int[] num) {
        if (num == null || num.length == 0) {
            return "0";
        }

        String[] strs = new String[num.length];
        for (int i = 0; i < num.length; i++) {
            strs[i] = Integer.toString(num[i]);
        }

        Arrays.sort(strs, new NumberComparator());

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < strs.length; i++) {
            sb.append(strs[i]);
        }
        String res = sb.toString();

        if(res.charAt(0) == '0') {
            return "0";
        }

        return res;
    }


    class NumberComparator implements Comparator<String> {
        @Override
        public int compare(String s1, String s2){
            return (s2 + s1).compareTo(s1 + s2);
        }
    }
}

```