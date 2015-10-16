+ 难度：简单

给定一个整数数组，找到和为零的子数组。你的代码应该返回满足要求的子数组的起始位置和结束位置

样例

    给出[-3, 1, 2, -3, 4]，返回[0, 2] 或者 [1, 3].

## 题解

哈希表

题目中的对象是分析子串和，那么我们先从常见的对数组求和出发，
f(i) 表示从数组下标 0 开始至下标 i 的和。子串和为0，也就意味着存在不同的 i1 和 i2 使得 f(i1) - f(i2) = 0 即 f(i1) = f(i2)。思路很快就明晰了，使用一 vector 保存数组中从 0 开始到索引i的和，在将值 push 进 vector 之前先检查 vector 中是否已经存在，若存在则将相应索引加入最终结果并返回。

```cpp
class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number
     *          and the index of the last number
     */
    vector<int> subarraySum(vector<int> nums){
        vector<int> results;
        // curr_cum for the first item, index for the second item
        map<int, int> hash;
        hash[0] = 0;

        int curr_sum = 0;
        for (int i = 0; i != nums.size(); ++i){
            curr_sum += nums[i];
            if (hash.find(curr_sum) != hash.end()){
                results.push_back(hash[curr_sum]);
                results.push_back(i);
                return results;
            } else {
                hash[curr_sum] = i + 1;
            }
        }

        return results;
    }
};

```