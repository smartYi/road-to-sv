# 三数之和

给出一个有n个整数的数组S，在S中找到三个整数a, b, c，找到所有使得a + b + c = 0的三元组。

样例

    如S = {-1 0 1 2 -1 -4}, 你需要返回的三元组集合的是：
    (-1, 0, 1)
    (-1, -1, 2)

注意

    在三元组(a, b, c)，要求a <= b <= c。
    结果不能包含重复的三元组。

## 题解

brute force时间复杂度为O(n^3), 对每三个数进行比较。这道题和Two Sum有所不同，使用哈希表的解法并不是很方便，因为结果数组中元素可能重复，如果不排序对于重复的处理将会比较麻烦，因此这道题一般使用排序之后夹逼的方法，总的时间复杂度为O(n^2+nlogn)=(n^2),空间复杂度是O(n),Two Sum的时间复杂度是O(n+nlogn)

困难点主要在于去重

排序后每一次假设把nums[i]作为第一个数字，在剩下的数字取两个

```java
public class Solution {
    /**
     * @param numbers : Give an array numbers of n integer
     * @return : Find all unique triplets in the array which gives the sum of zero.
     */
    public ArrayList<ArrayList<Integer>> threeSum(int[] numbers) {
        if(numbers == null || numbers.length <= 2) return null;
        ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        Arrays.sort(numbers);
        for(int i = 0; i < numbers.length - 2; i++){
            if(i > 0 && numbers[i] == numbers[i-1])
                continue;
            int j = i + 1, k = numbers.length - 1;
            while(j < k){
                if(j > i + 1 && numbers[j] == numbers[j-1]){
                    j++;
                    continue;
                }
                if(numbers[i] + numbers[j] + numbers[k] == 0){
                    ArrayList<Integer> sub = new ArrayList<Integer>();
                    sub.add(numbers[i]);
                    sub.add(numbers[j]);
                    sub.add(numbers[k]);
                    res.add(sub);
                    j++; k--;
                } else if(numbers[i] + numbers[j] + numbers[k] > 0) k--;
                else j++;
            }
        }
        return res;
    }
}

```