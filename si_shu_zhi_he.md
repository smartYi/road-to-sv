# 四数之和

给一个包含n个数的整数数组S，在S中找到所有使得和为给定整数target的四元组(a, b, c, d)。

样例

    例如，对于给定的整数数组S=[1, 0, -1, 0, -2, 2] 和 target=0. 满足要求的四元组集合为：
    (-1, 0, 0, 1)
    (-2, -1, 1, 2)
    (-2, 0, 0, 2)

注意

    四元组(a, b, c, d)中，需要满足a <= b <= c <= d
    答案中不可以包含重复的四元组。

## 题解

```java
public class Solution {
    /**
     * @param numbers : Give an array numbersbers of n integer
     * @param target : you need to find four elements that's sum of target
     * @return : Find all unique quadruplets in the array which gives the sum of
     *           zero.
     */
    public ArrayList<ArrayList<Integer>> fourSum(int[] numbers, int target) {   ArrayList<ArrayList<Integer>> res = new ArrayList<ArrayList<Integer>>();
        if(numbers == null || numbers.length <= 3) return res;
        Arrays.sort(numbers);
        for(int i = 0; i < numbers.length - 3; i++){
            if(i > 0 && numbers[i] == numbers[i-1])
                continue;
            for(int j = i + 1; j < numbers.length - 2; j++){
                if(j > i + 1 && numbers[j] == numbers[j-1])
                    continue;
                int h = j + 1, k = numbers.length - 1;
                while(h < k){
                    if(h > j + 1 && numbers[h] == numbers[h-1]){
                        h++;
                        continue;
                    }
                    int sum = numbers[i] + numbers[j] + + numbers[h] + numbers[k];
                    if(sum == target){
                        ArrayList<Integer> sub = new ArrayList<Integer>();
                        sub.add(numbers[i]);
                        sub.add(numbers[j]);
                        sub.add(numbers[h]);
                        sub.add(numbers[k]);
                        res.add(sub);
                        h++; k--;
                    } else if(sum > target) k--;
                    else h++;
                }
            }
        }
        return res;
    }
}


```