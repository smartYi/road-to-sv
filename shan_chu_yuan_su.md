+ 难度：简单

定一个数组和一个值，在原地删除与值相同的数字，返回新数组的长度。

元素的顺序可以改变，并且对新的数组不会有影响。

样例

    给出一个数组 [0,4,4,0,0,2,4,4]，和值 4
    返回 4 并且4个元素的新数组为[0,0,0,2]

## 题解

利用 python 的特性可以快速删除，注意下标的范围会变化，所以找一个值来记录之后遍历时对应到正确的元素即可

```python
class Solution:
    """
    @param A: A list of integers
    @param elem: An integer
    @return: The new length after remove
    """
    def removeElement(self, A, elem):
        dc = 0
        for i in range(len(A)):
            if A[i-dc] == elem:
                A.remove(elem)
                dc = dc + 1
        return A

```

C++ 版本1 - 使用容器

遍历容器内元素，若元素值与给定删除值相等，删除当前元素并往后继续遍历。

```cpp
class Solution {
public:
    /**
     *@param A: A list of integers
     *@param elem: An integer
     *@return: The new length after remove
     */
    int removeElement(vector<int> &A, int elem) {
        // write your code here
        for (vector<int>::iterator iter = A.begin(); iter < A.end(); ++iter){
            if (*iter == elem){
                iter = A.erase(iter);
                --iter;
            }
        }
        return A.size();
    }
};

```

注意在遍历容器内元素和指定欲删除值相等时，需要先自减`--iter`, 因为`for`循环会对`iter`自增，`A.erase()`删除当前元素值并返回指向下一个元素的指针，一增一减正好平衡。如果改用`while`循环，则需注意访问数组时是否越界。

C++ 版本2 - 两根指针

由于题中明确暗示元素的顺序可变，且新长度后的元素不用理会。我们可以使用两根指针分别往前往后遍历，头指针用于指示当前遍历的元素位置，尾指针则用于在当前元素与欲删除值相等时替换当前元素，两根指针相遇时返回尾指针索引——即删除元素后「新数组」的长度。

```cpp
public:
    int removeElement(int A[], int n, int elem) {
        for (int i = 0; i < n; ++i) {
            if (A[i] == elem) {
                A[i] = A[n - 1];
                --i;
                --n;
            }
        }

        return n;
    }
};
```
遍历当前数组，A[i] == elem时将数组「尾部(以 n 为长度时的尾部)」元素赋给当前遍历的元素。同时自减i和n，原因见题解1的分析。需要注意的是n在遍历过程中可能会变化。