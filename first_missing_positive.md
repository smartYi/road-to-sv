# First Missing Positive

Given an unsorted integer array, find the first missing positive integer.

For example,

Given [1,2,0] return 3,

and [3,4,-1,1] return 2.

Your algorithm should run in O(n) time and uses constant space.

## Solution

Although we can only use constant space, we can still exchange elements within input A! Swap elements in A and try to make all the elements in A satisfy: A[i] == i + 1. Pick out the first one that does not satisfy A[i] == i + 1.

```java
public class Solution {
    public int firstMissingPositive(int[] A) {
        int n = A.length;
        for(int i=0;i<n;i++){
            while(A[i]>=1&&A[i]<=n&&A[i]!=A[A[i]-1]) {
                int tmp = A[i];
                A[i] = A[tmp - 1];
                A[tmp - 1] = tmp;
            }
        }
        int i=0;
        for(i=0;i<n;i++){
            if(A[i]!=i+1) break;
        }
        return i+1;
    }
}
```