+ 难度：简单

跟进“删除重复数字”：

如果可以允许出现两次重复将如何处理？

样例

    给出数组A =[1,1,1,2,2,3]，你的函数应该返回长度5，此时A=[1,1,2,2,3]

## 题解

就是需要多加一个计数器，以及状态改变之后的值恢复，其他的和上面那题差不多，注意用 `len(A)` 的时候需要 `-1`才是正确的下标值


```python
class Solution:
    """
    @param A: a list of integers
    @return an integer
    """
    def removeDuplicates(self, A):
        # write your code here
        count = 0
        dc = 0
        for i in range(len(A)-1):
            if count == 0:
                if A[i-dc] == A[i-dc+1]:
                    count = count + 1
            elif count == 1:
                if A[i-dc] == A[i-dc+1]:
                    A.remove(A[i-dc])
                    dc = dc + 1
                else:
                    count = 0
        return len(A)

```

核心思想仍然是两根指针，只不过此时新索引自增的条件是当前遍历的数组值和『新索引』或者『新索引-1』两者之一不同。

```java
// java
public class Solution {
    /**
     * @param A: a array of integers
     * @return : return an integer
     */
    public int removeDuplicates(int[] nums) {
        if (nums == null) return -1;
        if (nums.length <= 2) return nums.length;

        int newIndex = 1;
        for (int i = 2; i < nums.length; i++) {
            if (nums[i] != nums[newIndex] || nums[i] != nums[newIndex - 1]) {
                newIndex++;
                nums[newIndex] = nums[i];
            }
        }

        return newIndex + 1;
    }
}
```

遍历数组时 i 从2开始，newIndex 初始化为1便于分析。