# 两数之和

给一个整数数组，找到两个数使得他们的和等于一个给定的数target。

你需要实现的函数twoSum需要返回这两个数的下标, 并且第一个下标小于第二个下标。注意这里下标的范围是1到n，不是以0开头。

样例

    numbers=[2, 7, 11, 15],  target=9
    return [1, 2]

注意

    你可以假设只有一组答案。

## 题解

O(n) Space, O(n) Time solution:

Use a hashmap to store element of array as key, index as value.

Then use a for loop to check whether target - current exist in hashmap.
However, if element in the array is repeated, the hashmap has to hold repetitive keys.

So we keep comparing and storage at the same time and don't need to worry about repetitions.


```java
public class Solution {
    /*
     * @param numbers : An array of Integer
     * @param target : target = numbers[index1] + numbers[index2]
     * @return : [index1 + 1, index2 + 1] (index1 < index2)
     */
    public int[] twoSum(int[] numbers, int target) {
        if(numbers == null || numbers.length < 2) return null;
        int[] res = new int[2];
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        for(int i = 0; i < numbers.length; i++){
            if(map.containsKey(numbers[i])){
                res[0] = map.get(numbers[i]) + 1;
                res[1] = i+1;
            } else{
                map.put(target-numbers[i], i);
            }
        }
        return res;
    }
}

```