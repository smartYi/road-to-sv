# Median of Two Sorted Arrays

There are two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively. Find the median of the two sorted arrays. The overall run time complexity should be `O(log (m+n))`.

## Solution

将原问题转变成一个寻找第k小数的问题（假设两个原序列升序排列），这样中位数实际上是第(m+n)/2小的数。所以只要解决了第k小数的问题，原问题也得以解决。

首先假设数组A和B的元素个数都大于k/2，我们比较A[k/2-1]和B[k/2-1]两个元素，这两个元素分别表示A的第k/2小的元素和B的第k/2小的元素。这两个元素比较共有三种情况：>、<和=。如果A[k/2-1]<B[k/2-1]，这表示A[0]到A[k/2-1]的元素都在A和B合并之后的前k小的元素中。换句话说，A[k/2-1]不可能大于两数组合并之后的第k小值，所以我们可以将其抛弃。

证明也很简单，可以采用反证法。假设A[k/2-1]大于合并之后的第k小值，我们不妨假定其为第（k+1）小值。由于A[k/2-1]小于B[k/2-1]，所以B[k/2-1]至少是第（k+2）小值。但实际上，在A中至多存在k/2-1个元素小于A[k/2-1]，B中也至多存在k/2-1个元素小于A[k/2-1]，所以小于A[k/2-1]的元素个数至多有k/2+ k/2-2，小于k，这与A[k/2-1]是第（k+1）的数矛盾。

当A[k/2-1]>B[k/2-1]时存在类似的结论。

当A[k/2-1]=B[k/2-1]时，我们已经找到了第k小的数，也即这个相等的元素，我们将其记为m。由于在A和B中分别有k/2-1个元素小于m，所以m即是第k小的数。(这里可能有人会有疑问，如果k为奇数，则m不是中位数。这里是进行了理想化考虑，在实际代码中略有不同，是先求k/2，然后利用k-k/2获得另一个数。)

通过上面的分析，我们即可以采用递归的方式实现寻找第k小的数。此外我们还需要考虑几个边界条件：

+ 如果A或者B为空，则直接返回B[k-1]或者A[k-1]；
+ 如果k为1，我们只需要返回A[0]和B[0]中的较小值；
+ 如果A[k/2-1]=B[k/2-1]，返回其中一个；

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int total = m + n;
        int k = total / 2;
        if ((total&1) == 1) return findKth(nums1,nums2,k+1,0,m-1,0,n-1);
        else return (findKth(nums1,nums2,k,0,m-1,0,n-1)+findKth(nums1,nums2,k+1,0,m-1,0,n-1))/2.0;
    }
    public double findKth(int A[], int B[], int k, int astart, int aend, int bstart, int bend) {
        int alen = aend - astart + 1;
        int blen = bend - bstart + 1;
        if (alen > blen) 
            return findKth(B,A,k, bstart, bend, astart, aend);
        if (alen == 0) 
            return B[bstart + k - 1];
        if (k == 1) return Math.min(A[astart],B[bstart]);
        
        int sa = Math.min(alen, k/2), sb = k- sa;
        if (A[astart+sa-1] == B[bstart+sb-1]) 
            return A[astart+sa-1];
        else if (A[astart+sa-1] > B[bstart+sb-1]) 
            return findKth(A,B,k - sb,astart,aend,bstart+sb,bend);
        else 
            return findKth(A,B,k - sa,astart+sa,aend,bstart,bend);
    }
}
```