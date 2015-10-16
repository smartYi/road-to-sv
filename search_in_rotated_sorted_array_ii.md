# Search in Rotated Sorted Array II

Follow up for "Search in Rotated Sorted Array":

What if duplicates are allowed?

Would this affect the run-time complexity? How and why?

Write a function to determine if a given target is in the array.

## Solution

Sequence search. O(n)

Since there are duplicates, it's hard to decide which branch to go if binary-search is deployed.
           
```java
public class Solution {
    public boolean search_1(int[] A, int target) {
        for (int i = 0; i < A.length; ++i) {
            if (A[i] == target) return true;
        }
        return false;
    }
    public boolean search(int[] A, int target) {
        int left = 0, right = A.length - 1;
        while (left <= right) {
            int mid = (left + right) /2;
            if (A[mid] == target) return true;
            if (A[left] < A[mid]) {
                if (A[left] <= target && target < A[mid]) right = mid - 1;
                else left = mid + 1;
            } else if (A[left] > A[mid]) {
                if (A[mid] < target && target <= A[right]) left = mid + 1;
                else right = mid - 1;
            } else {
                if (A[left] == A[right]) {
                    ++left; --right;
                } else left = mid + 1;
            }
        }
        return false;
    }
}
```