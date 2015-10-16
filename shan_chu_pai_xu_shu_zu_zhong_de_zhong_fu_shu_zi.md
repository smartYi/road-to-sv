+ 难度：简单

给定一个排序数组，在原数组中删除重复出现的数字，使得每个元素只出现一次，并且返回新的数组的长度。

不要使用额外的数组空间，必须在原地没有额外空间的条件下完成。

样例

给出数组A =[1,1,2]，你的函数应该返回长度2，此时A=[1,2]。

## 题解

与上题类似，如果需要删除的话则需要给删除计数，然后对应即可，注意下标的正确性

```python
# python
class Solution:
    """
    @param A: a list of integers
    @return an integer
    """
    def removeDuplicates(self, A):
        # write your code here
        dc = 0
        for i in range(len(A)-1):
            if A[i-dc] == A[i-dc+1]:
                A.remove(A[i-dc])
                dc = dc + 1
        return len(A)
```

```java
//java
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
        if (nums == null) return -1;
        if (nums.length <= 1) return nums.length;

        int newIndex = 0;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[newIndex]) {
                newIndex++;
                nums[newIndex] = nums[i];
            }
        }

        return newIndex + 1;
    }
}
```

注意最后需要返回的是索引值加1。